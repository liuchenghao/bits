<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const props = defineProps({
  auto: { type: Boolean, default: true },
  interval: { type: Number, default: 2500 },
  intensity: { type: Number, default: 0.5 },
  offset: { type: Number, default: 4 },
  color: { type: String, default: 'transparent' },
})

/**
 * GlitchText — RGB 分离故障文字
 * 通过多重 ::before/::after 与 clip 抖动实现
 */
const glitching = ref(false)

const layerStyle = {
  '--glitch-offset': props.offset + 'px',
}

let timer = null

function trigger() {
  glitching.value = true
  setTimeout(() => { glitching.value = false }, 800)
}

function loop() {
  if (!props.auto) return
  trigger()
  timer = setTimeout(loop, props.interval)
}

onMounted(() => { loop() })
onBeforeUnmount(() => { if (timer) clearTimeout(timer) })

defineExpose({ trigger })
</script>

<template>
  <div class="glitch-wrap" :class="{ active: glitching }" :style="layerStyle">
    <slot />
    <!-- 故障层（红） -->
    <span class="glitch-layer r" :style="{ color: '#ff2d55' }" aria-hidden="true">
      <slot />
    </span>
    <!-- 故障层（蓝） -->
    <span class="glitch-layer b" :style="{ color: '#00d4ff' }" aria-hidden="true">
      <slot />
    </span>
  </div>
</template>

<style scoped>
.glitch-wrap {
  position: relative;
  display: inline-block;
  font-family: "SF Pro Display", "Inter", sans-serif;
}
.glitch-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;
  opacity: 0;
  transition: opacity 0.15s;
}
.glitch-wrap.active .glitch-layer {
  opacity: 1;
}
.glitch-layer.r {
  z-index: 1;
}
.glitch-wrap.active .glitch-layer.r {
  animation: glitch-r 0.4s steps(2) infinite;
}
.glitch-layer.b {
  z-index: 2;
}
.glitch-wrap.active .glitch-layer.b {
  animation: glitch-b 0.4s steps(2) infinite reverse;
}

@keyframes glitch-r {
  0% { transform: translate(calc(var(--glitch-offset) * -1), calc(var(--glitch-offset) * 0.5)); clip-path: inset(20% 0 40% 0); }
  25% { transform: translate(var(--glitch-offset), calc(var(--glitch-offset) * -0.3)); clip-path: inset(50% 0 10% 0); }
  50% { transform: translate(calc(var(--glitch-offset) * -0.7), calc(var(--glitch-offset) * 0.8)); clip-path: inset(10% 0 60% 0); }
  75% { transform: translate(var(--glitch-offset), calc(var(--glitch-offset) * 0.2)); clip-path: inset(60% 0 5% 0); }
  100% { transform: translate(calc(var(--glitch-offset) * -1), calc(var(--glitch-offset) * 0.5)); clip-path: inset(20% 0 40% 0); }
}
@keyframes glitch-b {
  0% { transform: translate(calc(var(--glitch-offset) * 1), calc(var(--glitch-offset) * -0.5)); clip-path: inset(40% 0 20% 0); }
  25% { transform: translate(calc(var(--glitch-offset) * -0.8), calc(var(--glitch-offset) * 0.4)); clip-path: inset(10% 0 50% 0); }
  50% { transform: translate(var(--glitch-offset), calc(var(--glitch-offset) * -0.6)); clip-path: inset(55% 0 8% 0); }
  75% { transform: translate(calc(var(--glitch-offset) * -0.5), var(--glitch-offset)); clip-path: inset(5% 0 70% 0); }
  100% { transform: translate(calc(var(--glitch-offset) * 1), calc(var(--glitch-offset) * -0.5)); clip-path: inset(40% 0 20% 0); }
}
</style>
