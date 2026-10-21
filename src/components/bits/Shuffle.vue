<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  /** 文本 */
  text: { type: String, required: true },
  /** 字符集 */
  chars: { type: String, default: 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789#$%&@' },
  /** 单帧间隔 ms */
  tick: { type: Number, default: 40 },
  /** 延迟 ms */
  delay: { type: Number, default: 200 },
  /** 颜色 */
  color: { type: String, default: 'transparent' },
})

/**
 * Shuffle — 字符洗牌揭示
 * 每个字符位置独立翻转，到达目标后锁定
 */
const spans = ref([])
let timers = []

function start() {
  const full = [...props.text]
  spans.value = full.map((ch, i) => ({
    ch,
    current: props.chars[Math.floor(Math.random() * props.chars.length)],
    done: false,
    index: i,
  }))

  // 逐个锁定，间隔错开
  full.forEach((_, i) => {
    const t = props.delay + i * props.tick * 3
    timers.push(setTimeout(() => {
      const s = spans.value[i]
      s.done = true
      s.current = s.ch
    }, t))
  })

  // 未锁定的持续翻转
  const interval = setInterval(() => {
    for (const s of spans.value) {
      if (!s.done) {
        s.current = props.chars[Math.floor(Math.random() * props.chars.length)]
      }
    }
  }, props.tick)
  timers.push(interval)
}

onMounted(start)
onBeforeUnmount(() => {
  timers.forEach(t => { clearTimeout(t); clearInterval(t) })
})
</script>

<template>
  <div class="shuffle-text" :style="{ color: color || 'inherit' }">
    <template v-for="s in spans" :key="s.index">
      <span
        :style="{
          opacity: s.done ? 1 : 0.4,
          color: s.done ? 'inherit' : '#6366f1',
          fontWeight: s.done ? 'inherit' : 600,
        }"
      >{{ s.current }}</span>
    </template>
  </div>
</template>

<style scoped>
.shuffle-text {
  display: inline;
  font-family: "SF Pro Display", "Inter", sans-serif;
}
</style>
