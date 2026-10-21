<script setup>
import { ref } from 'vue'

const props = defineProps({
  /** 默认是否已激活 */
  active: { type: Boolean, default: false },
  /** 激活时颜色 */
  activeColor: { type: String, default: '#ef4444' },
  /** 尺寸 px */
  size: { type: Number, default: 48 },
})

/**
 * PulseHeart — 脉动爱心
 * 点击→收缩成圆点→换色→膨胀→跳动
 */
const isActive = ref(props.active)

function toggle() {
  isActive.value = !isActive.value
}
</script>

<template>
  <button
    class="pulse-heart"
    :class="{ active: isActive }"
    :style="{ width: size+'px', height: size+'px', '--active-c': activeColor }"
    @click="toggle"
  >
    <svg :width="size * 0.6" :height="size * 0.6" viewBox="0 0 24 24" fill="currentColor">
      <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
    </svg>
  </button>
</template>

<style scoped>
.pulse-heart {
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  border: 2px solid #e5e7eb;
  background: rgba(255,255,255,0.7);
  color: #cbd5e1;
  cursor: pointer;
  transition: all 0.3s;
  animation: none;
}
.pulse-heart:hover {
  border-color: #fca5a5;
  background: #fff1f2;
}
.pulse-heart.active {
  color: var(--active-c);
  border-color: var(--active-c);
  background: color-mix(in srgb, var(--active-c) 10%, white);
  animation: heart-pulse 0.6s ease;
}
@keyframes heart-pulse {
  0% { transform: scale(0.3); }
  50% { transform: scale(1.3); }
  100% { transform: scale(1); }
}
</style>
