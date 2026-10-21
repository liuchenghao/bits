<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  arcCount: { type: Number, default: 4 },
  color: { type: String, default: '#818cf8' },
  sparkColor: { type: String, default: '#c7d2fe' },
  intensity: { type: Number, default: 0.5 },
  radius: { type: Number, default: 24 },
})

/**
 * ElectricBorder — 电弧沿矩形边框流动
 */
const canvasRef = ref(null)
const wrapRef = ref(null)
let raf = 0
let W = 0, H = 0
let arcs = []

function resize() {
  const wrap = wrapRef.value
  const cv = canvasRef.value
  if (!wrap || !cv) return
  W = wrap.clientWidth || 100
  H = wrap.clientHeight || 100
  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  cv.width = W * dpr
  cv.height = H * dpr
  cv.style.width = W + 'px'
  cv.style.height = H + 'px'
  const ctx = cv.getContext('2d')
  ctx.scale(dpr, dpr)
}

/**
 * 沿圆角矩形周界取点 t∈[0,1)
 * 路径顺序：左上→顶部水平→右上弧→右侧垂直→右下弧→底部水平→左下弧→左侧垂直→左上弧
 */
function pathPoint(t) {
  const r = Math.min(props.radius, Math.min(W, H) / 2)
  const w = W, h = H
  const segH = w - 2 * r  // 水平段
  const segV = h - 2 * r  // 垂直段
  const segA = Math.PI * r / 2  // 90° 圆弧

  const total = 2 * segH + 2 * segV + 4 * segA
  let d = ((t % 1) + 1) % 1 * total
  let x = 0, y = 0

  if (d < segH) {
    // 顶部水平：左上(r,0)→右上(w-r,0)
    x = r + d; y = 0
  } else if ((d -= segH) < segA) {
    // 右上弧
    const a = (d / segA) * (Math.PI / 2)
    x = w - r + r * Math.sin(a); y = r - r * Math.cos(a)
  } else if ((d -= segA) < segV) {
    // 右侧垂直：右上→右下
    x = w; y = r + d
  } else if ((d -= segV) < segA) {
    // 右下弧
    const a = (d / segA) * (Math.PI / 2)
    x = w - r + r * Math.cos(a); y = h - r + r * Math.sin(a)
  } else if ((d -= segA) < segH) {
    // 底部水平：右下→左下
    x = w - r - d; y = h
  } else if ((d -= segH) < segA) {
    // 左下弧
    const a = (d / segA) * (Math.PI / 2)
    x = r - r * Math.cos(a); y = h - r + r * Math.sin(a)
  } else if ((d -= segA) < segV) {
    // 左侧垂直：左下→左上
    x = 0; y = h - r - d
  } else {
    // 左上弧
    d -= segV
    const a = (d / segA) * (Math.PI / 2)
    x = r - r * Math.cos(a); y = r - r * Math.sin(a)
  }
  return { x, y }
}

function initArcs() {
  arcs = []
  for (let i = 0; i < props.arcCount; i++) {
    arcs.push({
      t: i / props.arcCount,
      speed: 0.0012 + Math.random() * 0.002,
      len: 0.04 + Math.random() * 0.05,
      phase: Math.random() * 100,
      amp: 0.5 + Math.random(),
    })
  }
}

function drawFrame(time) {
  const cv = canvasRef.value
  if (!cv) return
  const ctx = cv.getContext('2d')
  if (!ctx) return
  ctx.clearRect(0, 0, W, H)
  const sec = time * 0.001

  for (const a of arcs) {
    a.t = (a.t + a.speed) % 1
    const headT = a.t
    const steps = 14

    // 尾迹（从旧到新）
    ctx.lineCap = 'round'
    let prev = null
    for (let s = steps; s >= 0; s--) {
      const t = headT - (s / steps) * a.len
      const p = pathPoint(t)
      const jx = (Math.sin(sec * 0.6 + s * 0.5 + a.phase) + Math.sin(sec * 0.4 + s * 0.3)) * 8 * a.amp * props.intensity
      const jy = (Math.cos(sec * 0.5 + s * 0.4 + a.phase) + Math.sin(sec * 0.7 + s * 0.2)) * 8 * a.amp * props.intensity
      const px = p.x + jx
      const py = p.y + jy
      const fade = 1 - s / (steps + 1)

      if (prev) {
        ctx.strokeStyle = props.color
        ctx.globalAlpha = fade * 0.45
        ctx.lineWidth = 2.5 * fade + 0.4
        ctx.beginPath()
        ctx.moveTo(prev.x, prev.y)
        ctx.lineTo(px, py)
        ctx.stroke()
      }

      if (s === 0) {
        // 头部火花
        const glow = ctx.createRadialGradient(px, py, 0, px, py, 12)
        glow.addColorStop(0, props.sparkColor)
        glow.addColorStop(0.5, props.color + 'aa')
        glow.addColorStop(1, props.color + '00')
        ctx.fillStyle = glow
        ctx.globalAlpha = 0.9
        ctx.beginPath()
        ctx.arc(px, py, 12, 0, Math.PI * 2)
        ctx.fill()
      }
      prev = { x: px, y: py }
    }
    ctx.globalAlpha = 1
  }
}

function loop(time) {
  drawFrame(time)
  raf = requestAnimationFrame(loop)
}

onMounted(() => {
  resize()
  initArcs()
  window.addEventListener('resize', resize)
  raf = requestAnimationFrame(loop)
})

onUnmounted(() => {
  cancelAnimationFrame(raf)
  window.removeEventListener('resize', resize)
})
</script>

<template>
  <div ref="wrapRef" class="electric-wrap">
    <canvas ref="canvasRef" class="electric-canvas"></canvas>
    <slot />
  </div>
</template>

<style scoped>
.electric-wrap {
  position: relative;
  display: block;
}
.electric-canvas {
  position: absolute;
  inset: -3px;
  width: calc(100% + 6px);
  height: calc(100% + 6px);
  pointer-events: none;
  z-index: 10;
}
</style>
