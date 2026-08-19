<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted, watch } from 'vue'

const paragraphs = [
  'Hello, welcome to my homepage!',
  "I'm Halcyonsong, a Java and AI Full-Stack Engineer.",
  'As a passionate explorer and enthusiastic sharer, I love diving into new technologies and sharing creative insights along the way. Always keeping an eye on the latest trends in AI development, I excel at leveraging modern tools to boost productivity and streamline my workflow.',
  "Currently, I'm transitioning from using outstanding existing frameworks to building my own — with the goal of giving back to the open-source community.",
  'Thanks for stopping by, and happy coding!',
]

interface CharItem {
  char: string
  globalIndex: number
}

const renderedParagraphs = computed(() => {
  let globalIdx = 0
  return paragraphs.map((p) => {
    const chars: CharItem[] = p.split('').map((ch) => {
      const item = { char: ch, globalIndex: globalIdx++ }
      globalIdx++
      return item
    })
    globalIdx++
    return chars
  })
})

const totalChars = computed(() => {
  let count = 0
  for (const p of renderedParagraphs.value) {
    count += p.length
  }
  return count
})

const CHAR_DELAY = 25 // ms per char

const animating = ref(true)
const revealedCount = ref(0)

const props = defineProps<{
  forceSkip?: boolean
}>()

const emit = defineEmits<{
  (e: 'visited'): void
}>()

function skip() {
  if (!animating.value) return
  animating.value = false
  revealedCount.value = totalChars.value
}

let frameId = 0
let startTime = 0
let hasStarted = false

function startAnimation() {
  if (hasStarted) return
  hasStarted = true
  if (props.forceSkip) {
    animating.value = false
    revealedCount.value = totalChars.value
    return
  }
  startTime = performance.now()
  frameId = requestAnimationFrame(tick)
}

function tick() {
  if (!animating.value) return
  const elapsed = performance.now() - startTime
  const count = Math.floor(elapsed / CHAR_DELAY)
  revealedCount.value = Math.min(count, totalChars.value)
  if (revealedCount.value >= totalChars.value) {
    animating.value = false
    emit('visited')
    return
  }
  frameId = requestAnimationFrame(tick)
}

function onWheel(e: WheelEvent) {
  e.preventDefault()
  e.stopPropagation()
  skip()
}

function onClick() {
  skip()
}

onMounted(() => {
  // 如果 forceSkip 为 true（已经访问过），直接显示全部
  if (props.forceSkip) {
    animating.value = false
    revealedCount.value = totalChars.value
    hasStarted = true
  }
  // 否则等待外部触发 start（通过变为 active）
  // 这里用一个 MutationObserver 或 watch 来检测
  // 但更简单的方式：延迟一小段时间检测是否可见
  checkVisibility()
})

let visibilityCheck: number

function checkVisibility() {
  visibilityCheck = window.setInterval(() => {
    if (props.forceSkip) {
      window.clearInterval(visibilityCheck)
      animating.value = false
      revealedCount.value = totalChars.value
      hasStarted = true
      return
    }
    const el = document.querySelector('.about-section')
    if (el) {
      const rect = el.getBoundingClientRect()
      const visible = rect.top < window.innerHeight * 0.5 && rect.bottom > window.innerHeight * 0.5
      if (visible && !hasStarted) {
        startAnimation()
        window.clearInterval(visibilityCheck)
      }
    }
  }, 200)
}

onUnmounted(() => {
  if (frameId) cancelAnimationFrame(frameId)
  if (visibilityCheck) window.clearInterval(visibilityCheck)
})

// 当 forceSkip 变为 true 时（已经访问过），直接显示全部
watch(() => props.forceSkip, (val) => {
  if (val && !hasStarted) {
    animating.value = false
    revealedCount.value = totalChars.value
    hasStarted = true
  }
})
</script>

<template>
  <section class="about-section">
    <h2 class="section-title">About Me</h2>

    <div
      class="card-frame"
      @click="onClick"
      @wheel="onWheel"
    >
      <div class="about-content">
        <p
          v-for="(para, pi) in renderedParagraphs"
          :key="pi"
          class="about-paragraph"
          :class="{ first: pi === 0, last: pi === paragraphs.length - 1 }"
        >
          <span
            v-for="(item, ci) in para"
            :key="ci"
            class="char"
            :class="{ visible: item.globalIndex < revealedCount || !animating }"
          >{{ item.char === ' ' ? '\u00A0' : item.char }}</span>
        </p>
      </div>

      <div v-if="animating" class="skip-hint">
        Click or scroll to skip
      </div>
    </div>
  </section>
</template>

<style scoped>
.about-section {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
}

.section-title {
  font-size: 2rem;
  font-weight: 700;
  letter-spacing: 2px;
  margin-bottom: 24px;
  opacity: 0.9;
}

.card-frame {
  width: 70%;
  max-width: 800px;
  max-height: 72vh;
  overflow-y: auto;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.22);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  box-shadow:
    0 8px 40px rgba(0, 0, 0, 0.4),
    inset 0 1px 0 rgba(255, 255, 255, 0.15);
  padding: 40px 50px;
  cursor: pointer;
  scrollbar-width: none;
}

.card-frame::-webkit-scrollbar {
  display: none;
}

.about-content {
  text-align: center;
}

.about-paragraph {
  font-size: 1.05rem;
  font-weight: 300;
  line-height: 2;
  color: rgba(255, 255, 255, 0.85);
  margin-bottom: 16px;
  letter-spacing: 0.5px;
}

.about-paragraph.first {
  font-size: 1.2rem;
  font-weight: 400;
}

.about-paragraph.last {
  margin-top: 24px;
  font-style: italic;
  opacity: 0.75;
}

.char {
  display: inline-block;
  opacity: 0;
  filter: blur(6px);
  transform: translateY(4px);
  transition:
    opacity 0.4s ease,
    filter 0.4s ease,
    transform 0.4s ease;
}

.char.visible {
  opacity: 1;
  filter: blur(0);
  transform: translateY(0);
}

.skip-hint {
  text-align: center;
  margin-top: 20px;
  font-size: 0.7rem;
  opacity: 0.3;
  letter-spacing: 1px;
  text-transform: uppercase;
}
</style>
