<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import HeroSection from './components/HeroSection.vue'
import AboutSection from './components/AboutSection.vue'
import TimelineSection from './components/TimelineSection.vue'
import Contribution3D from './components/Contribution3D.vue'
import SakuraPetals from './components/SakuraPetals.vue'
import IntroQuote from './components/IntroQuote.vue'

const currentIndex = ref(0)
const isTransitioning = ref(false)
const introDone = ref(false)
const aboutVisited = ref(false)

const sections = [
  HeroSection,
  AboutSection,
  TimelineSection,
  Contribution3D,
]

const TRANSITION_DURATION = 900
let wheelLock = false
let touchStartY = 0

function goTo(index: number) {
  if (isTransitioning.value) return
  if (index < 0 || index >= sections.length) return
  if (index === currentIndex.value) return

  isTransitioning.value = true
  currentIndex.value = index

  setTimeout(() => {
    isTransitioning.value = false
  }, TRANSITION_DURATION)
}

function onWheel(e: WheelEvent) {
  e.preventDefault()
  if (!introDone.value || isTransitioning.value || wheelLock) return

  wheelLock = true
  setTimeout(() => { wheelLock = false }, TRANSITION_DURATION)

  if (e.deltaY > 0) {
    goTo(currentIndex.value + 1)
  } else if (e.deltaY < 0) {
    goTo(currentIndex.value - 1)
  }
}

function onTouchStart(e: TouchEvent) {
  touchStartY = e.touches[0].clientY
}

function onTouchEnd(e: TouchEvent) {
  if (!introDone.value || isTransitioning.value) return
  const delta = touchStartY - e.changedTouches[0].clientY
  if (Math.abs(delta) < 40) return

  if (delta > 0) {
    goTo(currentIndex.value + 1)
  } else {
    goTo(currentIndex.value - 1)
  }
}

function onKeydown(e: KeyboardEvent) {
  if (!introDone.value || isTransitioning.value) return
  if (e.key === 'ArrowDown' || e.key === 'PageDown' || e.key === ' ') {
    e.preventDefault()
    goTo(currentIndex.value + 1)
  } else if (e.key === 'ArrowUp' || e.key === 'PageUp') {
    e.preventDefault()
    goTo(currentIndex.value - 1)
  }
}

onMounted(() => {
  window.addEventListener('wheel', onWheel, { passive: false })
  window.addEventListener('touchstart', onTouchStart, { passive: true })
  window.addEventListener('touchend', onTouchEnd, { passive: true })
  window.addEventListener('keydown', onKeydown)

  setTimeout(() => {
    introDone.value = true
  }, 5200)
})

onUnmounted(() => {
  window.removeEventListener('wheel', onWheel)
  window.removeEventListener('touchstart', onTouchStart)
  window.removeEventListener('touchend', onTouchEnd)
  window.removeEventListener('keydown', onKeydown)
})

function onSlideEnter(i: number) {
  if (i === 1 && !aboutVisited.value) {
    aboutVisited.value = true
  }
}

function onAboutVisited() {
  aboutVisited.value = true
}
</script>

<template>
  <div class="page">
    <div class="bg-layer"></div>

    <IntroQuote v-if="!introDone" />

    <div class="slides-container" :class="{ 'hero-start': introDone }">
      <section
        v-for="(Section, i) in sections"
        :key="i"
        class="slide"
        :class="{ active: i === currentIndex }"
        @transitionend="i === currentIndex && onSlideEnter(i)"
      >
        <component
          :is="Section"
          :forceSkip="i === 1 && aboutVisited"
          @visited="i === 1 && onAboutVisited()"
        />
      </section>
    </div>

    <!-- 分页指示器 -->
    <div class="page-dots" v-if="introDone">
      <button
        v-for="(_, i) in sections"
        :key="i"
        class="dot"
        :class="{ active: i === currentIndex }"
        @click="goTo(i)"
      />
    </div>

    <SakuraPetals />
  </div>
</template>

<style scoped>
.page {
  position: relative;
  min-height: 100vh;
  overflow: hidden;
}

.bg-layer {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: url('/background.jpg') center center / cover no-repeat fixed;
  z-index: -2;
}

.bg-layer::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.65);
  z-index: -1;
}

.slides-container {
  position: relative;
  width: 100%;
  height: 100vh;
  opacity: 0;
  transition: opacity 0s;
}

.slides-container.hero-start {
  opacity: 1;
}

.slide {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  opacity: 0;
  filter: blur(12px);
  transform: scale(1.03);
  transition:
    opacity 0.9s cubic-bezier(0.4, 0, 0.2, 1),
    filter 0.9s cubic-bezier(0.4, 0, 0.2, 1),
    transform 0.9s cubic-bezier(0.4, 0, 0.2, 1);
  pointer-events: none;
  overflow: hidden;
}

.slide.active {
  opacity: 1;
  filter: blur(0);
  transform: scale(1);
  pointer-events: auto;
  z-index: 1;
}

.page-dots {
  position: fixed;
  right: 28px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 12px;
  z-index: 100;
  opacity: 0;
  animation: dotsFadeIn 0.8s ease 0.5s both;
}

@keyframes dotsFadeIn {
  to { opacity: 1; }
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.4);
  background: transparent;
  cursor: pointer;
  transition: all 0.4s ease;
  padding: 0;
}

.dot.active {
  background: rgba(255, 255, 255, 0.8);
  border-color: rgba(255, 255, 255, 0.8);
  box-shadow: 0 0 8px rgba(255, 255, 255, 0.4);
  transform: scale(1.3);
}
</style>
