<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  /** 多段文本循环 */
  phrases: { type: Array, required: true },
  /** 单句显示时长 ms */
  duration: { type: Number, default: 2600 },
  /** 切换间隔 ms */
  swapTime: { type: Number, default: 700 },
})

/**
 * RotatingText — 多语句 3D 翻转轮播
 * 当前句 3D 翻出，下一句从背面翻入
 */
const current = ref(0)
const state = ref('idle') // idle | out | in
let timer = 0

function rotate() {
  state.value = 'out'
  timer = setTimeout(() => {
    current.value = (current.value + 1) % props.phrases.length
    state.value = 'in'
    timer = setTimeout(() => {
      state.value = 'idle'
      timer = setTimeout(rotate, props.duration)
    }, props.swapTime)
  }, props.swapTime)
}

onMounted(() => {
  timer = setTimeout(rotate, props.duration)
})
onBeforeUnmount(() => clearTimeout(timer))
</script>

<template>
  <div class="rotating-wrap">
    <transition
      name="flip"
      mode="out-in"
      @enter="state = 'in'"
      @after-leave="state = 'idle'"
    >
      <span :key="current" class="rotating-item">{{ phrases[current] }}</span>
    </transition>
  </div>
</template>

<style scoped>
.rotating-wrap {
  display: inline-block;
  min-height: 1.4em;
  position: relative;
  perspective: 600px;
}
.rotating-item {
  display: inline-block;
  font-family: "SF Pro Display", "Inter", sans-serif;
  font-weight: 800;
  backface-visibility: hidden;
}
.flip-enter-active,
.flip-leave-active {
  transition: all 0.55s cubic-bezier(0.4, 0, 0.2, 1);
}
.flip-enter-from {
  opacity: 0;
  transform: rotateX(80deg) translateY(20px);
}
.flip-leave-to {
  opacity: 0;
  transform: rotateX(-80deg) translateY(-20px);
}
</style>
