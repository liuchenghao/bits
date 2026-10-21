<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  /** 按住多长时间触发（ms） */
  duration: { type: Number, default: 1500 },
  /** 标签 */
  label: { type: String, default: 'Hold to Delete' },
  /** 完成标签 */
  doneLabel: { type: String, default: 'Confirm' },
})

/**
 * HoldButton — 长按液态填充按钮
 * - 按住→液面从下上涨
 * - 到达顶部→触发事件
 * - 松开未完成→回弹
 */
const filling = ref(false)
const progress = ref(0)
let raf = 0
let startTime = 0
let emitted = false

function start() {
  if (progress.value >= 1) return
  filling.value = true
  startTime = performance.now()
  emitted = false
  raf = requestAnimationFrame(step)
}

function step(now) {
  if (!filling.value) return
  const p = Math.min(1, (now - startTime) / props.duration)
  progress.value = p
  if (p >= 1) {
    filling.value = false
    // 闪烁确认
    setTimeout(() => {
      progress.value = 0
    }, 300)
    emit('complete')
    return
  }
  raf = requestAnimationFrame(step)
}

function cancel() {
  if (progress.value >= 1) return
  filling.value = false
  cancelAnimationFrame(raf)
  // 回弹
  const start = progress.value
  const startT = performance.now()
  function stepBack(now) {
    const p = Math.min(1, (now - startT) / 300)
    progress.value = start * (1 - p) * (1 - p)
    if (p < 1) raf = requestAnimationFrame(stepBack)
    else progress.value = 0
  }
  raf = requestAnimationFrame(stepBack)
}

const emit = defineEmits(['complete'])

onUnmounted(() => cancelAnimationFrame(raf))
</script>

<template>
  <button
    class="hold-btn"
    @pointerdown.prevent="start"
    @pointerup="cancel"
    @pointerleave="cancel"
  >
    <span class="fill" :style="{ height: progress * 100 + '%' }" />
    <span class="label" :class="{ done: progress >= 1 }">
      {{ progress >= 1 ? doneLabel : (filling ? Math.round(progress * 100) + '%' : label) }}
    </span>
  </button>
</template>

<style scoped>
.hold-btn {
  position: relative;
  overflow: hidden;
  border-radius: 999px;
  border: 1.5px solid rgba(239, 68, 68, 0.4);
  background: rgba(255, 255, 255, 0.7);
  padding: 14px 36px;
  font-size: 15px;
  font-weight: 600;
  color: #374151;
  cursor: pointer;
  font-family: inherit;
  transition: border-color 0.2s;
  user-select: none;
  -webkit-user-select: none;
  touch-action: none;
}
.hold-btn:hover {
  border-color: #ef4444;
}
.fill {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  background: linear-gradient(180deg, #fca5a5 0%, #ef4444 100%);
  transition: none;
  pointer-events: none;
}
.label {
  position: relative;
  z-index: 1;
  display: block;
}
.label.done {
  color: white;
}
</style>
