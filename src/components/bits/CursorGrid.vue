<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  /** 行数 */
  rows: { type: Number, default: 8 },
  /** 列数 */
  cols: { type: Number, default: 14 },
  /** 基础色 */
  color: { type: String, default: '#cbd5e1' },
  /** 点亮颜色 */
  activeColor: { type: String, default: '#6366f1' },
  /** 光标影响半径 */
  radius: { type: Number, default: 160 },
})

/**
 * CursorGrid — 网格单元随光标点亮
 */
const canvasRef = ref(null)
const wrapRef = ref(null)
let raf = 0
let W = 0, H = 0
let mouse = { x: -999, y: -999 }

function resize() {
  const cv = canvasRef.value
  const wrap = wrapRef.value
  if (!cv || !wrap) return
  W = wrap.clientWidth || 100
  H = wrap.clientHeight || 100
  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  cv.width = W * dpr
  cv.height = H * dpr
  cv.style.width = W + 'px'
  cv.style.height = H + 'px'
  cv.getContext('2d').setTransform(dpr, 0, 0, dpr, 0, 0)
}

function onMouse(e) {
  const rect = canvasRef.value.getBoundingClientRect()
  mouse.x = e.clientX - rect.left
  mouse.y = e.clientY - rect.top
}
function onLeave() { mouse.x = -999; mouse.y = -999 }

function draw() {
  const cv = canvasRef.value
  if (!cv) return
  const ctx = cv.getContext('2d')
  if (!ctx) return
  ctx.clearRect(0, 0, W, H)

  const cellW = W / props.cols
  const cellH = H / props.rows
  const dotR = 2.5

  for (let r = 0; r < props.rows; r++) {
    for (let c = 0; c < props.cols; c++) {
      const x = c * cellW + cellW / 2
      const y = r * cellH + cellH / 2
      const dist = Math.hypot(x - mouse.x, y - mouse.y)
      let alpha = 0.15
      let color = props.color
      let size = dotR
      if (dist < props.radius) {
        const f = 1 - dist / props.radius
        alpha = 0.15 + f * 0.85
        color = props.activeColor
        size = dotR + f * 3
      }
      ctx.fillStyle = color
      ctx.globalAlpha = alpha
      ctx.beginPath()
      ctx.arc(x, y, size, 0, Math.PI * 2)
      ctx.fill()
    }
  }
  ctx.globalAlpha = 1
  raf = requestAnimationFrame(draw)
}

onMounted(() => {
  resize()
  window.addEventListener('resize', resize)
  wrapRef.value.addEventListener('mousemove', onMouse)
  wrapRef.value.addEventListener('mouseleave', onLeave)
  raf = requestAnimationFrame(draw)
})
onUnmounted(() => {
  cancelAnimationFrame(raf)
  window.removeEventListener('resize', resize)
  if (wrapRef.value) {
    wrapRef.value.removeEventListener('mousemove', onMouse)
    wrapRef.value.removeEventListener('mouseleave', onLeave)
  }
})
</script>

<template>
  <div ref="wrapRef" class="cursor-grid-wrap">
    <canvas ref="canvasRef"></canvas>
  </div>
</template>

<style scoped>
.cursor-grid-wrap {
  width: 100%;
  height: 240px;
  position: relative;
  overflow: hidden;
  background: rgba(255,255,255,0.5);
  border-radius: 12px;
}
canvas {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
