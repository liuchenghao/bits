<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  items: { type: Array, required: true },
  /** 自动播放 */
  auto: { type: Boolean, default: false },
  /** 间隔 ms */
  interval: { type: Number, default: 2500 },
})
const emit = defineEmits(['change'])

const active = ref(0)
const prev = ref(-1)
let timer = 0

function next() {
  prev.value = active.value
  active.value = (active.value + 1) % props.items.length
  emit('change', active.value)
}

function start() {
  if (!props.auto) return
  timer = setInterval(next, props.interval)
}
function stop() {
  if (timer) clearInterval(timer)
}

defineExpose({ next, stop, active })
</script>

<template>
  <div class="stacked-carousel" @mouseenter="stop" @mouseleave="start">
    <div
      v-for="(item, i) in items"
      :key="i"
      class="sc-card"
      :class="{ active: i === active, leaving: i === prev }"
      :style="{ zIndex: 10 - i }"
      @click="active = i"
    >
      <slot :item="item" :index="i">
        <div class="sc-content">{{ item }}</div>
      </slot>
    </div>
  </div>
</template>

<style scoped>
.stacked-carousel {
  position: relative;
  width: 100%;
  height: 280px;
  perspective: 1000px;
}
.sc-card {
  position: absolute;
  inset: 0;
  margin: auto;
  max-width: 520px;
  height: 240px;
  border-radius: 20px;
  background: rgba(255,255,255,0.78);
  border: 1px solid rgba(99,102,241,0.2);
  backdrop-filter: blur(10px);
  transition: transform 0.6s cubic-bezier(0.34,1.2,0.64,1), opacity 0.5s, box-shadow 0.4s;
  box-shadow: 0 4px 18px rgba(99,102,241,0.08);
  cursor: pointer;
  overflow: hidden;
}
/* 非激活的往后堆叠 */
.sc-card:not(.active):not(.leaving) {
  transform: translateY(40px) scale(0.86);
  opacity: 0.35;
}
.sc-card.active {
  transform: translateY(0) scale(1);
  opacity: 1;
  box-shadow: 0 24px 60px rgba(99,102,241,0.22);
}
.sc-card.leaving {
  transform: translateY(-60px) scale(0.82);
  opacity: 0;
}
.sc-content {
  padding: 32px;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 14px;
  font-size: 18px;
  font-weight: 700;
  color: #1e293b;
  background: linear-gradient(135deg, rgba(238,242,255,0.8), rgba(236,254,255,0.8));
}
</style>
