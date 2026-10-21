<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  items: { type: Array, required: true },
  activeIndex: { type: Number, default: 0 },
})
const emit = defineEmits(['change'])

const active = ref(props.activeIndex)

function select(i) {
  active.value = i
  emit('change', i)
}

const activeW = computed(() => active.value * 100 + '%')
</script>

<template>
  <nav class="gooey-nav">
    <!-- 液体指示器 -->
    <span class="gooey-indicator" :style="insetIndicator" />
    <button
      v-for="(item, i) in items"
      :key="i"
      class="gooey-item"
      :class="{ active: active === i }"
      @click="select(i)"
    >
      <slot :name="`item-${i}`">
        {{ item }}
      </slot>
    </button>
  </nav>
</template>

<script>
// 指示器 inset 定位：用 width + left 让液体"流动"
export default {
  computed: {
    insetIndicator() {
      const n = this.items.length
      return {
        width: `${100 / n}%`,
        left: `${(this.active * 100) / n}%`,
      }
    }
  }
}
</script>

<style scoped>
.gooey-nav {
  position: relative;
  display: inline-flex;
  background: rgba(255, 255, 255, 0.6);
  border: 1px solid rgba(0, 0, 0, 0.06);
  border-radius: 999px;
  padding: 4px;
  gap: 0;
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  filter: contrast(1.2);
}
.gooey-indicator {
  position: absolute;
  top: 4px;
  bottom: 4px;
  background: #6366f1;
  border-radius: 999px;
  transition: left 0.45s cubic-bezier(0.4, 0, 0.2, 1), width 0.45s;
  z-index: 0;
}
.gooey-item {
  position: relative;
  z-index: 1;
  padding: 10px 24px;
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
.gooey-item.active {
  color: white;
}
.gooey-item:hover:not(.active) {
  color: #6366f1;
}
</style>
