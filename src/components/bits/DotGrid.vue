<script setup>
/**
 * DotGrid — animated dot grid with cursor interactions.
 * Based on vue-bits.dev DotGrid component.
 */
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  dotSize: { type: Number, default: 2 },
  spacing: { type: Number, default: 40 },
  color: { type: String, default: 'rgba(148, 163, 184, 0.25)' },
  highlightColor: { type: String, default: 'rgba(168, 85, 247, 0.9)' },
  mouseRadius: { type: Number, default: 180 },
  pulse: { type: Boolean, default: true }
})

const canvas = ref(null)
let ctx, raf, dpr
let mx = -9999, my = -9999

function onMouse(e) {
  const r = canvas.value?.getBoundingClientRect()
  if (!r) return
  mx = e.clientX - r.left
  my = e.clientY - r.top
}

function draw() {
  if (!ctx || !canvas.value) return
  const w = canvas.value.width / dpr
  const h = canvas.value.height / dpr

  ctx.clearRect(0, 0, w, h)

  const cols = Math.ceil(w / props.spacing)
  const rows = Math.ceil(h / props.spacing)

  for (let i = 0; i <= cols; i++) {
    for (let j = 0; j <= rows; j++) {
      const x = i * props.spacing + props.spacing / 2
      const y = j * props.spacing + props.spacing / 2

      const dx = x - mx
      const dy = y - my
      const dist = Math.sqrt(dx * dx + dy * dy)

      let size = props.dotSize
      let alpha = 0.25

      if (dist < props.mouseRadius) {
        const t = 1 - dist / props.mouseRadius
        const ease = t * t * (3 - 2 * t)
        size = props.dotSize + ease * props.dotSize * 2.5
        alpha = 0.25 + ease * 0.75
      }

      if (props.pulse) {
        const p = Math.sin(performance.now() * 0.001 + i * 0.3 + j * 0.3) * 0.5 + 0.5
        alpha *= 0.5 + p * 0.5
      }

      const c = dist < props.mouseRadius ? props.highlightColor : props.color
      ctx.globalAlpha = alpha
      ctx.beginPath()
      ctx.arc(x, y, size, 0, Math.PI * 2)
      ctx.fillStyle = c
      ctx.fill()
    }
  }
  ctx.globalAlpha = 1
  raf = requestAnimationFrame(draw)
}

function resize() {
  if (!canvas.value) return
  dpr = Math.min(window.devicePixelRatio || 1, 2)
  canvas.value.width = canvas.value.clientWidth * dpr
  canvas.value.height = canvas.value.clientHeight * dpr
  ctx = canvas.value.getContext('2d')
  ctx.scale(dpr, dpr)
}

onMounted(() => {
  resize()
  window.addEventListener('resize', resize)
  window.addEventListener('mousemove', onMouse, { passive: true })
  draw()
})

onBeforeUnmount(() => {
  cancelAnimationFrame(raf)
  window.removeEventListener('mousemove', onMouse)
  window.removeEventListener('resize', resize)
})

</script>

<template>
  <div class="dot-grid" aria-hidden="true">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<style scoped>
.dot-grid {
  position: absolute;
  inset: 0;
  overflow: hidden;
  pointer-events: none;
}
.dot-grid canvas {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
