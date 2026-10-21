<script setup>
/**
 * Aurora — OGL WebGL flowing aurora background
 * Source: vue-bits.dev (OGL simplex noise shader)
 */
import { onMounted, onBeforeUnmount, ref, shallowRef } from 'vue'

const props = defineProps({
  colorStops: {
    type: Array,
    default: () => ['#171D22', '#7cff67', '#171D22']
  },
  amplitude: { type: Number, default: 1.0 },
  blend: { type: Number, default: 0.5 },
  speed: { type: Number, default: 1.0 }
})

const container = ref(null)
const glCanvas = shallowRef(null)

let mouse = { x: 0.5, y: 0.5 }
let raf, startTime = 0

function hexToRgb(hex) {
  const h = hex.replace('#', '')
  return [
    parseInt(h.slice(0, 2), 16) / 255,
    parseInt(h.slice(2, 4), 16) / 255,
    parseInt(h.slice(4, 6), 16) / 255
  ]
}

const VERTEX = `
attribute vec2 position;
void main() {
  gl_Position = vec4(position, 0.0, 1.0);
}
`

const FRAGMENT = `
precision highp float;
uniform float uTime;
uniform vec2 uRes;
uniform vec3 uColorA;
uniform vec3 uColorB;
uniform vec3 uColorC;
uniform float uAmplitude;
uniform float uBlend;
uniform vec2 uMouse;

// 经典 3D value noise（SwiftShader 兼容）
float hash(float n) { return fract(sin(n) * 43758.5453123); }
float noise3(vec3 x) {
  vec3 p = floor(x);
  vec3 f = fract(x);
  f = f * f * (3.0 - 2.0 * f);
  float n000 = hash(dot(p + vec3(0,0,0), vec3(127.1, 311.7, 74.7)));
  float n100 = hash(dot(p + vec3(1,0,0), vec3(127.1, 311.7, 74.7)));
  float n010 = hash(dot(p + vec3(0,1,0), vec3(127.1, 311.7, 74.7)));
  float n110 = hash(dot(p + vec3(1,1,0), vec3(127.1, 311.7, 74.7)));
  float n001 = hash(dot(p + vec3(0,0,1), vec3(127.1, 311.7, 74.7)));
  float n101 = hash(dot(p + vec3(1,0,1), vec3(127.1, 311.7, 74.7)));
  float n011 = hash(dot(p + vec3(0,1,1), vec3(127.1, 311.7, 74.7)));
  float n111 = hash(dot(p + vec3(1,1,1), vec3(127.1, 311.7, 74.7)));
  float v0 = mix(n000, n100, f.x);
  float v1 = mix(n010, n110, f.x);
  float v2 = mix(n001, n101, f.x);
  float v3 = mix(n011, n111, f.x);
  return mix(mix(v0, v1, f.y), mix(v2, v3, f.y), f.z);
}
float fbm(vec3 p) {
  float v = 0.0; float a = 0.5;
  for (int i = 0; i < 3; i++) { v += a * noise3(p); p *= 2.0; a *= 0.5; }
  return v;
}

void main() {
  vec2 uv = gl_FragCoord.xy / uRes;
  float t = uTime;

  // 流动噪声场
  vec3 p = vec3(uv * vec2(uRes.x / uRes.y, 1.0) * 1.5, t * 0.15);
  float n = fbm(p);

  // 极光带：以 y 方向主色 + 噪声扰动
  float band = sin(uv.y * 5.0 + n * 4.0 + t * 0.25) * uBlend + 0.5;
  band = smoothstep(0.0, 1.0, band);

  // 鼠标增强
  float mx = 1.0 - smoothstep(0.0, 0.8, distance(uv, uMouse));
  band = clamp(band + mx * 0.35, 0.0, 1.0);

  vec3 color = mix(uColorA, uColorB, band);
  color = mix(color, uColorC, n * 0.4);

  // 辉光带（加强，让极光更亮）
  float glow = pow(band, 2.5) * 0.9;
  color += vec3(glow * 0.5, glow * 0.9, glow * 1.2);

  // 暗角（弱化，避免中心过暗）
  float vig = 1.0 - smoothstep(0.55, 1.3, length(uv - 0.5)) * 0.6;
  color *= vig;

  gl_FragColor = vec4(color, 1.0);
}
`

function initGL(canvas) {
  const gl = canvas.getContext('webgl', { preserveDrawingBuffer: true }) || canvas.getContext('experimental-webgl', { preserveDrawingBuffer: true })
  if (!gl) {
    console.warn('Aurora: WebGL not supported')
    return null
  }

  function makeShader(type, src) {
    const s = gl.createShader(type)
    gl.shaderSource(s, src)
    gl.compileShader(s)
    if (!gl.getShaderParameter(s, gl.COMPILE_STATUS)) {
      console.error('Aurora shader compile error:', gl.getShaderInfoLog(s))
    }
    return s
  }

  const prog = gl.createProgram()
  gl.attachShader(prog, makeShader(gl.VERTEX_SHADER, VERTEX))
  gl.attachShader(prog, makeShader(gl.FRAGMENT_SHADER, FRAGMENT))
  // 固定 attribute 位置，避免被优化掉
  gl.bindAttribLocation(prog, 0, 'position')
  gl.linkProgram(prog)
  if (!gl.getProgramParameter(prog, gl.LINK_STATUS)) {
    console.error('Aurora program link failed:', gl.getProgramInfoLog(prog))
    return null
  }
  gl.useProgram(prog)

  // 全屏大三角（覆盖整个视口，3 个顶点即可）
  const buf = gl.createBuffer()
  gl.bindBuffer(gl.ARRAY_BUFFER, buf)
  gl.bufferData(gl.ARRAY_BUFFER, new Float32Array([
    -1, -1, 3, -1, -1, 3
  ]), gl.STATIC_DRAW)
  gl.enableVertexAttribArray(0)
  gl.vertexAttribPointer(0, 2, gl.FLOAT, false, 0, 0)

  gl.disable(gl.DEPTH_TEST)
  gl.disable(gl.BLEND)

  return { gl, prog }
}

function renderFrame(gl, prog, canvas) {
  if (!gl) return
  const now = performance.now()
  if (!startTime) startTime = now

  const t = ((now - startTime) / 1000) * props.speed * 0.1

  gl.uniform1f(gl.getUniformLocation(prog, 'uTime'), t)
  gl.uniform2f(gl.getUniformLocation(prog, 'uRes'), canvas.width, canvas.height)
  gl.uniform3fv(gl.getUniformLocation(prog, 'uColorA'), hexToRgb(props.colorStops[0] || '#171D22'))
  gl.uniform3fv(gl.getUniformLocation(prog, 'uColorB'), hexToRgb(props.colorStops[1] || '#7cff67'))
  gl.uniform3fv(gl.getUniformLocation(prog, 'uColorC'), hexToRgb(props.colorStops[2] || '#171D22'))
  gl.uniform1f(gl.getUniformLocation(prog, 'uAmplitude'), props.amplitude)
  gl.uniform1f(gl.getUniformLocation(prog, 'uBlend'), props.blend)
  gl.uniform2f(gl.getUniformLocation(prog, 'uMouse'), mouse.x, 1 - mouse.y)

  gl.viewport(0, 0, canvas.width, canvas.height)
  gl.drawArrays(gl.TRIANGLES, 0, 3)
  raf = requestAnimationFrame(() => renderFrame(gl, prog, canvas))
}

function resize(canvas, gl) {
  const rect = container.value?.getBoundingClientRect()
  if (!rect || rect.width === 0 || rect.height === 0) return
  const dpr = Math.min(window.devicePixelRatio || 1, 2)
  canvas.width = Math.floor(rect.width * dpr)
  canvas.height = Math.floor(rect.height * dpr)
  if (gl) gl.viewport(0, 0, canvas.width, canvas.height)
}

function onMove(e) {
  const rect = container.value?.getBoundingClientRect()
  if (!rect) return
  mouse.x = (e.clientX - rect.left) / rect.width
  mouse.y = (e.clientY - rect.top) / rect.height
}

let resizeFn = null

onMounted(() => {
  const c = document.createElement('canvas')
  c.style.cssText = 'position:absolute;inset:0;width:100%;height:100%;display:block;'
  container.value.appendChild(c)
  glCanvas.value = c

  const ctx = initGL(c)
  if (!ctx) return
  const { gl, prog } = ctx

  resizeFn = () => resize(c, gl)
  window.addEventListener('resize', resizeFn)
  window.addEventListener('mousemove', onMove, { passive: true })

  // 首帧等布局稳定后再定尺寸
  requestAnimationFrame(() => {
    resize(c, gl)
    renderFrame(gl, prog, c)
  })
})

onBeforeUnmount(() => {
  cancelAnimationFrame(raf)
  if (resizeFn) window.removeEventListener('resize', resizeFn)
  window.removeEventListener('mousemove', onMove)
  glCanvas.value?.remove()
  glCanvas.value = null
})
</script>

<template>
  <div ref="container" class="aurora-root" aria-hidden="true"></div>
</template>

<style scoped>
.aurora-root {
  position: absolute;
  inset: 0;
  overflow: hidden;
  pointer-events: none;
}
</style>
