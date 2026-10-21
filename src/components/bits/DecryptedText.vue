<script setup>
/**
 * DecryptedText — hacker-style decryption: cycles random glyphs then resolves to real text.
 * Based on vue-bits.dev DecryptedText component.
 */
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  text: { type: String, required: true },
  speed: { type: Number, default: 50 },
  charSet: {
    type: String,
    default: 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()_+-=[]{}|;:,.<>?/'
  },
  resolveDelay: { type: Number, default: 0 },
  stagger: { type: Number, default: 30 },
  className: { type: String, default: '' }
})

const display = ref(Array.from(props.text).map(() => ' '))
const container = ref(null)
let interval

const CHARS = () => {
  return Array.from(props.charSet)
}

function start() {
  const chars = Array.from(props.text)
  const total = chars.length
  const charPool = CHARS()

  let frame = 0
  const framesPerChar = Math.ceil(props.speed / 10)
  const staggerFrames = Math.ceil(props.stagger / 10)

  interval = setInterval(() => {
    frame++

    display.value = chars.map((c, i) => {
      const resolveAt = i * staggerFrames + framesPerChar
      if (frame >= resolveAt + props.resolveDelay / 10) {
        return c
      } else if (frame > i * staggerFrames) {
        return charPool[Math.floor(Math.random() * charPool.length)]
      } else {
        return ' '
      }
    })

    // Stop when all resolved
    if (display.value.every((c, i) => c === chars[i])) {
      clearInterval(interval)
    }
  }, 10)
}

onMounted(() => {
  const obs = new IntersectionObserver((entries) => {
    entries.forEach(e => {
      if (e.isIntersecting && !interval) {
        start()
        obs.disconnect()
      }
    })
  }, { threshold: 0.3 })
  if (container.value) obs.observe(container.value)
})

onBeforeUnmount(() => {
  clearInterval(interval)
})

</script>

<template>
  <span ref="container" class="decrypted-text" :class="className">
    <template v-for="(ch, i) in display" :key="i">{{ ch || '\u00A0' }}</template>
  </span>
</template>

<style scoped>
.decrypted-text {
  font-family: 'JetBrains Mono', 'Fira Code', monospace;
  white-space: pre-wrap;
}
</style>
