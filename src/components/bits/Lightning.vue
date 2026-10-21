<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  /** 闪频率（每秒几次） */
  frequency: { type: Number, default: 3 },
  /** 闪电颜色 */
  color: { type: String, default: '#818cf8' },
  /** 最大分支 */
  maxBranches: { type: Number, default: 5 },
  /** 画布尺寸（正方形，px） */
  size: { type: Number, default: 320 },
})

/**
 * Lightning — 程序化闪电
 * - 每 N 秒产生一次闪电
 * - 闪电从顶部随机位置分叉向下
 * - 闪烁消散
 */
const canvasRef = ref(null)
let raf = 0
let bolt = null
let nextFlash = 0

function genBolt(x0, y0, x1, y1, depth, maxBranches) {
  /** 分叉线段：[ { x0,y0,x1,y1,width } ] */
  if (depth > 4 || maxBranches <= 0) return []
  const midX = (x0 + x1) / 2 + (Math.random() - 0.5) * Math.abs(y1 - y0) * 0.4
  const midY = (y0 + y1) / 2
  const segs = [
    { x0, y0, x1: midX, y1: midY, width: 2.5 * (1 - depth * 0.15) },
    { x0: midX, y0: midY, x1, y1, width: 2.5 * (1 - depth * 0.15) },
  ]
  const out = [...segs]
  // 分叉
  if (Math.random() > 0.5 && maxBranches > 1) {
    const bx = midX + (Math.random() - 0.5) * 60
    const by = midY + Math.random() * 40
    out.push(...genBolt(midX, midY, bx, by, depth + 1, maxBranches - 1))
  }
  if (Math.random() > 0.7 && maxBranches > 2) {
    const bx2 = midX - (Math.random()) * 50
    const by2 = midY + Math.random() * 30
    out.push(...genBolt(midX, midY, bx2, by2, depth + 1, maxBranches - 2))
  }
  return out
}

function flash(time) {
  const cv = canvasRef.value
  if (!cv) return
  const ctx = cv.getContext('2d')
  const W = props.size, H = props.size

  if (time > nextFlash && !bolt) {
    // 新闪电
    const startX = W * 0.2 + Math.random() * W * 0.6
    bolt = {
      segments: genBolt(startX, -10, W / 2, H + 10, 0, props.maxBranches),
      born: time,
      life: 300 + Math.random() * 200,
    }
    nextFlash = time + 1000 / props.frequency + Math.random() * (1000 / props.frequency)
  }

  ctx.clearRect(0, 0, W, H)

  if (bolt) {
    const age = time - bolt.born
    const p = age / bolt.life
    if (p >= 1) {
      bolt = null
    } else {
      const fade = p < 0.1 ? p * 10 : (1 - p) * 1.5
      ctx.lineCap = 'round'
      ctx.lineJoin = 'round'
      // 辉光层
      ctx.strokeStyle = props.color
      ctx.globalAlpha = fade * 0.25
      ctx.lineWidth = 8
      ctx.shadowColor = props.color
      ctx.shadowBlur = 24
      drawSegs(ctx, bolt.segments)
      // 核心
      ctx.globalAlpha = fade
      ctx.lineWidth = 2.5
      ctx.shadowBlur = 12
      drawSegs(ctx, bolt.segments)
      // 白芯
      ctx.strokeStyle = '#fff'
      ctx.globalAlpha = fade * 0.7
      ctx.lineWidth = 1
      drawSegs(ctx, bolt.segments)
      ctx.globalAlpha = 1
      ctx.shadowBlur = 0
    }
  }
}

function drawSegs(ctx, segs) {
  for (const s of segs) {
    ctx.beginPath()
    ctx.moveTo(s.x0, s.y0)
    ctx.lineTo(s.x1, s.y1)
    ctx.stroke()
  }
}

function loop(time) {
  flash(time)
  raf = requestAnimationFrame(loop)
}

onMounted(() => {
  nextFlash = performance.now() + 400
  raf = requestAnimationFrame(loop)
})
onBeforeUnmount(() => cancelAnimationFrame(raf))
</script>

<template>
  <canvas
    ref="canvasRef"
    :width="size"
    :height="size"
    :style="{ width: size + 'px', height: size + 'px' }"
    class="lightning-canvas"
  />
</template>

<style scoped>
.lightning-canvas {
  display: block;
}
</style>
