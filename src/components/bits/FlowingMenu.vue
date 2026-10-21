<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  items: { type: Array, required: true },
  activeIndex: { type: Number, default: 0 },
  /** 指示器颜色 */
  indicatorColor: { type: String, default: 'transparent' },
})
const emit = defineEmits(['change'])

const active = ref(props.activeIndex)

function select(i) {
  active.value = i
  emit('change', i)
}
</script>

<template>
  <nav class="flowing-menu">
    <!-- 液态滑动指示器 -->
    <span
      class="indicator"
      :style="{
        left: `${(active * 100) / items.length}%`,
        width: `${100 / items.length}%`,
        backgroundColor: indicatorColor,
      }"
    />
    <button
      v-for="(item, i) in items"
      :key="i"
      class="flowing-item"
      :class="{ active: active === i }"
      @click="select(i)"
    >
      {{ item }}
    </button>
  </nav>
</template>

<style scoped>
.flowing-menu {
  position: relative;
  display: inline-flex;
  background: rgba(255, 255, 255, 0.6);
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: 999px;
  padding: 4px;
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
}
.indicator {
  position: absolute;
  top: 4px;
  bottom: 4px;
  background: #f97316;
  border-radius: 999px;
  transition: left 0.45s cubic-bezier(0.34, 1.56, 0.64, 1);
  z-index: 0;
}
.flowing-item {
  position: relative;
  z-index: 1;
  padding: 10px 28px;
  font-size: 14px;
  font-weight: 500;
  font-family: inherit;
  color: #374151;
  background: transparent;
  border: none;
  cursor: pointer;
  border-radius: 999px;
  transition: color 0.3s;
}
.flowing-item.active {
  color: white;
}
.flowing-item:hover:not(.active) {
  color: #f97316;
}
</style>
