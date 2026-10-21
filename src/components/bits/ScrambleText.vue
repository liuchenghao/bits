<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
  text: { type: String, required: true },
  /** 字符集 */
  chars: { type: String, default: 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*░▒▓█▄▀' },
  /** 单字符定位间隔 ms */
  charTime: { type: Number, default: 60 },
  /** 起始延迟 ms */
  startDelay: { type: Number, default: 0 },
  /** 持续乱码翻转间隔 ms（已锁定字符之外） */
  flicker: { type: Number, default: 40 },
})

/**
 * ScrambleText — 乱码→逐字定位
 * 已定位字符固定，未定位字符持续随机翻转
 */
const chars = ref([])
let timers = []

function start() {
  const full = [...props.text]
  const n = full.length
  // 初始化：全部随机
  chars.value = full.map((ch) => ({
    ch,
    cur: props.chars[Math.floor(Math.random() * props.chars.length)],
    done: ch === ' ',
  }))
  // 空格直接锁定

  // 逐个锁定（从左到右）
  full.forEach((ch, i) => {
    const t = props.startDelay + i * props.charTime
    timers.push(setTimeout(() => {
      if (chars.value[i]) {
        chars.value[i].done = true
        chars.value[i].cur = ch
      }
    }, t))
  })

  // 未锁定字符持续翻转
  const interval = setInterval(() => {
    for (const c of chars.value) {
      if (!c.done && c.ch !== ' ') {
        c.cur = props.chars[Math.floor(Math.random() * props.chars.length)]
      }
    }
  }, props.flicker)
  timers.push(interval)
}

onMounted(start)
onUnmounted(() => timers.forEach((t) => { clearTimeout(t); clearInterval(t) }))
</script>

<template>
  <span class="scramble-text">
    <template v-for="(c, i) in chars.length ? chars : []" :key="i">
      <span :style="{ opacity: c.done ? 1 : 0.45, color: c.done ? 'inherit' : '#6366f1', fontWeight: c.done ? 'inherit' : 600 }">{{ c.cur }}</span>
    </template>
    <!-- 未挂载前的占位（避免空白跳动） -->
    <template v-if="!chars.length">{{ text.replace(/\S/g, '\u00a0') }}</template>
  </span>
</template>

<style scoped>
.scramble-text {
  display: inline-block;
  font-family: "SF Pro Display", "Inter", sans-serif;
  font-variant-ligatures: none;
}
</style>
