<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  /** 线条数量 */
  lines: { type: Number, default: 14 },
  /** 线颜色 */
  color: { type: String, default: 'rgba(99,102,241,0.25)' },
  /** 光标影响半径 */
  radius: { type: Number, default: 220 },
  /** 弯曲强度 */
  strength: { type: Number, default: 40 },
})

/**
 * MagnetLines — 场线向光标弯曲
 * Canvas 画水平线，每段点根据到光标的距离做垂直偏移
 */
const canvasRef = ref(null)
const wrapRef = ref(null)
let raf = 0
let W = 0, H = 0
let mouse = { x: -999, y: -999 }
let target = { x: -999, y: -999 }

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
  target.x = e.clientX - rect.left
  target.y = e.clientY - rect.top
}
function onLeave() { target.x = -999; target.y = -999 }

function draw() {
  // 平滑跟随
  mouse.x += (target.x - mouse.x) * 0.12
  mouse.y += (target.y - mouse.y) * 0.12

  const cv = canvasRef.value
  if (!cv) return
  const ctx = cv.getContext('2d')
  if (!ctx) return
  ctx.clearRect(0, 0, W, H)

  const gap = H / props.lines
  for (let i = 0; i < props.lines; i++) {
    const baseY = gap * i + gap / 2
    ctx.beginPath()
    ctx.strokeStyle = props.color
    ctx.lineWidth = 1.2

    const step = 8
    for (let x = 0; x <= W; x += step) {
      // 该点到光标的垂直距离
      const dx = x - mouse.x
      const dy = baseY - mouse.y
      const dist = Math.hypot(dx, dy)
      // 弯曲量：越近越大
      let bend = 0
      if (dist < props.radius) {
        const f = 1 - dist / props.radius
        // 推向光标方向（垂直分量）
        const dyNorm = dist > 0 ? dy / dist : 0
        bend = -dyNorm * f * f * props.strength
      }
      const y = baseY + bend
      if (x === 0) ctx.moveTo(x, y)
      else ctx.lineTo(x, y)
    }
    ctx.stroke()
  }
  raf = requestAnimationFrame(draw)
}

onMounted(() => {
  resize()
  window.addEventListener('resize', resize)
  canvasRef.value.addEventListener('mousemove', onMouse)
  canvasRef.value.addEventListener('mouseleave', onLeave)
  raf = requestAnimationFrame(draw)
})
onUnmounted(() => {
  cancelAnimationFrame(raf)
  window.removeEventListener('resize', resize)
  if (canvasRef.value) {
    canvasRef.value.removeEventListener('mousemove', onMouse)
    canvasRef.value.removeEventListener('mouseleave', onLeave)
  }
})
</script>

<template>
  <div ref="wrapRef" class="magnet-lines-wrap">
    <canvas ref="canvasRef"></canvas>
  </div>
</template>

<style scoped>
.magnet-lines-wrap {
  width: 100%;
  height: 260px;
  position: relative;
  overflow: hidden;
}
canvas {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
