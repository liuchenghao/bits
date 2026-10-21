<script setup>
import { ref } from 'vue'

/**
 * SpotlightCard — 光标位置驱动的高光
 * 用 CSS 变量 --x/--y 传递指针相对位置
 */
const pos = ref({ x: 50, y: 50 })

function onMove(e) {
  const rect = e.currentTarget.getBoundingClientRect()
  pos.value.x = e.clientX - rect.left
  pos.value.y = e.clientY - rect.top
}
</script>

<template>
  <div
    class="spotlight-card"
    :style="{ '--sx': pos.x + 'px', '--sy': pos.y + 'px' }"
    @mousemove="onMove"
  >
    <slot />
  </div>
</template>

<style scoped>
.spotlight-card {
  position: relative;
  background: rgba(255, 255, 255, 0.72);
  border: 1px solid rgba(99, 102, 241, 0.2);
  border-radius: 24px;
  overflow: hidden;
  transition: border-color 0.3s;
  backdrop-filter: blur(10px);
}
.spotlight-card:hover {
  border-color: rgba(99, 102, 241, 0.5);
}
.spotlight-card::before {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(420px circle at var(--sx) var(--sy), rgba(99, 102, 241, 0.22), transparent 65%);
  pointer-events: none;
  transition: opacity 0.3s;
  opacity: 0;
}
.spotlight-card:hover::before {
  opacity: 1;
}
.spotlight-card > * {
  position: relative;
  z-index: 1;
}
</style>
