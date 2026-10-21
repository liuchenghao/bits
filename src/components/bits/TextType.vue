<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  /** 文本 */
  text: { type: String, required: true },
  /** 打字速度 ms/字符 */
  speed: { type: Number, default: 80 },
  /** 是否循环 */
  loop: { type: Boolean, default: true },
  /** 循环停留 ms */
  pause: { type: Number, default: 1500 },
  /** 光标字符 */
  cursorChar: { type: String, default: '|' },
})

/**
 * TextType — 打字机
 * 逐字符显示 + 闪烁光标，可选循环
 */
const displayed = ref('')
const showCursor = ref(true)
let timer = 0
let cursorTimer = 0
let index = 0
let deleting = false

function type() {
  const full = props.text
  if (!deleting) {
    index++
    displayed.value = full.slice(0, index)
    if (index >= full.length) {
      deleting = true
      if (props.loop) {
        timer = setTimeout(() => { deleting = true; type() }, props.pause)
      }
      return
    }
    timer = setTimeout(type, props.speed)
  } else {
    index--
    displayed.value = full.slice(0, index)
    if (index <= 0) {
      deleting = false
      timer = setTimeout(type, props.pause / 3)
      return
    }
    timer = setTimeout(type, props.speed / 2)
  }
}

onMounted(() => {
  type()
  cursorTimer = setInterval(() => { showCursor.value = !showCursor.value }, 400)
})
onBeforeUnmount(() => {
  clearTimeout(timer)
  clearInterval(cursorTimer)
})
</script>

<template>
  <div class="text-type">
    <span class="tt-text">{{ displayed }}</span>
    <span class="tt-cursor" :class="{ blink: !showCursor }">{{ cursorChar }}</span>
  </div>
</template>

<style scoped>
.text-type {
  display: inline-flex;
  align-items: center;
  font-family: "SF Pro Display", "Inter", sans-serif;
}
.tt-text {
  white-space: pre-wrap;
}
.tt-cursor {
  display: inline-block;
  margin-left: 2px;
  color: #6366f1;
  font-weight: 700;
  transition: opacity 0.15s;
}
.tt-cursor.blink {
  opacity: 0;
}
</style>
