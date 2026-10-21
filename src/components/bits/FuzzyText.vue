<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  text: { type: String, required: true },
  /** 抖动强度 0~1（默认 0.5，0 表示静态） */
  intensity: { type: Number, default: 0.5 },
  /** 是否 hover 触发（默认常开） */
  hoverOnly: { type: Boolean, default: false },
})

/**
 * FuzzyText — 振动模糊文字
 * 高频微小位移 + 抖动，强度可调
 */
const style = ref({})
let raf = 0
let active = !props.hoverOnly

function loop() {
  if (!active || props.intensity === 0) {
    style.value = { transform: '', filter: '' }
    return
  }
  const jx = (Math.random() - 0.5) * props.intensity * 3
  const jy = (Math.random() - 0.5) * props.intensity * 3
  style.value = {
    transform: `translate(${jx}px, ${jy}px)`,
    filter: `blur(${props.intensity * 0.4}px)`,
  }
  raf = requestAnimationFrame(loop)
}

function onEnter() { active = true; loop() }
function onLeave() {
  active = false
  cancelAnimationFrame(raf)
  style.value = { transform: '', filter: '' }
}

onMounted(() => { if (!props.hoverOnly) loop() })
onBeforeUnmount(() => cancelAnimationFrame(raf))
</script>

<template>
  <span class="fuzzy-text" :style="style" @mouseenter="onEnter" @mouseleave="onLeave">
    <slot>{{ text }}</slot>
  </span>
</template>

<style scoped>
.fuzzy-text {
  display: inline-block;
  will-change: transform;
  font-family: "SF Pro Display", "Inter", sans-serif;
}
</style>
