<script setup>
import Aurora from './components/bits/Aurora.vue'
import BorderGlow from './components/bits/BorderGlow.vue'
import DotGrid from './components/bits/DotGrid.vue'
import DecryptedText from './components/bits/DecryptedText.vue'
import Counter from './components/bits/Counter.vue'
import ClickSpark from './components/bits/ClickSpark.vue'
import ElectricBorder from './components/bits/ElectricBorder.vue'
import Noise from './components/bits/Noise.vue'
import ParticleText from './components/bits/ParticleText.vue'
import PulseHeart from './components/bits/PulseHeart.vue'
import HoldButton from './components/bits/HoldButton.vue'
import GooeyNav from './components/bits/GooeyNav.vue'
import FlowingMenu from './components/bits/FlowingMenu.vue'
import ScrambleText from './components/bits/ScrambleText.vue'
import GlitchText from './components/bits/GlitchText.vue'
import EchoText from './components/bits/EchoText.vue'
import Lightning from './components/bits/Lightning.vue'
import ShinyText from './components/bits/ShinyText.vue'
import SpotlightCard from './components/bits/SpotlightCard.vue'
import Magnet from './components/bits/Magnet.vue'
import MagnetLines from './components/bits/MagnetLines.vue'
import TextType from './components/bits/TextType.vue'
import RotatingText from './components/bits/RotatingText.vue'
import Shuffle from './components/bits/Shuffle.vue'
import GridMotion from './components/bits/GridMotion.vue'
import CursorGrid from './components/bits/CursorGrid.vue'
import DepthText from './components/bits/DepthText.vue'
import StackedCarousel from './components/bits/StackedCarousel.vue'
import OrbitImages from './components/bits/OrbitImages.vue'
import FuzzyText from './components/bits/FuzzyText.vue'
import SplitText from './components/bits/SplitText.vue'
</script>

<template>
  <div class="page">

    <!-- Click spark overlay -->
    <ClickSpark color="#f97316" :count="10" />

    <!-- Aurora WebGL background (fixed, brighter palette) -->
    <div class="layer-bg">
      <Aurora
        :color-stops="['#4f46e5', '#06b6d4', '#f97316']"
        :amplitude="1.0"
        :blend="0.55"
        :speed="0.8"
      />
      <DotGrid
        :dot-size="1.5"
        :spacing="52"
        color="rgba(79,70,229,0.15)"
        highlight-color="rgba(6,182,212,0.8)"
        :mouse-radius="220"
      />
    </div>

    <!-- Light overlay to soften the background -->
    <div class="layer-overlay"></div>

    <!-- Main content -->
    <div class="content">

      <!-- ═══════════ NAVBAR ═══════════ -->
      <nav class="navbar">
        <div class="nav-logo">
          <span class="logo-mark">◆</span>
          <span>VueBits</span>
        </div>
        <div class="nav-links">
          <a href="#components">Components</a>
          <a href="#features">Features</a>
          <a href="https://vue-bits.dev" target="_blank" class="nav-cta">Visit Site →</a>
        </div>
      </nav>

      <!-- ═══════════ HERO ═══════════ -->
      <section class="hero">
        <div class="hero-badge">
          <span class="badge-live"></span>
          <span>209+ Components · Free Forever</span>
        </div>

        <h1 class="hero-title">
          <DecryptedText
            text="Animated UI for Vue."
            :speed="40"
            :stagger="25"
            class="title-decrypted"
          />
        </h1>

        <p class="hero-sub">
          Drop-in animated components &amp; backgrounds for Vue 3.<br />
          <span class="sub-highlight">The code is yours.</span> No runtime dependency.
        </p>

        <div class="hero-actions">
          <a href="https://vue-bits.dev" target="_blank" class="btn-primary">
            <span>Browse Components</span>
            <span class="btn-arrow">→</span>
          </a>
          <a href="https://github.com/DavidHDev/vue-bits" target="_blank" class="btn-ghost">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="currentColor" aria-hidden>
              <path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38
                         0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.06-.18-.28-.93-.5-1.13-.08-.08-.08-.18
                         .04-.23.08-.05.28-.08.55-.06.23.02.43.14.55.25.21.19.52.43.84.53.11-.03.27-.06.47-.07.85
                         -.35 1.45-.89 1.45-2.18 0-.02 0-.05-.01-.09.66-.09 1.45-.34 1.76-1.31.03-.1.04-.2.05-.29.01-.08.01-.17.01-.25
                         0-.22-.04-.43-.13-.62.57-.09 1.17-.32 1.69-.78-.2-.55-.55-1.04-1.03-1.42.07-.06.15-.14.21-.21.21-.25.34-.56
                         .4-.88.06-.32.06-.65.06-.96C15.98 3.58 12.42 0 8 0Z"/>
            </svg>
            GitHub
          </a>
        </div>

        <!-- Stats row -->
        <div class="hero-stats">
          <div class="stat-item">
            <span class="stat-num"><Counter from="0" to="209" :duration="2500" suffix="+" /></span>
            <span class="stat-lbl">Components</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-num"><Counter from="0" to="193" :duration="2500" /></span>
            <span class="stat-lbl">Live Demos</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-num"><Counter from="0" to="4" :duration="2000" /></span>
            <span class="stat-lbl">Frameworks</span>
          </div>
          <div class="stat-divider"></div>
          <div class="stat-item">
            <span class="stat-num"><Counter from="0" to="100" :duration="2000" suffix="%" /></span>
            <span class="stat-lbl">Free</span>
          </div>
        </div>
      </section>

      <!-- ═══════════ COMPONENT SHOWCASE ═══════════ -->
      <section class="section" id="components">
        <div class="section-header">
          <span class="section-tag">COMPONENTS</span>
          <h2 class="section-title">
            <DecryptedText text="Hover to experience" :speed="30" :stagger="15" />
          </h2>
          <p class="section-desc">BorderGlow cards respond to your cursor — the glow follows and intensifies near edges</p>
        </div>

        <div class="glow-grid">
          <BorderGlow
            :animated="true"
            :border-radius="28"
            :glow-color="'99 102 241'"
            background-color="rgba(255,255,255,0.72)"
            :glow-radius="48"
            :glow-intensity="0.9"
            :edge-sensitivity="40"
            :cone-spread="30"
            :colors="['#6366f1', '#06b6d4', '#f97316']"
            class="glow-card"
          >
            <div class="gc-inner">
              <div class="gc-icon">
                <svg width="22" height="22" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                  <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"/>
                </svg>
              </div>
              <h3>Cursor-Tracking Glow</h3>
              <p>The conic gradient border tracks your cursor angle in real-time. Move the pointer across the card edge to see the glow intensify.</p>
              <div class="gc-tag">BorderGlow</div>
            </div>
          </BorderGlow>

          <BorderGlow
            :animated="true"
            :border-radius="28"
            :glow-color="'6 182 212'"
            background-color="rgba(255,255,255,0.72)"
            :glow-radius="46"
            :glow-intensity="0.9"
            :edge-sensitivity="35"
            :cone-spread="28"
            :colors="['#06b6d4', '#8b5cf6', '#22c55e']"
            class="glow-card"
          >
            <div class="gc-inner">
              <div class="gc-icon">
                <svg width="22" height="22" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                  <rect x="3" y="3" width="18" height="18" rx="2"/><path d="M9 3v18M3 9h18"/>
                </svg>
              </div>
              <h3>Three-Color Mesh</h3>
              <p>Three HSL color stops are cycled across a conic gradient with a mask ring. The glow color and radius are fully configurable per card.</p>
              <div class="gc-tag">BorderGlow</div>
            </div>
          </BorderGlow>

          <BorderGlow
            :animated="true"
            :border-radius="28"
            :glow-color="'249 115 22'"
            background-color="rgba(255,255,255,0.72)"
            :glow-radius="50"
            :glow-intensity="0.9"
            :edge-sensitivity="30"
            :cone-spread="32"
            :colors="['#f97316', '#ec4899', '#a855f7']"
            class="glow-card"
          >
            <div class="gc-inner">
              <div class="gc-icon">
                <svg width="22" height="22" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                  <path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"/>
                </svg>
              </div>
              <h3>Mount Sweep Animation</h3>
              <p>On page load, a 360° sweep plays once around the border to attract attention. After that the border follows your cursor direction.</p>
              <div class="gc-tag">animated=true</div>
            </div>
          </BorderGlow>
        </div>
      </section>

      <!-- ═══════════ AURORA SECTION ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">BACKGROUND</span>
          <h2 class="section-title">
            <DecryptedText text="WebGL Aurora" :speed="30" :stagger="20" />
          </h2>
          <p class="section-desc">Simplex noise shader rendered in real-time via WebGL. Moves with your cursor.</p>
        </div>

        <div class="aurora-frame">
          <Aurora
            :color-stops="['#4f46e5', '#06b6d4', '#fbbf24']"
            :amplitude="1.4"
            :blend="0.7"
            :speed="1.0"
          />
          <div class="aurora-caption">
            <span class="cap-dot"></span>
            OGL shader · simplex noise · GPU-rendered
          </div>
        </div>
      </section>

      <!-- ═══════════ FX SHOWCASE: Lightning / Glitch / Echo ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">EFFECTS</span>
          <h2 class="section-title">
            <DecryptedText text="Electric &amp; Glitch" :speed="30" :stagger="18" />
          </h2>
          <p class="section-desc">Canvas 程序化闪电 / RGB 分离故障 / 重影文字</p>
        </div>
        <div class="fx-grid">
          <div class="fx-card">
            <h3>⚡ Lightning</h3>
            <div class="fx-canvas-wrap">
              <Lightning :size="220" :frequency="2" color="#818cf8" :max-branches="4" />
            </div>
            <p class="fx-desc">Canvas 程序化闪电：分叉 + 辉光 + 白芯，循环闪回</p>
          </div>
          <div class="fx-card">
            <h3>👁️ GlitchText</h3>
            <div class="fx-text-center">
              <GlitchText :auto="true" :interval="2500" :offset="5">
                <span style="font-size:44px; font-weight:800; color:#1e293b;">GLITCH</span>
              </GlitchText>
            </div>
            <p class="fx-desc">RGB 分离故障抖动，红蓝重影随机位移与裁切</p>
          </div>
          <div class="fx-card">
            <h3>🔁 EchoText</h3>
            <div class="fx-text-center" style="height: 240px;">
              <EchoText text="hello" :ghosts="3" />
            </div>
            <p class="fx-desc">重影文字残留在后方渐次消散（静态演示）</p>
          </div>
        </div>
      </section>

      <!-- ═══════════ PARTICLE TEXT ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">PARTICLE</span>
          <h2 class="section-title">
            <DecryptedText text="ParticleText" :speed="25" :stagger="20" />
          </h2>
          <p class="section-desc">1200 个粒子聚拢成文字，靠近的光标会推开粒子</p>
        </div>
        <div class="particle-frame">
          <ParticleText text="VUE BITS" :count="1200" color="#6366f1" />
        </div>
      </section>

      <!-- ═══════════ ELECTRIC BORDER ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">ELECTRIC</span>
          <h2 class="section-title">
            <DecryptedText text="ElectricBorder" :speed="25" :stagger="20" />
          </h2>
          <p class="section-desc">四条电流电弧沿圆角矩形周界流动，头部亮、尾迹渐灭</p>
        </div>
        <div class="electric-frame">
          <ElectricBorder :arc-count="4" color="#818cf8" spark-color="#c7d2fe" :radius="24" style="display:block;">
            <div class="electric-inner">
              <div class="electric-title">电流边框</div>
              <p>沿周界流动的 4 条电弧，带火花头部与抖动尾迹</p>
            </div>
          </ElectricBorder>
        </div>
      </section>

      <!-- ═══════════ NAV DEMO: Gooey + Flowing ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">NAVIGATION</span>
          <h2 class="section-title">
            <DecryptedText text="Gooey &amp; Flowing" :speed="30" :stagger="15" />
          </h2>
          <p class="section-desc">两种液态导航：Gooey 黏液融合 vs Flowing 弹性滑动</p>
        </div>
        <div class="nav-demo">
          <span class="nav-demo-label">GooeyNav</span>
          <GooeyNav :items="['Overview','Demo','Code','Docs']" :active-index="0" />
          <div style="height: 24px"></div>
          <span class="nav-demo-label">FlowingMenu</span>
          <FlowingMenu :items="['Design','Motion','Shader']" :active-index="1" />
        </div>
      </section>

      <!-- ═══════════ BUTTONS: Hold + PulseHeart ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">INTERACT</span>
          <h2 class="section-title">
            <DecryptedText text="Hold &amp; Pulse" :speed="30" :stagger="15" />
          </h2>
          <p class="section-desc">长按液面填充→触发；爱心点击后脉动放大换色</p>
        </div>
        <div class="btn-demo">
          <HoldButton label="Hold to Delete" done-label="Deleted" :duration="1200" />
          <PulseHeart :size="56" active-color="#ef4444" />
          <PulseHeart :size="56" active-color="#f97316" />
        </div>
      </section>

      <!-- ═══════════ TEXT FX: Shiny / Rotating / Split / Fuzzy ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">TEXT FX</span>
          <h2 class="section-title">
            <DecryptedText text="Text Effects" :speed="25" :stagger="20" />
          </h2>
          <p class="section-desc">金属光泽 / 3D 翻转轮播 / 逐字入场 / 高频振动</p>
        </div>
        <div class="textfx-grid">
          <div class="textfx-card glass">
            <h3>ShinyText</h3>
            <div class="tfx-center"><ShinyText text="SHINY METALLIC" sheen-color="linear-gradient(90deg, #6366f1 20%, #fff 50%, #06b6d4 80%)" /></div>
            <p class="fx-desc">background-clip:text 金属流光</p>
          </div>
          <div class="textfx-card glass">
            <h3>RotatingText</h3>
            <div class="tfx-center" style="font-size:42px;"><RotatingText :phrases="['VUE','BITS','ANIMATED']" :duration="2200" :swap-time="700" /></div>
            <p class="fx-desc">3D 翻转切换多语句</p>
          </div>
          <div class="textfx-card glass">
            <h3>SplitText</h3>
            <div class="tfx-center" style="font-size:36px;"><SplitText text="逐字入场" :stagger="90" :duration="700" :delay="500" /></div>
            <p class="fx-desc">逐字模糊下滑交错</p>
          </div>
          <div class="textfx-card glass">
            <h3>FuzzyText</h3>
            <div class="tfx-center" style="font-size:36px;"><FuzzyText text="模糊振动" :intensity="0.7" /></div>
            <p class="fx-desc">高频微位移 + 抖动</p>
          </div>
        </div>
      </section>

      <!-- ═══════════ TYPE: TextType / DepthText / Orbit ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">TYPE</span>
          <h2 class="section-title">
            <DecryptedText text="Typing &amp; Depth" :speed="30" :stagger="15" />
          </h2>
          <p class="section-desc">打字机循环 / 3D 挤出视差 / 环形轨道</p>
        </div>
        <div class="type-grid">
          <div class="type-card glass">
            <h3>TextType</h3>
            <div style="font-size: 22px; font-weight: 700; min-height: 32px;"><TextType text="Hello Vue Bits!" :speed="120" :pause="1400" /></div>
            <p class="fx-desc">打字机 + 闪烁光标 + 循环删除</p>
          </div>
          <div class="type-card glass">
            <h3>DepthText</h3>
            <div style="font-size: 56px; font-weight: 900; min-height: 80px; display:flex; align-items:center; justify-content:center;"><DepthText text="3D" :layers="8" :depth="3" extrude-color="#a5b4fc" color="#1e293b" /></div>
            <p class="fx-desc">层叠挤出 + 鼠标视差</p>
          </div>
          <div class="type-card glass">
            <h3>OrbitImages</h3>
            <div style="min-height: 280px; display:flex; align-items:center; justify-content:center;"><OrbitImages :count="6" :radius="90" :period="16" /></div>
            <p class="fx-desc">等距环形匀速旋转</p>
          </div>
        </div>
      </section>

      <!-- ═══════════ CAROUSEL & SHUFFLE ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">STACK &amp; SHUFFLE</span>
          <h2 class="section-title">
            <DecryptedText text="Stack &amp; Shuffle" :speed="30" :stagger="15" />
          </h2>
          <p class="section-desc">堆叠卡片轮播 + 字符洗牌揭示</p>
        </div>
        <div class="stack-shuffle">
          <div class="stack-card">
            <StackedCarousel :items="['✨ 组件 1','🚀 组件 2','🌊 组件 3','⚡ 组件 4']" auto :interval="2200" />
          </div>
          <div class="shuffle-card">
            <h3>Shuffle</h3>
            <div style="font-size: 44px; font-weight: 800; min-height: 56px; text-align:center; padding: 16px 0;"><Shuffle text="SHUFFLE" :tick="50" delay="300" /></div>
            <p class="fx-desc">字符逐个锁定，未锁定持续乱码翻转</p>
          </div>
        </div>
      </section>

      <!-- ═══════════ INTERACTIVE POINTERS ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">INTERACTIVE</span>
          <h2 class="section-title">
            <DecryptedText text="Pointer Reactions" :speed="28" :stagger="18" />
          </h2>
          <p class="section-desc">聚光卡片 / 磁场吸附 / 光标点阵 / 透视网格</p>
        </div>
        <div class="interact-grid">
          <div class="interact-cell">
            <h3>SpotlightCard</h3>
            <SpotlightCard style="padding: 28px;">
              <h4 style="font-size:16px; font-weight:700; margin:0 0 8px;">✨ Hover 我</h4>
              <p style="font-size:13px; color:#64748b; margin:0;">光标位置驱动的高光晕染</p>
            </SpotlightCard>
          </div>
          <div class="interact-cell">
            <h3>Magnet</h3>
            <div style="height: 160px; display:flex; align-items:center; justify-content:center; gap:20px;">
              <Magnet :radius="260" :strength="70"><div class="magnet-chip" style="font-size:24px;">🧲</div></Magnet>
              <Magnet :radius="260" :strength="70"><div class="magnet-chip" style="font-size:24px;">🪐</div></Magnet>
              <Magnet :radius="260" :strength="70"><div class="magnet-chip" style="font-size:24px;">💫</div></Magnet>
            </div>
            <p class="fx-desc">元素向光标磁性吸附</p>
          </div>
          <div class="interact-cell">
            <h3>CursorGrid</h3>
            <CursorGrid :rows="7" :cols="12" :radius="140" />
            <p class="fx-desc">网格单元随光标距离点亮</p>
          </div>
          <div class="interact-cell">
            <h3>GridMotion</h3>
            <GridMotion :rows="10" :cols="12" />
            <p class="fx-desc">透视网格 + 消失点随光标移动</p>
          </div>
          <div class="interact-cell">
            <h3>MagnetLines</h3>
            <MagnetLines :lines="12" radius="200" :strength="50" />
            <p class="fx-desc">水平场线向光标弯曲</p>
          </div>
          <div class="interact-cell">
            <h3>Shuffle 重播</h3>
            <div style="font-size: 32px; font-weight: 800; min-height: 40px; text-align:center; padding: 8px 0;"><Shuffle text="REWIND" :tick="45" delay="200" /></div>
            <p class="fx-desc">另一组字符洗牌演示</p>
          </div>
        </div>
      </section>

      <!-- ═══════════ SCRAMBLE TEXT ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">SCRAMBLE</span>
          <h2 class="section-title">
            <DecryptedText text="ScrambleText" :speed="30" :stagger="15" />
          </h2>
          <p class="section-desc">乱码字符逐步定位到目标，逐字定格</p>
        </div>
        <div class="scramble-demo" style="font-size: 48px; font-weight: 800; letter-spacing: 2px; color: #1e293b;">
          <ScrambleText text="HELLO VUE BITS" :char-time="70" start-delay="400" />
        </div>
      </section>

      <!-- ═══════════ FEATURES ═══════════ -->
      <section class="section" id="features">
        <div class="section-header">
          <span class="section-tag">WHY VUE-BITS</span>
          <h2 class="section-title">
            <DecryptedText text="Built for developers" :speed="25" :stagger="18" />
          </h2>
        </div>

        <div class="feature-grid">
          <div class="feature-card">
            <div class="fc-icon">⚡</div>
            <h3>Zero Runtime Dep</h3>
            <p>Each component is self-contained source code. No vue-bits package required — it's yours to own.</p>
          </div>
          <div class="feature-card">
            <div class="fc-icon">🎨</div>
            <h3>Fully Customizable</h3>
            <p>Every color, speed, size, and intensity is a prop. Tailwind, CSS Modules, or inline — all work.</p>
          </div>
          <div class="feature-card">
            <div class="fc-icon">🖥️</div>
            <h3>GPU Shaders</h3>
            <p>Aurora, Ballpit, and other effects use WebGL via OGL or Three.js for smooth 60fps rendering.</p>
          </div>
          <div class="feature-card">
            <div class="fc-icon">📦</div>
            <h3>Multi-Framework</h3>
            <p>Same effects in React, Svelte, and SolidJS. Install once via CLI, use anywhere.</p>
          </div>
          <div class="feature-card">
            <div class="fc-icon">🔧</div>
            <h3>CLI Install</h3>
            <p><code>npx pnpm dlx @vue-bits/cli add Aurora</code> — one command drops source into your project.</p>
          </div>
          <div class="feature-card">
            <div class="fc-icon">♿</div>
            <h3>Accessible</h3>
            <p>Respects <code>prefers-reduced-motion</code>. Decorative layers are aria-hidden by design.</p>
          </div>
        </div>
      </section>

      <!-- ═══════════ CODE SNIPPET ═══════════ -->
      <section class="section">
        <div class="section-header">
          <span class="section-tag">INSTALL</span>
          <h2 class="section-title">
            <DecryptedText text="One command to start" :speed="20" :stagger="15" />
          </h2>
        </div>

        <div class="code-block">
          <div class="code-header">
            <span class="dot red"></span>
            <span class="dot yellow"></span>
            <span class="dot green"></span>
            <span class="code-title">terminal</span>
          </div>
          <div class="code-body">
            <div class="code-line">
              <span class="code-prompt">$</span>
              <span class="code-cmd">npx</span>
              <span class="code-arg">pnpm</span>
              <span class="code-arg">dlx</span>
              <span class="code-arg">@vue-bits/cli</span>
              <span class="code-arg">add</span>
              <span class="code-highlight">Aurora</span>
            </div>
            <div class="code-line">
              <span class="code-prompt">✓</span>
              <span class="code-dim">Aurora.vue → src/components/Aurora/Aurora.vue</span>
            </div>
            <div class="code-line">
              <span class="code-prompt">✓</span>
              <span class="code-dim">ogl@^1.0.11 installed</span>
            </div>
            <div class="code-line">
              <span class="code-prompt">$</span>
              <span class="code-cursor blink">▊</span>
            </div>
          </div>
        </div>
      </section>

      <!-- ═══════════ CTA ═══════════ -->
      <section class="cta-section">
        <div class="cta-card">
          <div class="cta-inner">
            <h2>Ready to build something<br />visually striking?</h2>
            <p>209+ animated components. Free. The code is yours.</p>
            <div class="cta-actions">
              <a href="https://vue-bits.dev" target="_blank" class="btn-cta">
                Go to vue-bits.dev →
              </a>
            </div>
          </div>
        </div>
      </section>

      <!-- ═══════════ FOOTER ═══════════ -->
      <footer class="footer">
        <div class="footer-top">
          <div class="footer-brand">
            <span class="logo-mark-sm">◆</span> VueBits
          </div>
          <div class="footer-links">
            <a href="https://vue-bits.dev" target="_blank">Components</a>
            <a href="https://github.com/DavidHDev/vue-bits" target="_blank">GitHub</a>
            <a href="https://vue-bits.dev/get-started/installation" target="_blank">Docs</a>
          </div>
        </div>
        <div class="footer-bottom">
          <p>Demo page built with Vue 3 + Vite · Components: vue-bits.dev pattern</p>
        </div>
      </footer>
    </div>

    <!-- 全局噪点叠加 -->
    <Noise />
  </div>
</template>

<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  background: #f0f4ff;
  color: #1e293b;
  font-family: -apple-system, BlinkMacSystemFont, 'Inter', 'Segoe UI', sans-serif;
  overflow-x: hidden;
}

.page {
  position: relative;
  min-height: 100vh;
  background: #f0f4ff;
}

/* ── Layers ─────────────────────────────── */
.layer-bg {
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  opacity: 1;
  filter: saturate(1.7) brightness(1.12);
}
.layer-overlay {
  position: fixed;
  inset: 0;
  z-index: 0;
  pointer-events: none;
  background:
    radial-gradient(ellipse 90% 40% at 50% 0%, rgba(255,255,255,0.55) 0%, rgba(240,244,255,0.15) 60%, transparent 100%),
    linear-gradient(to bottom, transparent 0%, transparent 55%, rgba(240,244,255,0.75) 100%);
}

/* ── Content ────────────────────────────── */
.content {
  position: relative;
  z-index: 1;
  max-width: 1160px;
  margin: 0 auto;
  padding: 0 32px;
}

/* ── Navbar ─────────────────────────────── */
.navbar {
  position: sticky;
  top: 0;
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 18px 32px;
  margin: 0 -32px 0;
  border-bottom: 1px solid rgba(79,70,229,0.12);
  backdrop-filter: blur(16px);
  background: rgba(255,255,255,0.78);
}
.nav-logo {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 18px;
  font-weight: 700;
  color: #1e293b;
}
.logo-mark { color: #6366f1; font-size: 14px; }
.logo-mark-sm { color: #6366f1; font-size: 12px; }
.nav-links { display: flex; gap: 28px; align-items: center; }
.nav-links a {
  font-size: 14px;
  color: #64748b;
  text-decoration: none;
  transition: color 0.2s;
}
.nav-links a:hover { color: #1e293b; }
.nav-cta {
  background: #6366f1;
  color: #fff !important;
  padding: 8px 18px;
  border-radius: 100px;
  font-weight: 600;
  transition: background 0.2s !important;
}
.nav-cta:hover { background: #4f46e5; }

/* ── Hero ───────────────────────────────── */
.hero {
  min-height: 85vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  gap: 30px;
  padding: 80px 0;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: rgba(99,102,241,0.1);
  border: 1px solid rgba(99,102,241,0.3);
  border-radius: 100px;
  padding: 8px 20px;
  font-size: 13px;
  color: #4f46e5;
  font-weight: 600;
  backdrop-filter: blur(8px);
}
.badge-live {
  width: 7px; height: 7px;
  background: #f97316;
  border-radius: 50%;
  animation: pulse-glow 2s infinite;
}
@keyframes pulse-glow {
  0%,100% { opacity:1; box-shadow: 0 0 6px #f97316; }
  50% { opacity:0.3; box-shadow: 0 0 2px #f97316; }
}

.hero-title {
  font-size: clamp(48px, 9vw, 84px);
  font-weight: 800;
  line-height: 1.02;
  letter-spacing: -0.02em;
  min-height: 1em;
}
.title-decrypted {
  display: inline;
  background: linear-gradient(135deg, #4f46e5 0%, #06b6d4 50%, #f97316 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  font-family: 'JetBrains Mono', 'Fira Code', monospace;
  font-size: clamp(28px, 4.5vw, 54px) !important;
}

.hero-sub {
  font-size: 18px;
  line-height: 1.75;
  color: #475569;
  max-width: 480px;
}
.sub-highlight { color: #4f46e5; font-weight: 700; }

.hero-actions { display: flex; gap: 14px; flex-wrap: wrap; justify-content: center; }

.btn-primary {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  background: linear-gradient(135deg, #6366f1, #06b6d4);
  color: #fff;
  font-size: 15px;
  font-weight: 700;
  padding: 14px 28px;
  border-radius: 14px;
  text-decoration: none;
  transition: transform 0.2s, box-shadow 0.25s;
  box-shadow: 0 4px 24px rgba(99,102,241,0.4);
}
.btn-primary:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 36px rgba(99,102,241,0.55);
}
.btn-arrow { transition: transform 0.2s; }
.btn-primary:hover .btn-arrow { transform: translateX(3px); }

.btn-ghost {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(255,255,255,0.65);
  border: 1.5px solid rgba(79,70,229,0.25);
  color: #1e293b;
  font-size: 15px;
  font-weight: 600;
  padding: 14px 28px;
  border-radius: 14px;
  text-decoration: none;
  transition: background 0.2s, border-color 0.2s;
  backdrop-filter: blur(8px);
}
.btn-ghost:hover {
  background: rgba(255,255,255,0.95);
  border-color: #6366f1;
}

/* Stats */
.hero-stats {
  display: flex;
  align-items: center;
  gap: 24px;
  padding: 24px 40px;
  background: rgba(255,255,255,0.65);
  border: 1px solid rgba(99,102,241,0.15);
  border-radius: 20px;
  backdrop-filter: blur(12px);
  flex-wrap: wrap;
  justify-content: center;
}
.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  min-width: 80px;
}
.stat-num {
  font-size: 36px;
  font-weight: 800;
  background: linear-gradient(135deg, #4f46e5, #06b6d4);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-variant-numeric: tabular-nums;
}
.stat-lbl {
  font-size: 11px;
  color: #64748b;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  font-weight: 600;
}
.stat-divider {
  width: 1px;
  height: 44px;
  background: rgba(99,102,241,0.18);
}

/* ── Sections ───────────────────────────── */
.section { padding: 90px 0; position: relative; }

.section-header {
  text-align: center;
  margin-bottom: 52px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 14px;
}
.section-tag {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 3px;
  color: #4f46e5;
  background: rgba(99,102,241,0.1);
  padding: 4px 14px;
  border-radius: 100px;
}
.section-title {
  font-size: 34px;
  font-weight: 800;
  color: #1e293b;
}
.section-desc {
  font-size: 15px;
  color: #64748b;
  max-width: 420px;
  line-height: 1.6;
}

/* ── Glow cards grid ────────────────────── */
.glow-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
}
.glow-card { height: 100%; }

.gc-inner {
  padding: 32px;
  height: 100%;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.gc-icon {
  width: 48px; height: 48px;
  background: linear-gradient(135deg, rgba(99,102,241,0.15), rgba(6,182,212,0.15));
  border: 1px solid rgba(99,102,241,0.2);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #4f46e5;
}

.gc-inner h3 { font-size: 17px; font-weight: 700; color: #1e293b; }
.gc-inner p { font-size: 13.5px; line-height: 1.7; color: #64748b; flex: 1; }
.gc-tag {
  display: inline-flex;
  align-items: center;
  font-size: 11px;
  font-weight: 600;
  color: #4f46e5;
  background: rgba(99,102,241,0.1);
  padding: 3px 10px;
  border-radius: 100px;
  align-self: flex-start;
}

/* ── Aurora frame ────────────────────────── */
.aurora-frame {
  position: relative;
  height: 420px;
  border-radius: 24px;
  overflow: hidden;
  border: 1px solid rgba(99,102,241,0.25);
  background: linear-gradient(135deg, #1e1b4b, #312e81);
  cursor: crosshair;
  box-shadow: 0 20px 60px rgba(99,102,241,0.2);
}
.aurora-frame > .aurora-root {
  position: absolute !important;
  inset: 0 !important;
}
.aurora-caption {
  position: absolute;
  bottom: 16px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 12px;
  color: rgba(255,255,255,0.8);
  background: rgba(0,0,0,0.45);
  padding: 5px 14px;
  border-radius: 100px;
  white-space: nowrap;
  backdrop-filter: blur(4px);
}
.cap-dot {
  width: 6px; height: 6px;
  background: #4ade80;
  border-radius: 50%;
  animation: cap-blink 1.5s infinite;
}
@keyframes cap-blink { 0%,100%{opacity:1;} 50%{opacity:0.3;} }

/* ── Features ────────────────────────────── */
.feature-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 18px;
}
.feature-card {
  background: rgba(255,255,255,0.72);
  border: 1px solid rgba(99,102,241,0.15);
  border-radius: 20px;
  padding: 28px;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}
.feature-card:hover {
  background: rgba(255,255,255,0.95);
  border-color: rgba(99,102,241,0.45);
  transform: translateY(-4px);
  box-shadow: 0 16px 48px rgba(99,102,241,0.18);
}
.fc-icon { font-size: 28px; margin-bottom: 14px; display: block; }
.feature-card h3 { font-size: 15px; font-weight: 700; color: #1e293b; margin-bottom: 8px; }
.feature-card p { font-size: 13px; line-height: 1.7; color: #64748b; }
.feature-card code {
  background: rgba(99,102,241,0.12);
  color: #4f46e5;
  font-family: 'JetBrains Mono', monospace;
  font-size: 11.5px;
  padding: 2px 6px;
  border-radius: 5px;
}

/* ── Code block ──────────────────────────── */
.code-block {
  max-width: 560px;
  margin: 0 auto;
  background: #1e1b4b;
  border: 1px solid rgba(99,102,241,0.35);
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 20px 60px rgba(99,102,241,0.25);
}
.code-header {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 14px 18px;
  background: rgba(99,102,241,0.15);
  border-bottom: 1px solid rgba(99,102,241,0.25);
}
.dot { width: 12px; height: 12px; border-radius: 50%; }
.dot.red { background: #ff5f57; }
.dot.yellow { background: #ffbd2e; }
.dot.green { background: #28ca42; }
.code-title { font-size: 12px; color: #a5b4fc; margin-left: auto; }
.code-body {
  padding: 20px;
  font-family: 'JetBrains Mono', 'Fira Code', monospace;
  font-size: 14px;
  line-height: 2.1;
}
.code-line { display: flex; gap: 8px; }
.code-prompt { color: #64748b; flex-shrink: 0; }
.code-cmd { color: #22d3ee; font-weight: 600; }
.code-arg { color: #e2e8f0; }
.code-highlight { color: #a78bfa; font-weight: 600; }
.code-dim { color: #94a3b8; font-size: 13px; }
.code-cursor { color: #a78bfa; }
.blink { animation: blink-cursor 1s step-end infinite; }
@keyframes blink-cursor { 0%,100%{opacity:1;} 50%{opacity:0;} }

/* ── CTA ────────────────────────────────── */
.cta-section { padding: 60px 0; }
.cta-card {
  position: relative;
  border-radius: 32px;
  overflow: hidden;
  background: linear-gradient(135deg, #eef2ff, #ecfeff, #fff7ed);
  border: 1px solid rgba(99,102,241,0.3);
  box-shadow: 0 0 80px rgba(99,102,241,0.2);
}
.cta-card::before {
  content: '';
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse 40% 60% at 30% 30%, rgba(99,102,241,0.25), transparent),
    radial-gradient(ellipse 50% 50% at 70% 70%, rgba(6,182,212,0.25), transparent),
    radial-gradient(ellipse 40% 40% at 90% 20%, rgba(249,115,22,0.2), transparent);
  pointer-events: none;
}
.cta-inner {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 80px 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 18px;
}
.cta-inner h2 {
  font-size: 36px;
  font-weight: 800;
  line-height: 1.2;
  color: #1e293b;
}
.cta-inner p { font-size: 16px; color: #64748b; }
.cta-actions { margin-top: 8px; }
.btn-cta {
  display: inline-flex;
  align-items: center;
  background: linear-gradient(135deg, #6366f1, #06b6d4);
  color: #fff;
  font-size: 16px;
  font-weight: 700;
  padding: 16px 36px;
  border-radius: 16px;
  text-decoration: none;
  transition: transform 0.2s, box-shadow 0.25s;
  box-shadow: 0 8px 32px rgba(99,102,241,0.5);
}
.btn-cta:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 48px rgba(99,102,241,0.7);
}

/* ── Footer ─────────────────────────────── */
.footer {
  border-top: 1px solid rgba(99,102,241,0.15);
  margin-top: 40px;
  padding: 40px 0;
}
.footer-top {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 20px;
  margin-bottom: 20px;
}
.footer-brand { font-size: 18px; font-weight: 700; color: #1e293b; display: flex; align-items: center; gap: 8px; }
.footer-links { display: flex; gap: 24px; }
.footer-links a { font-size: 13px; color: #64748b; text-decoration: none; transition: color 0.2s; }
.footer-links a:hover { color: #4f46e5; }
.footer-bottom p { font-size: 12px; color: #94a3b8; }

/* ── Responsive ─────────────────────────── */
@media (max-width: 768px) {
  .content { padding: 0 20px; }
  .navbar { margin: 0 -20px; padding: 16px 20px; }
  .hero { min-height: 70vh; padding: 60px 0; }
  .hero-stats { padding: 20px; gap: 16px; }
  .nav-links a:not(.nav-cta) { display: none; }
  .cta-inner { padding: 60px 24px; }
  .cta-inner h2 { font-size: 26px; }
  .fx-grid { grid-template-columns: 1fr; }
}

/* ── 粒子拼字 ───────────────────────────── */
.particle-frame {
  max-width: 900px;
  margin: 0 auto;
  background: linear-gradient(135deg, #eef2ff, #ecfeff);
  border: 1px solid rgba(99,102,241,0.25);
  border-radius: 24px;
  overflow: hidden;
  box-shadow: 0 20px 60px rgba(99,102,241,0.18);
}

/* ── FX 三列 ───────────────────────────── */
.fx-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}
.fx-card {
  background: rgba(255,255,255,0.72);
  border: 1px solid rgba(99,102,241,0.15);
  border-radius: 20px;
  padding: 24px;
  backdrop-filter: blur(10px);
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.fx-card h3 { font-size: 15px; font-weight: 700; color: #1e293b; margin: 0; }
.fx-canvas-wrap {
  height: 240px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(180deg, rgba(30,27,75,0.95), rgba(49,46,129,0.85));
  border-radius: 12px;
  overflow: hidden;
}
.fx-text-center {
  height: 240px;
  display: flex;
  align-items: flex-start;
  padding-top: 40px;
}
.fx-desc { font-size: 13px; color: #64748b; line-height: 1.6; }

/* ── 电弧边框 ─────────────────────────── */
.electric-frame {
  max-width: 800px;
  margin: 0 auto;
  background: linear-gradient(135deg, rgba(238,242,255,0.7), rgba(236,254,255,0.7));
  border-radius: 24px;
  padding: 2px;
  backdrop-filter: blur(10px);
}
.electric-inner {
  padding: 56px 40px;
  text-align: center;
  border-radius: 22px;
  background: rgba(255,255,255,0.55);
}
.electric-title {
  font-size: 36px;
  font-weight: 800;
  background: linear-gradient(135deg, #6366f1, #f97316);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
.electric-inner p { font-size: 15px; color: #64748b; margin-top: 12px; }

/* ── 导航演示 ──────────────────────────── */
.nav-demo {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  max-width: 500px;
  margin: 0 auto;
  background: rgba(255,255,255,0.6);
  border: 1px solid rgba(99,102,241,0.15);
  border-radius: 20px;
  padding: 32px;
  backdrop-filter: blur(10px);
}
.nav-demo-label {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 2px;
  color: #64748b;
  text-transform: uppercase;
  margin-bottom: 12px;
}

/* ── 文字特效 4格 ── */
.textfx-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 18px;
}
.textfx-card {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 20px;
  border-radius: 20px;
  min-height: 180px;
  justify-content: center;
}
.textfx-card h3 { font-size: 13px; font-weight: 700; color: #6366f1; margin: 0; letter-spacing: 0.5px; }
.tfx-center { min-height: 56px; display: flex; align-items: center; justify-content: center; }

/* ── 打字/深度/轨道 3格 ── */
.type-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 18px;
}
.type-card {
  display: flex;
  flex-direction: column;
  gap: 14px;
  padding: 24px;
  border-radius: 20px;
  min-height: 280px;
}
.type-card h3 { font-size: 13px; font-weight: 700; color: #6366f1; margin: 0; letter-spacing: 0.5px; }

/* ── 堆叠卡片 + 洗牌 ── */
.stack-shuffle {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  max-width: 900px;
  margin: 0 auto;
}
.stack-card {
  background: rgba(255,255,255,0.6);
  border: 1px solid rgba(99,102,241,0.15);
  border-radius: 20px;
  padding: 20px;
  backdrop-filter: blur(10px);
}
.shuffle-card {
  background: linear-gradient(135deg, rgba(238,242,255,0.7), rgba(236,254,255,0.7));
  border: 1px solid rgba(99,102,241,0.2);
  border-radius: 20px;
  padding: 32px;
  text-align: center;
  backdrop-filter: blur(10px);
}
.shuffle-card h3 { font-size: 13px; font-weight: 700; color: #6366f1; margin: 0 0 16px; }

/* ── 交互指针 6格 ── */
.interact-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 18px;
}
.interact-cell {
  display: flex;
  flex-direction: column;
  gap: 10px;
  background: rgba(255,255,255,0.6);
  border: 1px solid rgba(99,102,241,0.15);
  border-radius: 20px;
  padding: 20px;
  backdrop-filter: blur(10px);
  min-height: 280px;
}
.interact-cell h3 { font-size: 13px; font-weight: 700; color: #6366f1; margin: 0; letter-spacing: 0.5px; }
.magnet-chip {
  width: 56px; height: 56px;
  display: flex; align-items: center; justify-content: center;
  background: rgba(255,255,255,0.9);
  border: 1px solid rgba(99,102,241,0.3);
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(99,102,241,0.15);
  will-change: transform;
}

/* ── 按钮演示 ──────────────────────────── */
.btn-demo {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 24px;
  flex-wrap: wrap;
  padding: 40px;
  background: rgba(255,255,255,0.6);
  border: 1px solid rgba(99,102,241,0.15);
  border-radius: 20px;
  backdrop-filter: blur(10px);
}

/* ── 乱码文字 ──────────────────────────── */
.scramble-demo {
  text-align: center;
  padding: 56px 32px;
  background: linear-gradient(135deg, rgba(238,242,255,0.7), rgba(236,254,255,0.7));
  border: 1px solid rgba(99,102,241,0.2);
  border-radius: 24px;
  backdrop-filter: blur(10px);
}
</style>
