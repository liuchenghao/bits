<script setup>
import { ref } from 'vue'

const props = defineProps({
  text: { type: String, required: true },
  /** 颜色 */
  color: { type: String, default: 'transparent' },
  /** 重影数量（默认 3） */
  ghosts: { type: Number, default: 3 },
})

/**
 * EchoText — 重影文字
 * 每次触发时，旧文字副本残留并逐渐变淡/位移
 */
const echoes = ref([])
let tick = 0

function trigger() {
  tick++
  echoes.value = Array.from({ length: props.ghosts }, (_, i) => ({
    text: props.text,
    opacity: 0.35 - i * 0.1,
    dx: (i + 1) * 6,
    key: tick + '-' + i,
  }))
  // 清除旧的
  setTimeout(() => {
    echoes.value = []
  }, 800)
}
</script>

<template>
  <div class="echo-text" :style="{ color: color || 'inherit' }">
    <span
      v-for="e in echoes"
      :key="e.key"
      class="echo-ghost"
      :style="{
        opacity: e.opacity,
        transform: `translateX(${e.dx}px)`,
      }"
      :aria-hidden="true"
    >{{ e.text }}</span>
    <span class="echo-main">{{ text }}</span>
  </div>
</template>

<style scoped>
.echo-text {
  position: relative;
  display: inline-block;
  font-family: "SF Pro Display", "Inter", sans-serif;
}
.echo-ghost {
  position: absolute;
  left: 0;
  top: 0;
  color: #6366f1;
  transition: opacity 0.8s ease, transform 0.8s ease;
  pointer-events: none;
}
.echo-main {
  position: relative;
  z-index: 1;
}
</style>
