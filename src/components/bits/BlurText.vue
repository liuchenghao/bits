<script setup>
/**
 * BlurText — text that animates from blurred/scattered to clear.
 * Based on vue-bits.dev BlurText component.
 */
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'

const props = defineProps({
  text: { type: String, required: true },
  delay: { type: Number, default: 100 },
  duration: { type: Number, default: 1500 },
  stagger: { type: Number, default: 30 },
  triggerOnView: { type: Boolean, default: true }
})

const spans = ref([])
const container = ref(null)
const animating = ref(false)

onMounted(() => {
  const text = props.text
  spans.value = text.split(' ').map((word, wi) =>
    word.split('').map((ch, ci) => ({
      char: ch === ' ' ? '' : ch,
      isSpace: ch === ' ',
      index: wi + ci,
      total: text.length
    }))
  )

  setTimeout(start, props.delay)

  if (props.triggerOnView) {
    const obs = new IntersectionObserver((entries) => {
      entries.forEach(e => { if (e.isIntersecting && !animating.value) start() })
    }, { threshold: 0.3 })
    obs.observe(container.value)
  }
})

function start() {
  if (animating.value) return
  animating.value = true
  const all = spans.value.flat()
  all.forEach((s, i) => {
    const el = document.getElementById(`blur-char-${i}`)
    if (!el) return
    el.style.opacity = '0'
    el.style.filter = 'blur(8px)'
    el.style.transform = `translate(${(Math.random() - 0.5) * 40}px, ${(Math.random() - 0.5) * 40}px)`
    el.style.transition = `all ${props.duration}ms cubic-bezier(0.22, 1, 0.36, 1) ${i * props.stagger}ms`
    requestAnimationFrame(() => {
      requestAnimationFrame(() => {
        el.style.opacity = '1'
        el.style.filter = 'blur(0px)'
        el.style.transform = 'translate(0, 0)'
      })
    })
  })
}

</script>

<template>
  <div ref="container" class="blur-text">
    <template v-for="(word, wi) in spans" :key="wi">
      <span class="word">
        <template v-for="(ch, ci) in word" :key="ci">
          <span
            :id="`blur-char-${spans.slice(0, wi + 1).reduce((a, w) => a + w.length, 0) - 1 + ci}`"
            class="char"
            :style="ci === 0 ? '' : ''"
          >
            {{ ch.isSpace ? '\u00A0' : ch.char }}
          </span>
        </template>
      </span>
      <template v-if="wi < spans.length - 1"><span class="space">&nbsp;</span></template>
    </template>
  </div>
</template>

<style scoped>
.blur-text {
  display: inline;
}
.word {
  display: inline-block;
  white-space: nowrap;
}
.char {
  display: inline-block;
  opacity: 0;
  filter: blur(8px);
  transition: all 1.5s cubic-bezier(0.22, 1, 0.36, 1);
}
</style>
