<script setup lang="ts">
interface OrbData {
  label: string
  color: string
  angle: number
  distance: number
  coreSize: number
  glowSize: number
  delay: number
  floatDuration: number
  pulseDuration: number
}

// 统一低饱和冷色调，明度差异区分层次
const coreOrbs: OrbData[] = [
  { label: 'Java',         color: '#5b7c99', angle: 8,   distance: 260, coreSize: 20, glowSize: 140, delay: 0.00, floatDuration: 7.5, pulseDuration: 5 },
  { label: 'Spring',       color: '#4a8b6f', angle: 38,  distance: 320, coreSize: 16, glowSize: 120, delay: 0.08, floatDuration: 8,   pulseDuration: 6 },
  { label: 'Spring Boot',  color: '#4a8b6f', angle: 65,  distance: 285, coreSize: 18, glowSize: 130, delay: 0.16, floatDuration: 7,   pulseDuration: 5.5 },
  { label: 'Spring Cloud', color: '#4a8b6f', angle: 95,  distance: 350, coreSize: 16, glowSize: 115, delay: 0.12, floatDuration: 8.5, pulseDuration: 6 },
  { label: 'Spring AI',    color: '#6a5a8c', angle: 122, distance: 300, coreSize: 14, glowSize: 110, delay: 0.20, floatDuration: 7.5, pulseDuration: 5 },
  { label: 'Nacos',        color: '#3a7a9c', angle: 150, distance: 340, coreSize: 15, glowSize: 115, delay: 0.24, floatDuration: 8,   pulseDuration: 6 },
  { label: 'Gateway',      color: '#3a7a9c', angle: 178, distance: 275, coreSize: 14, glowSize: 105, delay: 0.04, floatDuration: 7,   pulseDuration: 5.5 },
  { label: 'Sentinel',     color: '#3a7a9c', angle: 208, distance: 330, coreSize: 14, glowSize: 108, delay: 0.18, floatDuration: 8.5, pulseDuration: 6 },
  { label: 'Seata',        color: '#3a7a9c', angle: 238, distance: 295, coreSize: 13, glowSize: 100, delay: 0.28, floatDuration: 7,   pulseDuration: 5 },
  { label: 'Redis',        color: '#8c5a6a', angle: 268, distance: 360, coreSize: 16, glowSize: 120, delay: 0.10, floatDuration: 8,   pulseDuration: 6 },
  { label: 'MySQL',        color: '#5a7a9c', angle: 298, distance: 310, coreSize: 17, glowSize: 125, delay: 0.22, floatDuration: 7.5, pulseDuration: 5.5 },
  { label: 'SQLite',       color: '#4a6a7c', angle: 328, distance: 325, coreSize: 13, glowSize: 98,  delay: 0.14, floatDuration: 8.5, pulseDuration: 6 },
]

const nonCoreOrbs: OrbData[] = [
  { label: 'C++',       color: '#4a6a8c', angle: 22,  distance: 400, coreSize: 13, glowSize: 95,  delay: 0.35, floatDuration: 8,   pulseDuration: 7 },
  { label: 'Python',    color: '#4a7a8c', angle: 52,  distance: 385, coreSize: 14, glowSize: 100, delay: 0.42, floatDuration: 7.5, pulseDuration: 6.5 },
  { label: 'FastAPI',   color: '#3a8a7a', angle: 108, distance: 420, coreSize: 13, glowSize: 95,  delay: 0.50, floatDuration: 8.5, pulseDuration: 7 },
  { label: 'LangChain', color: '#5a6a8c', angle: 138, distance: 390, coreSize: 12, glowSize: 88,  delay: 0.38, floatDuration: 7,   pulseDuration: 6.5 },
  { label: 'Django',    color: '#3a6a5a', angle: 165, distance: 410, coreSize: 13, glowSize: 90,  delay: 0.46, floatDuration: 8,   pulseDuration: 7 },
  { label: 'ES',        color: '#4a6a7c', angle: 195, distance: 400, coreSize: 12, glowSize: 88,  delay: 0.52, floatDuration: 7.5, pulseDuration: 7 },
  { label: 'Docker',    color: '#3a6a9c', angle: 225, distance: 380, coreSize: 13, glowSize: 92,  delay: 0.40, floatDuration: 8.5, pulseDuration: 6.5 },
  { label: 'Git',       color: '#8c5a4a', angle: 285, distance: 415, coreSize: 13, glowSize: 92,  delay: 0.36, floatDuration: 7,   pulseDuration: 6 },
  { label: 'Linux',     color: '#7a7a4a', angle: 318, distance: 395, coreSize: 12, glowSize: 86,  delay: 0.48, floatDuration: 8,   pulseDuration: 7 },
]
</script>

<template>
  <div class="orbs-container">
    <!-- 核心粒子 -->
    <div
      v-for="(orb, i) in coreOrbs"
      :key="'core-' + i"
      class="orb-wrapper"
      :style="{
        '--angle': orb.angle + 'deg',
        '--distance': orb.distance + 'px',
        '--core-size': orb.coreSize + 'px',
        '--glow-size': orb.glowSize + 'px',
        '--delay': orb.delay + 's',
        '--float-duration': orb.floatDuration + 's',
        '--pulse-duration': orb.pulseDuration + 's',
        '--color': orb.color,
      }"
    >
      <div class="orb-float">
        <div class="orb-particle core-particle">
          <div class="particle-core"></div>
          <span class="orb-label">{{ orb.label }}</span>
        </div>
      </div>
    </div>

    <!-- 非核心粒子 -->
    <div
      v-for="(orb, i) in nonCoreOrbs"
      :key="'non-' + i"
      class="orb-wrapper"
      :style="{
        '--angle': orb.angle + 'deg',
        '--distance': orb.distance + 'px',
        '--core-size': orb.coreSize + 'px',
        '--glow-size': orb.glowSize + 'px',
        '--delay': orb.delay + 's',
        '--float-duration': orb.floatDuration + 's',
        '--pulse-duration': orb.pulseDuration + 's',
        '--color': orb.color,
      }"
    >
      <div class="orb-float">
        <div class="orb-particle non-core-particle">
          <div class="particle-core"></div>
          <span class="orb-label">{{ orb.label }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.orbs-container {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  pointer-events: none;
  z-index: 0;
}

/* ===== 定位层 ===== */
.orb-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  transform: translate(-50%, -50%)
    rotate(var(--angle))
    translateX(var(--distance))
    rotate(calc(-1 * var(--angle)));
  animation: orbPopOut 2s cubic-bezier(0.22, 1, 0.36, 1) calc(var(--delay) + 5.5s) both;
}

@keyframes orbPopOut {
  0% {
    transform: translate(-50%, -50%)
      rotate(var(--angle))
      translateX(0px)
      rotate(calc(-1 * var(--angle)));
    opacity: 0;
  }
  100% {
    transform: translate(-50%, -50%)
      rotate(var(--angle))
      translateX(var(--distance))
      rotate(calc(-1 * var(--angle)));
    opacity: 1;
  }
}

/* ===== 浮动层 ===== */
.orb-float {
  animation: orbFloat var(--float-duration) ease-in-out
    calc(var(--delay) + 7.5s) infinite;
}

@keyframes orbFloat {
  0%, 100% {
    transform: translateY(0) translateX(0);
  }
  25% {
    transform: translateY(-14px) translateX(7px);
  }
  50% {
    transform: translateY(-7px) translateX(-9px);
  }
  75% {
    transform: translateY(-16px) translateX(5px);
  }
}

/* ===== 粒子层 ===== */
.orb-particle {
  position: relative;
  width: var(--glow-size);
  height: var(--glow-size);
  display: flex;
  align-items: center;
  justify-content: center;
  animation: particleFadeIn 2s ease calc(var(--delay) + 5.5s) both;
}

@keyframes particleFadeIn {
  0%   { opacity: 0; scale: 0.3; }
  100% { opacity: 1; scale: 1; }
}

/* 核心：明亮发光点 */
.particle-core {
  width: var(--core-size);
  height: var(--core-size);
  border-radius: 50%;
  background: var(--color);
  animation: corePulse var(--pulse-duration) ease-in-out infinite;
}

/* 核心粒子：光晕大小变化 */
.core-particle .particle-core {
  animation:
    corePulse var(--pulse-duration) ease-in-out infinite,
    coreGlow var(--pulse-duration) ease-in-out infinite;
}

@keyframes corePulse {
  0%, 100% {
    box-shadow:
      0 0 10px 2px var(--color),
      0 0 25px 6px color-mix(in srgb, var(--color) 55%, transparent),
      0 0 50px 12px color-mix(in srgb, var(--color) 35%, transparent);
  }
  50% {
    box-shadow:
      0 0 14px 3px color-mix(in srgb, var(--color) 80%, white),
      0 0 35px 10px color-mix(in srgb, var(--color) 50%, transparent),
      0 0 70px 20px color-mix(in srgb, var(--color) 25%, transparent),
      0 0 100px 30px color-mix(in srgb, var(--color) 12%, transparent);
  }
}

@keyframes coreGlow {
  0%, 100% { scale: 0.9; }
  50%      { scale: 1.15; }
}

/* 非核心粒子：光晕变化更大，有时几乎收缩消失再扩散 */
.non-core-particle .particle-core {
  animation: nonCorePulse var(--pulse-duration) ease-in-out infinite;
}

@keyframes nonCorePulse {
  0%, 100% {
    box-shadow:
      0 0 8px 2px var(--color),
      0 0 20px 5px color-mix(in srgb, var(--color) 50%, transparent),
      0 0 40px 10px color-mix(in srgb, var(--color) 30%, transparent);
    scale: 0.85;
  }
  50% {
    box-shadow:
      0 0 12px 3px color-mix(in srgb, var(--color) 75%, white),
      0 0 30px 8px color-mix(in srgb, var(--color) 45%, transparent),
      0 0 55px 15px color-mix(in srgb, var(--color) 22%, transparent),
      0 0 85px 25px color-mix(in srgb, var(--color) 10%, transparent);
    scale: 1.1;
  }
}

/* 标签 */
.orb-label {
  position: absolute;
  bottom: calc(50% - var(--glow-size) / 2 - 18px);
  font-size: 0.65rem;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.55);
  text-shadow: 0 0 6px rgba(0, 0, 0, 0.9);
  white-space: nowrap;
  letter-spacing: 0.8px;
  text-transform: uppercase;
}
</style>
