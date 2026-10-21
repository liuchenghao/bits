<script setup>
/**
 * ClickSpark — creates particle spark bursts at click position.
 * Based on vue-bits.dev ClickSpark component (no external deps).
 */
import { onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  count: { type: Number, default: 12 },
  color: { type: String, default: '#a78bfa' },
  spread: { type: Number, default: 40 },
  duration: { type: Number, default: 800 }
})

function onClick(e) {
  const rect = e.currentTarget.getBoundingClientRect()
  const x = e.clientX - rect.left
  const y = e.clientY - rect.top
  spawnSparks(x, y)
}

function spawnSparks(cx, cy) {
  const container = e_target
  const chars = '✨⚡⚡✦⬡⬢●◦'
  for (let i = 0; i < props.count; i++) {
    const el = document.createElement('span')
    el.className = 'spark-particle'
    el.textContent = chars[Math.floor(Math.random() * chars.length)]
    el.style.left = cx + 'px'
    el.style.top = cy + 'px'
    el.style.color = props.color
    el.style.fontSize = (10 + Math.random() * 16) + 'px'
    el.style.setProperty('--tx', (Math.random() - 0.5) * props.spread * 2 + 'px')
    el.style.setProperty('--ty', (Math.random() - 0.5) * props.spread * 2 + 'px')
    el.style.animationDuration = (props.duration + Math.random() * 400) + 'ms'
    document.body.appendChild(el)
    el.addEventListener('animationend', () => el.remove())
  }
}

let e_target

onMounted(() => {
  e_target = document.body
  document.addEventListener('click', (e) => {
    if (e.target.closest('[data-spark-ignore]')) return
    spawnSparks(e.clientX, e.clientY)
  })
})
</script>

<style>
.spark-particle {
  position: fixed;
  pointer-events: none;
  z-index: 9999;
  animation: spark-fly linear forwards;
  transform: translate(0, 0);
  opacity: 1;
}

@keyframes spark-fly {
  0% {
    transform: translate(0, 0) scale(1);
    opacity: 1;
  }
  100% {
    transform: translate(var(--tx), var(--ty)) scale(0.2);
    opacity: 0;
  }
}
</style>
