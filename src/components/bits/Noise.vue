<script setup>
import { onMounted, onBeforeUnmount } from 'vue'

/**
 * Noise — Canvas 噪点胶片颗粒
 * 动态创建 canvas 叠加到 body
 */
let canvas = null
let styleEl = null
let raf = 0
let frame = 0
let active = false

function resize() {
  if (!canvas) return
  canvas.width = window.innerWidth
  canvas.height = window.innerHeight
  canvas.style.width = window.innerWidth + 'px'
  canvas.style.height = window.innerHeight + 'px'
}

function drawNoise() {
  if (!active || !canvas) return
  const ctx = canvas.getContext('2d')
  if (!ctx) return
  // 每 3 帧刷新一次
  frame++
  if (frame % 3 === 0) {
    ctx.globalAlpha = 0.06
    for (let i = 0; i < 600; i++) {
      const x = Math.random() * canvas.width
      const y = Math.random() * canvas.height
      const g = Math.floor(Math.random() * 100 + 128)
      ctx.fillStyle = `rgba(${g},${g},${g},0.5)`
      ctx.fillRect(x, y, 1.5, 1.5)
    }
    ctx.globalAlpha = 1
  }
  raf = requestAnimationFrame(drawNoise)
}

onMounted(() => {
  canvas = document.createElement('canvas')
  canvas.className = 'noise-canvas'

  styleEl = document.createElement('style')
  styleEl.textContent = '.noise-canvas{position:fixed;inset:0;pointer-events:none;z-index:9998;opacity:0.45;mix-blend-mode:soft-light}'
  document.head.appendChild(styleEl)
  document.body.appendChild(canvas)

  active = true
  resize()
  window.addEventListener('resize', resize)
  raf = requestAnimationFrame(drawNoise)
})

onBeforeUnmount(() => {
  active = false
  cancelAnimationFrame(raf)
  window.removeEventListener('resize', resize)
  if (canvas) canvas.remove()
  if (styleEl) styleEl.remove()
  canvas = null
  styleEl = null
})
</script>

<template>
  <div style="display:none" />
</template>
