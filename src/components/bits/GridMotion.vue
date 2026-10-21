<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  /** 水平线数量 */
  rows: { type: Number, default: 10 },
  /** 竖直格数 */
  cols: { type: Number, default: 14 },
  /** 网格线颜色（深色底上的亮线） */
  color: { type: String, default: '#818cf8' },
  /** 光标影响强度 0~1 */
  strength: { type: Number, default: 0.5 },
})

/**
 * GridMotion — 3D 透视网格（随光标倾斜）
 * 网格面做 rotateX 形成"地平线"透视，光标控制倾斜与消失点偏移
 */
const canvasRef = ref(null)
const wrapRef = ref(null)
let raf = 0
let W = 0, H = 0
let tilt = { x: 0, y: 0 }        // 当前
let tiltTarget = { x: 0, y: 0 }   // 目标（来自光标）
let time = 0

function resize() {
  const cv = canvasRef.value
  const wrap = wrapRef.value
  if (!cv || !wrap) return
  W = wrap.clientWidth || 400
  H = wrap.clientHeight || 240
  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  cv.width = W * dpr
  cv.height = H * dpr
  cv.style.width = W + 'px'
  cv.style.height = H + 'px'
  cv.getContext('2d').setTransform(dpr, 0, 0, dpr, 0, 0)
}

function onMouse(e) {
  const rect = wrapRef.value.getBoundingClientRect()
  const nx = (e.clientX - rect.left) / W - 0.5
  const ny = (e.clientY - rect.top) / H - 0.5
  // 光标越偏，网格倾斜越大
  tiltTarget.x = nx * 0.6 * props.strength * 2
  tiltTarget.y = ny * 0.4 * props.strength * 2
}
function onLeave() { tiltTarget.x = 0; tiltTarget.y = 0 }

function draw() {
  time += 0.016
  // 缓动
  tilt.x += (tiltTarget.x - tilt.x) * 0.06
  tilt.y += (tiltTarget.y - tilt.y) * 0.06

  const cv = canvasRef.value
  if (!cv) return
  const ctx = cv.getContext('2d')
  if (!ctx) return
  ctx.clearRect(0, 0, W, H)

  // 消失点：随水平倾斜偏移
  const vanX = W * 0.5 + tilt.x * W * 0.35
  const vanY = H * 0.28 + tilt.y * H * 0.25
  const horizon = H * 0.42

  ctx.strokeStyle = props.color

  // 水平线（透视：靠近消失点越密，越靠下越疏）
  for (let i = 0; i <= props.rows; i++) {
    const t = i / props.rows
    // 指数分布：0 在消失点附近，1 在底部
    const y = horizon + Math.pow(t, 1.8) * (H - horizon) + (t < 0.02 ? 0 : 0)
    // 上半部分加几条（消失点上方的"天空"网格）
    const skyY = horizon - Math.pow(1 - t, 1.8) * (horizon - vanY)
    const lineY = i <= props.rows / 2 ? skyY : y
    ctx.globalAlpha = 0.1 + t * 0.55
    ctx.lineWidth = 1
    ctx.beginPath()
    ctx.moveTo(0, lineY)
    ctx.lineTo(W, lineY)
    ctx.stroke()
  }

  // 竖直线（从消失点放射到左右两侧）
  const colW = W / props.cols
  for (let i = 0; i <= props.cols; i++) {
    const xBottom = i * colW
    ctx.globalAlpha = 0.18 + Math.abs(i - props.cols / 2) / props.cols * 0.4
    ctx.lineWidth = 1
    ctx.beginPath()
    ctx.moveTo(vanX, vanY)
    ctx.lineTo(xBottom, H)
    ctx.stroke()
    // 上半部分放射到顶部
    ctx.globalAlpha *= 0.6
    ctx.beginPath()
    ctx.moveTo(vanX, vanY)
    ctx.lineTo(xBottom - tilt.y * W * 0.2, 0)
    ctx.stroke()
  }

  // 中央消失点亮点
  ctx.globalAlpha = 0.5
  ctx.fillStyle = props.color
  ctx.beginPath()
  ctx.arc(vanX, vanY, 3, 0, Math.PI * 2)
  ctx.fill()

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
  <div ref="wrapRef" class="grid-motion-wrap">
    <canvas ref="canvasRef"></canvas>
    <span class="grid-motion-hint">移动鼠标倾斜网格</span>
  </div>
</template>

<style scoped>
.grid-motion-wrap {
  position: relative;
  width: 100%;
  height: 280px;
  overflow: hidden;
  background: linear-gradient(180deg, #1e1b4b 0%, #312e81 60%, #4338ca 100%);
  border-radius: 12px;
  border: 1px solid rgba(129,140,248,0.25);
}
.grid-motion-wrap canvas {
  display: block;
  width: 100%;
  height: 100%;
}
.grid-motion-hint {
  position: absolute;
  bottom: 10px;
  right: 14px;
  font-size: 11px;
  color: rgba(199,210,254,0.6);
  letter-spacing: 0.5px;
  pointer-events: none;
}
</style>
