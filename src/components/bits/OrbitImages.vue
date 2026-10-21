<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  /** 数量 */
  count: { type: Number, default: 6 },
  /** 半径 px */
  radius: { type: Number, default: 110 },
  /** 周期 s（转一圈） */
  period: { type: Number, default: 18 },
  /** 是否反向 */
  reverse: { type: Boolean, default: false },
})

/**
 * OrbitImages — 环形旋转
 * 子元素沿圆轨道等距分布，整体匀速旋转
 */
const angle = ref(0)
let raf = 0
let start = 0

function loop(now) {
  if (!start) start = now
  const elapsed = (now - start) / 1000
  angle.value = (elapsed / props.period) * 360 * (props.reverse ? -1 : 1)
  raf = requestAnimationFrame(loop)
}

onMounted(() => { raf = requestAnimationFrame(loop) })
onUnmounted(() => cancelAnimationFrame(raf))

const positions = computed(() => {
  const n = props.count
  const r = props.radius
  const dir = props.reverse ? -1 : 1
  return Array.from({ length: n }, (_, i) => {
    const rad = ((i / n) * 360 * dir + angle.value * dir) * Math.PI / 180
    return {
      x: Math.cos(rad) * r,
      y: Math.sin(rad) * r,
      front: Math.sin(rad) > 0,
    }
  })
})

const boxSize = computed(() => props.radius * 2 + 56)
</script>

<template>
  <div class="orbit-wrap" :style="{ width: boxSize + 'px', height: boxSize + 'px' }">
    <div
      v-for="(p, i) in positions"
      :key="i"
      class="orbit-item"
      :style="{
        transform: `translate(${p.x}px, ${p.y}px) scale(${p.front ? 1.1 : 0.75})`,
        zIndex: p.front ? 2 : 1,
        opacity: p.front ? 1 : 0.55,
      }"
      :class="{ front: p.front }"
    >
      <slot :index="i"><span class="orbit-emoji">{{ ['🌟','🎯','🔮','💎','⚡','🌈','🎲','✨'][i % 8] }}</span></slot>
    </div>
    <div class="orbit-center">
      <slot name="center"><div class="orbit-dot">◆</div></slot>
    </div>
  </div>
</template>

<style scoped>
.orbit-wrap {
  position: relative;
  margin: 0 auto;
}
.orbit-item {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 56px;
  height: 56px;
  margin: -28px 0 0 -28px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255,255,255,0.85);
  border: 1px solid rgba(99,102,241,0.3);
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(99,102,241,0.18);
  backdrop-filter: blur(6px);
  transition: opacity 0.3s;
  will-change: transform;
}
.orbit-emoji { font-size: 28px; }
.orbit-center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 3;
}
.orbit-dot {
  font-size: 30px;
  color: #6366f1;
  text-shadow: 0 0 24px rgba(99,102,241,0.6);
}
</style>
