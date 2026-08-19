<script setup lang="ts">
import { ref, onMounted } from 'vue'

const show = ref(true)
const mounted = ref(false)
const fadingOut = ref(false)

onMounted(() => {
  // 触发逐行淡入
  requestAnimationFrame(() => {
    mounted.value = true
  })
  // 3.8s 后开始整体淡出
  setTimeout(() => {
    fadingOut.value = true
  }, 3800)
  // 5.2s 后完全移除，触发 Hero 动画
  setTimeout(() => {
    show.value = false
  }, 5200)
})
</script>

<template>
  <div
    v-if="show"
    class="intro-overlay"
    :class="{ 'is-mounted': mounted, 'is-fading': fadingOut }"
  >
    <div class="quote-container">
      <p class="quote-line line-1">樱花飘落的速度是秒速五厘米</p>
      <p class="quote-line line-2">那么，我又该以什么样的速度去和你相遇？</p>
    </div>
  </div>
</template>

<style scoped>
.intro-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.10);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  transition:
    opacity 1.4s cubic-bezier(0.4, 0, 0.2, 1),
    backdrop-filter 1.4s cubic-bezier(0.4, 0, 0.2, 1),
    background 1.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.intro-overlay.is-fading {
  opacity: 0;
  backdrop-filter: blur(0);
  -webkit-backdrop-filter: blur(0);
  background: rgba(0, 0, 0, 0);
  pointer-events: none;
}

.quote-container {
  text-align: center;
}

.quote-line {
  font-size: 2.4rem;
  font-weight: 300;
  letter-spacing: 4px;
  color: rgba(255, 255, 255, 0.9);
  margin: 0;
  line-height: 2.2;
  opacity: 0;
  filter: blur(15px);
  transform: translateY(10px);
  transition:
    opacity 1.4s cubic-bezier(0.4, 0, 0.2, 1),
    filter 1.4s cubic-bezier(0.4, 0, 0.2, 1),
    transform 1.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.line-2 {
  padding-left: 2em;
}

/* 挂载后逐行触发淡入 */
.is-mounted .line-1 {
  opacity: 0.9;
  filter: blur(0);
  transform: translateY(0);
  transition-delay: 0.3s;
}

.is-mounted .line-2 {
  opacity: 0.9;
  filter: blur(0);
  transform: translateY(0);
  transition-delay: 1.6s;
}
</style>
