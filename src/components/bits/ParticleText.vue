<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  text: { type: String, required: true },
  /** 粒子数量（默认 800） */
  count: { type: Number, default: 800 },
  /** 粒子颜色 */
  color: { type: String, default: '#6366f1' },
  /** 汇聚时粒子间距 */
  gap: { type: Number, default: 4 },
})

/**
 * ParticleText — Canvas 粒子拼字
 * - 从画布采样文字像素位置
 * - 粒子从随机位置飞向目标位置
 * - 鼠标靠近时粒子被推开
 */
const canvasRef = ref(null)
let W = 0, H = 0
let raf = 0
let active = true
let particles = []
let targets = []
let mouse = { x: -9999, y: -9999 }

function measure() {
  const cv = canvasRef.value
  if (!cv) return
  const rect = cv.parentElement.getBoundingClientRect()
  W = Math.max(100, Math.round(rect.width))
  H = Math.max(50, Math.round(rect.height))
  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  cv.width = W * dpr
  cv.height = H * dpr
  cv.style.width = W + 'px'
  cv.style.height = H + 'px'
  const ctx = cv.getContext('2d')
  if (ctx) ctx.setTransform(1, 0, 0, 1, 0, 0)
  if (ctx) ctx.scale(dpr, dpr)
}

function sampleTargets() {
  const cv = canvasRef.value
  if (!cv) return
  const off = document.createElement('canvas')
  off.width = W
  off.height = H
  const ctx = off.getContext('2d')
  if (!ctx) { targets = []; return }
  ctx.fillStyle = '#000'
  ctx.font = `bold ${Math.min(H * 0.65, W * 0.22)}px "SF Pro Display", "Inter", sans-serif`
  ctx.textAlign = 'center'
  ctx.textBaseline = 'middle'
  ctx.fillText(props.text, W / 2, H / 2)
  let imgData
  try {
    imgData = ctx.getImageData(0, 0, W, H).data
  } catch (e) {
    targets = []
    return
  }

  targets = []
  const step = props.gap
  for (let y = 0; y < H; y += step) {
    for (let x = 0; x < W; x += step) {
      const i = (y * W + x) * 4
      if (imgData[i + 3] > 128) {
        targets.push({ x, y })
      }
    }
  }
}

function initParticles() {
  particles = []
  const n = Math.min(props.count, targets.length)
  for (let i = 0; i < n; i++) {
    const t = targets[i % targets.length]
    particles.push({
      x: Math.random() * W,
      y: Math.random() * H,
      vx: 0, vy: 0,
      tx: t.x,
      ty: t.y,
      size: 0.8 + Math.random() * 1.8,
    })
  }
}

function draw() {
  const cv = canvasRef.value
  if (!cv || !active) return
  const ctx = cv.getContext('2d')
  if (!ctx) return
  ctx.clearRect(0, 0, W, H)

  for (const p of particles) {
    // 弹性向目标移动
    const dx = p.tx - p.x
    const dy = p.ty - p.y
    p.vx += dx * 0.02
    p.vy += dy * 0.02
    p.vx *= 0.82
    p.vy *= 0.82

    // 鼠标排斥
    const mx = p.x - mouse.x
    const my = p.y - mouse.y
    const md = Math.hypot(mx, my)
    if (md < 80) {
      const f = (80 - md) / 80 * 6
      p.vx += (mx / md) * f
      p.vy += (my / md) * f
    }

    p.x += p.vx
    p.y += p.vy

    ctx.fillStyle = props.color
    ctx.globalAlpha = 0.85
    ctx.fillRect(p.x, p.y, p.size, p.size)
  }
  ctx.globalAlpha = 1
  raf = requestAnimationFrame(draw)
}

function onMouse(e) {
  const cv = canvasRef.value
  if (!cv) return
  const rect = cv.getBoundingClientRect()
  mouse.x = e.clientX - rect.left
  mouse.y = e.clientY - rect.top
}
function onLeave() { mouse.x = -9999; mouse.y = -9999 }

let resizeHandler = null
onMounted(() => {
  measure()
  sampleTargets()
  if (targets.length === 0) {
    // 回退：用简单网格生成目标
    targets = []
    for (let y = 20; y < H - 20; y += 12) {
      for (let x = 20; x < W - 20; x += 12) {
        if (Math.random() > 0.7) targets.push({ x, y })
      }
    }
  }
  initParticles()
  const cv = canvasRef.value
  cv.addEventListener('mousemove', onMouse)
  cv.addEventListener('mouseleave', onLeave)
  resizeHandler = () => { measure(); sampleTargets(); initParticles() }
  window.addEventListener('resize', resizeHandler)
  raf = requestAnimationFrame(draw)
})

onUnmounted(() => {
  active = false
  cancelAnimationFrame(raf)
  const cv = canvasRef.value
  if (cv) {
    cv.removeEventListener('mousemove', onMouse)
    cv.removeEventListener('mouseleave', onLeave)
  }
  if (resizeHandler) window.removeEventListener('resize', resizeHandler)
})
</script>

<template>
  <div class="particle-text-wrap">
    <canvas ref="canvasRef"></canvas>
  </div>
</template>

<style scoped>
.particle-text-wrap {
  width: 100%;
  aspect-ratio: 2 / 1;
  position: relative;
}
canvas {
  width: 100%;
  height: 100%;
  display: block;
}
</style>
