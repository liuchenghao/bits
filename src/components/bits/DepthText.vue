<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  /** 文字 */
  text: { type: String, required: true },
  /** 层数 */
  layers: { type: Number, default: 8 },
  /** 每层偏移 px */
  depth: { type: Number, default: 3 },
  /** 主色 */
  color: { type: String, default: '#1e293b' },
  /** 挤出色 */
  extrudeColor: { type: String, default: '#c7d2fe' },
})

/**
 * DepthText — 层叠挤出文字
 * 多层相同文字用挤出色错位叠放，鼠标视差
 */
const pos = ref({ x: 0, y: 0 })
let raf = 0
let mouse = { x: 0.5, y: 0.5 }
let target = { x: 0.5, y: 0.5 }

function onMouse(e) {
  target.x = e.clientX / window.innerWidth
  target.y = e.clientY / window.innerHeight
}

onMounted(() => {
  window.addEventListener('mousemove', onMouse, { passive: true })
  const loop = () => {
    mouse.x += (target.x - mouse.x) * 0.08
    mouse.y += (target.y - mouse.y) * 0.08
    pos.value.x = (mouse.x - 0.5) * 40
    pos.value.y = (mouse.y - 0.5) * 40
    raf = requestAnimationFrame(loop)
  }
  loop()
})
onUnmounted(() => {
  cancelAnimationFrame(raf)
  window.removeEventListener('mousemove', onMouse)
})

const layerStyles = ref([])
function build() {
  layerStyles.value = Array.from({ length: props.layers }, (_, i) => {
    const d = (props.layers - i) * props.depth
    return {
      transform: `translate(${d}px, ${d}px)`,
      color: props.extrudeColor,
      opacity: 1 - (i / props.layers) * 0.7,
    }
  })
}
onMounted(build)
</script>

<template>
  <div class="depth-text" :style="{ color, transform: `translate(${pos.x}px, ${pos.y}px)` }">
    <span v-for="(s, i) in layerStyles" :key="i" class="depth-layer" :style="s">{{ text }}</span>
    <span class="depth-front" :style="{ color }">{{ text }}</span>
  </div>
</template>

<style scoped>
.depth-text {
  position: relative;
  display: inline-block;
  font-family: "SF Pro Display", "Inter", sans-serif;
  font-weight: 900;
  will-change: transform;
}
.depth-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;
}
.depth-front {
  position: relative;
  z-index: 1;
}
</style>
