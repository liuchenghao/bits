<script setup>
import { ref, computed, onMounted, nextTick } from 'vue'

const props = defineProps({
  text: { type: String, required: true },
  /** 分割单位：char | word */
  unit: { type: String, default: 'char' },
  /** 交错延迟 ms */
  stagger: { type: Number, default: 40 },
  /** 每项动画时长 ms */
  duration: { type: Number, default: 600 },
  /** 触发延迟 */
  delay: { type: Number, default: 0 },
})

/**
 * SplitText — 逐字/逐词交错入场
 * 元素从下方模糊滑入
 */
const parts = computed(() => {
  const t = props.text
  if (props.unit === 'word') return t.split(' ')
  return [...t]
})

const visible = ref(false)

onMounted(async () => {
  await nextTick()
  setTimeout(() => { visible.value = true }, props.delay)
})
</script>

<template>
  <span class="split-text">
    <span
      v-for="(p, i) in parts"
      :key="i"
      class="split-part"
      :class="{ on: visible }"
      :style="{ transitionDelay: i * stagger + 'ms', transitionDuration: duration + 'ms' }"
    >
      {{ p }}<template v-if="unit === 'word' && i < parts.length - 1">&nbsp;</template>
    </span>
  </span>
</template>

<style scoped>
.split-text {
  display: inline;
  font-family: "SF Pro Display", "Inter", sans-serif;
}
.split-part {
  display: inline-block;
  opacity: 0;
  transform: translateY(40px) scale(0.9);
  filter: blur(8px);
  transition-property: opacity, transform, filter;
  transition-timing-function: cubic-bezier(0.16, 1, 0.3, 1);
}
.split-part.on {
  opacity: 1;
  transform: translateY(0) scale(1);
  filter: blur(0);
}
</style>
