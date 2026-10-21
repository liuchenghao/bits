<script setup>
/**
 * Counter — animated count-up number.
 * Based on vue-bits.dev Counter component.
 */
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'

const props = defineProps({
  from: { type: Number, default: 0 },
  to: { type: Number, default: 100 },
  duration: { type: Number, default: 2000 },
  suffix: { type: String, default: '' },
  prefix: { type: String, default: '' }
})

const value = ref(props.from)
let raf, startTime

function animate(ts) {
  if (!startTime) startTime = ts
  const p = Math.min((ts - startTime) / props.duration, 1)
  const eased = 1 - Math.pow(1 - p, 3)
  value.value = Math.round(props.from + (props.to - props.from) * eased)
  if (p < 1) raf = requestAnimationFrame(animate)
}

onMounted(() => {
  raf = requestAnimationFrame(animate)
})

onBeforeUnmount(() => cancelAnimationFrame(raf))

</script>

<template>
  <span class="counter">{{ prefix }}{{ value }}{{ suffix }}</span>
</template>

<style scoped>
.counter {
  font-variant-numeric: tabular-nums;
}
</style>
