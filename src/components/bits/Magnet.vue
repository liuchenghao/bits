<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  /** 影响半径 px */
  radius: { type: Number, default: 240 },
  /** 最大吸附距离 px */
  strength: { type: Number, default: 60 },
})

/**
 * Magnet — 磁性元素
 * 子元素在鼠标靠近时向光标偏移，离开后回弹
 */
const el = ref(null)
let raf = 0
const target = { x: 0, y: 0 }
const pos = { x: 0, y: 0 }

function onMove(e) {
  const rect = el.value.getBoundingClientRect()
  const cx = rect.left + rect.width / 2
  const cy = rect.top + rect.height / 2
  const dx = e.clientX - cx
  const dy = e.clientY - cy
  const dist = Math.hypot(dx, dy)
  if (dist < props.radius) {
    const f = 1 - dist / props.radius
    target.x = (dx / dist) * f * props.strength
    target.y = (dy / dist) * f * props.strength
  } else {
    target.x = 0
    target.y = 0
  }
  tick()
}

function tick() {
  cancelAnimationFrame(raf)
  const step = () => {
    pos.x += (target.x - pos.x) * 0.18
    pos.y += (target.y - pos.y) * 0.18
    if (el.value) {
      el.value.style.transform = `translate(${pos.x}px, ${pos.y}px)`
    }
    if (Math.abs(target.x - pos.x) + Math.abs(target.y - pos.y) > 0.4) {
      raf = requestAnimationFrame(step)
    }
  }
  raf = requestAnimationFrame(step)
}

onMounted(() => {
  window.addEventListener('mousemove', onMove, { passive: true })
})
onUnmounted(() => {
  cancelAnimationFrame(raf)
  window.removeEventListener('mousemove', onMove)
})
</script>

<template>
  <div ref="el" class="magnet" style="transition: transform 0.18s ease-out;">
    <slot />
  </div>
</template>

<style scoped>
.magnet {
  display: inline-block;
  will-change: transform;
}
</style>
