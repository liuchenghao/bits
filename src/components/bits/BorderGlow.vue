<script setup>
/**
 * BorderGlow — glowing mesh-gradient border that tracks cursor and intensifies near edges.
 * Based on vue-bits.dev BorderGlow component.
 */
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  animated: { type: Boolean, default: false },
  edgeSensitivity: { type: Number, default: 30 },
  glowColor: { type: String, default: '168 85 247' },
  backgroundColor: { type: String, default: '#060010' },
  borderRadius: { type: Number, default: 28 },
  glowRadius: { type: Number, default: 40 },
  glowIntensity: { type: Number, default: 1 },
  coneSpread: { type: Number, default: 25 },
  colors: { type: Array, default: () => ['#c084fc', '#f472b6', '#38bdf8'] },
  className: { type: String, default: '' }
})

const card = ref(null)
const angle = ref(0)
const hovering = ref(false)
const nearEdge = ref(0)
let sweepRaf = null
let lastEvt = null

function updateFromEvt(e) {
  if (!card.value) return
  const r = card.value.getBoundingClientRect()
  const cx = e.clientX - r.left
  const cy = e.clientY - r.top

  const x = cx - r.width / 2
  const y = cy - r.height / 2
  angle.value = (Math.atan2(y, x) * 180 / Math.PI + 90 + 360) % 360

  const edgeDist = Math.min(
    cx / r.width,
    (r.width - cx) / r.width,
    cy / r.height,
    (r.height - cy) / r.height
  )
  const sens = Math.max(1, props.edgeSensitivity)
  nearEdge.value = Math.max(0, Math.min(1, 1 - edgeDist * (100 / sens) * 2.5))
}

function onPointerMove(e) {
  lastEvt = e
  updateFromEvt(e)
}

function onPointerEnter() {
  hovering.value = true
  if (lastEvt) updateFromEvt(lastEvt)
}

function onPointerLeave() {
  hovering.value = false
  nearEdge.value = 0
}

function startSweep() {
  if (sweepRaf) cancelAnimationFrame(sweepRaf)
  let start = null
  const dur = 2200
  function tick(ts) {
    if (!start) start = ts
    const p = Math.min((ts - start) / dur, 1)
    angle.value = p * 360
    if (p < 1) sweepRaf = requestAnimationFrame(tick)
    else { sweepRaf = null; start = null }
  }
  sweepRaf = requestAnimationFrame(tick)
}

onMounted(() => { if (props.animated) startSweep() })
onBeforeUnmount(() => { if (sweepRaf) cancelAnimationFrame(sweepRaf) })

// Computed styles
const borderLayerStyle = computed(() => {
  const c0 = props.colors[0] || '#c084fc'
  const c1 = props.colors[1] || c0
  const c2 = props.colors[2] || c1
  const cs = props.coneSpread
  const deg = angle.value
  return {
    background: `conic-gradient(
      from ${deg}deg at 50% 50%,
      transparent 0deg,
      ${c0} ${(100 - cs * 2) * 3.6}deg,
      ${c1} ${(100 - cs) * 3.6}deg,
      ${c2} ${(100 - cs + 8) * 3.6}deg,
      transparent ${(100 - cs * 2 + 16) * 3.6}deg
    )`,
    opacity: 0.3 + nearEdge.value * 0.7
  }
})

const glowLayerStyle = computed(() => {
  const hsl = props.glowColor
  const ne = nearEdge.value
  const gi = props.glowIntensity
  return {
    boxShadow: `
      0 0 ${props.glowRadius * 0.7}px ${ne * 20}px rgba(${hsl}, ${ne * 0.7 * gi}),
      0 0 ${props.glowRadius * 0.4}px ${ne * 10}px rgba(${hsl}, ${ne * 0.4 * gi})
    `
  }
})

const rootStyle = computed(() => ({
  borderRadius: props.borderRadius + 'px',
  background: props.backgroundColor,
  '--card-bg': props.backgroundColor
}))

</script>

<template>
  <div
    ref="card"
    class="bg-card"
    :class="className"
    :style="rootStyle"
    @pointermove="onPointerMove"
    @pointerenter="onPointerEnter"
    @pointerleave="onPointerLeave"
  >
    <div class="bg-glow" :style="glowLayerStyle" aria-hidden="true"></div>
    <div class="bg-border" :style="borderLayerStyle" aria-hidden="true"></div>
    <div class="bg-content" :style="{ borderRadius: (props.borderRadius - 2) + 'px' }">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.bg-card {
  position: relative;
  padding: 3px;
  cursor: default;
}

.bg-glow {
  position: absolute;
  inset: 0;
  border-radius: inherit;
  pointer-events: none;
  transition: box-shadow 0.3s ease;
}

.bg-border {
  position: absolute;
  inset: 0;
  border-radius: inherit;
  pointer-events: none;
  transition: opacity 0.3s ease;
}

.bg-border::after {
  content: '';
  position: absolute;
  inset: 3px;
  border-radius: inherit;
  background: var(--card-bg, #fff);
}

.bg-content {
  position: relative;
  z-index: 1;
  overflow: hidden;
  min-height: 100%;
}
</style>
