<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  text: { type: String, required: true },
  /** 光泽颜色 */
  sheenColor: { type: String, default: 'linear-gradient(90deg, transparent 30%, #fff 50%, transparent 70%)' },
  /** 循环间隔 */
  interval: { type: Number, default: 3000 },
})

/**
 * ShinyText — 金属光泽扫过
 * 用 background-clip:text + 动画位移实现
 */
const style = computed(() => ({
  backgroundImage: props.sheenColor,
}))
</script>

<template>
  <span class="shiny-text" :style="style"><slot>{{ text }}</slot></span>
</template>

<style scoped>
.shiny-text {
  display: inline-block;
  background-size: 200% auto;
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  animation: shiny-sweep 2.5s linear infinite;
  font-weight: 800;
}
@keyframes shiny-sweep {
  0% { background-position: 200% center; }
  100% { background-position: -200% center; }
}
</style>
