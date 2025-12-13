<script setup lang="ts">
import gsap from 'gsap'
import { ScrollToPlugin } from 'gsap/ScrollToPlugin'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { nextTick, onBeforeUnmount, onMounted, ref } from 'vue'

gsap.registerPlugin(ScrollTrigger, ScrollToPlugin)

const highlights = [
  {
    label: '手蛇共鸣',
    title: '指向式手势',
    desc: '全局 0.1 秒以下响应延迟，快速捕捉手势动向，精准响应手势操作。让手势操作瞬间生效，无需等待。',
  },
  {
    label: '经典模式',
    title: '全新随机贪吃蛇',
    desc: '多种游戏玩法，随机效果食物，随机生成障碍物，随机生成道具。别出心裁的设计，让每一次游戏都有新的挑战。',
  },
  {
    label: '沉浸音画',
    title: '沉浸音效',
    desc: '逐一沉浸音效，每次操作都有不同的音效，让游戏更有沉浸感。魔性洗脑背景音乐，让你在游戏中感受到真实的环境。',
  },
]

const specs = [
  { name: 'pygame', value: '' },
  { name: 'opencv', value: '' },
  { name: 'numpy', value: '' },
  { name: 'pillow', value: '' },
  { name: 'cvzone', value: '' },
  { name: 'mediapipe', value: '' },
  { name: 'protobuf', value: '' },
  { name: 'requests', value: '' },
]

const milestones = [
  { phase: 'ALPHA', badge: '完成', detail: '基本功能' },
  { phase: 'BETA', badge: '进行中', detail: '限量内测，持续收集玩法与性能反馈' },
  { phase: 'LAUNCH', badge: '进行中', detail: '持续迭代' },
].reverse()

const screenshots = [
  { id: 'loading', file: '/assets/screenshots/loading.png', label: 'Loading', desc: '独创游戏加载动画\n炫酷粒子特效' },
  { id: 'modes', file: '/assets/screenshots/modes.png', label: 'Modes', desc: '简约面板 自选游戏模式' },
  { id: 'classic', file: '/assets/screenshots/classic.png', label: 'Classic', desc: '简约经典模式\n可自行切换背景图' },
  { id: 'gesture', file: '/assets/screenshots/gesture.png', label: 'Gesture', desc: '创新性手势控制模式\n用你的双手创造轨迹' },
  { id: 'language', file: '/assets/screenshots/language.png', label: 'Language', desc: '支持全局中英文无缝切换' },
  { id: 'colorful', file: '/assets/screenshots/colorful.jpg', label: 'Colorful', desc: '根据你的喜好自定义颜色\n<span style="font-size: 28px;opacity: 0.75">更多颜色敬请期待</span>' },
  { id: 'gesture_modes', file: '/assets/screenshots/gesture_modes.png', label: 'GestureModes', desc: '自定义手势控制模式画面设置\n保护您的隐私' },
  { id: 'game_setting', file: '/assets/screenshots/game_setting.png', label: 'Setting', desc: '按ESC快速调出丰富游戏设置\n<span style="font-size: 28px;opacity: 0.75">更多设置敬请期待。</span>' },
]

const activeShot = ref(0)
const sectionRefs = ref<HTMLElement[]>([])
const gsapContext = ref<gsap.Context | null>(null)

function recordSectionRef(el: Element | ComponentPublicInstance | null, index: number) {
  if (!el)
    return
  sectionRefs.value[index] = el as HTMLElement
}

function scrollToSection(index: number) {
  const target = sectionRefs.value[index]
  if (!target)
    return
  gsap.to(window, {
    duration: 1,
    scrollTo: { y: target, offsetY: 0 },
    ease: 'power3.inOut',
  })
  activeShot.value = index
}

function initGsapAnimations() {
  gsapContext.value = gsap.context(() => {
    // Hero 首屏入场动画
    gsap.from('.hero-content', {
      opacity: 0,
      y: 80,
      duration: 1.2,
      ease: 'power3.out',
    })

    gsap.from('.hero-content .stat', {
      opacity: 0,
      y: 50,
      stagger: 0.15,
      duration: 0.8,
      delay: 0.4,
      ease: 'power2.out',
    })

    // 特性卡片滚动动画
    gsap.utils.toArray('.highlight-card').forEach((card: any, i) => {
      gsap.from(card, {
        scrollTrigger: {
          trigger: card,
          start: 'top 85%',
          end: 'top 50%',
          toggleActions: 'play none none reverse',
        },
        opacity: 0,
        y: 60,
        scale: 0.95,
        duration: 0.8,
        delay: i * 0.1,
        ease: 'power2.out',
      })
    })

    // 截图区块丝滑视差动画
    sectionRefs.value.forEach((section, index) => {
      if (!section)
        return

      const shotText = section.querySelector('.shot-text')
      const shotFigure = section.querySelector('.shot-figure')
      const shotBg = section.querySelector('.shot-bg')
      const shotWrapper = section.querySelector('.shot-wrapper')
      const shotIndex = section.querySelector('.shot-index')

      // 创建 timeline
      const tl = gsap.timeline({
        scrollTrigger: {
          trigger: section,
          start: 'top 80%',
          end: 'top 20%',
          scrub: 0.8,
          onEnter: () => {
            activeShot.value = index
            section.classList.add('active')
          },
          onLeaveBack: () => {
            if (index > 0)
              activeShot.value = index - 1
            section.classList.remove('active')
          },
        },
      })

      // 文字从左侧滑入
      if (shotText) {
        tl.from(shotText, {
          x: -80,
          opacity: 0,
          duration: 1,
          ease: 'power2.out',
        }, 0)
      }

      // 图片从右侧滑入 + 缩放
      if (shotFigure) {
        tl.from(shotFigure, {
          // x: 100,
          opacity: 0,
          scale: 0.9,
          duration: 1,
          ease: 'power2.out',
        }, 0.1)
      }

      // 背景渐显 + 轻微放大
      if (shotBg) {
        tl.from(shotBg, {
          scale: 1.25,
          opacity: 0,
          duration: 1.2,
          ease: 'power1.out',
        }, 0)
      }

      if (shotWrapper) {
        tl.from(shotWrapper, {
          scale: 0.95,
          opacity: 0,
          duration: 1.2,
          ease: 'power1.out',
        }, 0)
      }

      // 序号淡入
      if (shotIndex) {
        tl.from(shotIndex, {
          opacity: 0,
          scale: 0.5,
          duration: 0.8,
          ease: 'back.out(1.5)',
        }, 0.2)
      }

      // 图片视差滚动效果
      if (shotFigure) {
        gsap.to(shotFigure, {
          scrollTrigger: {
            trigger: section,
            start: 'top bottom',
            end: 'bottom top',
            scrub: 1,
          },
          y: -50,
          ease: 'none',
        })
      }
    })

    // 规格卡片动画
    gsap.utils.toArray('.spec-card, .timeline-card').forEach((card: any, i) => {
      gsap.from(card, {
        scrollTrigger: {
          trigger: card,
          start: 'top 85%',
          toggleActions: 'play none none reverse',
        },
        opacity: 0,
        y: 80,
        scale: 0.95,
        duration: 0.9,
        delay: i * 0.15,
        ease: 'power3.out',
      })
    })

    // 滚动提示动画
    gsap.to('.scroll-hint', {
      scrollTrigger: {
        trigger: '.hero-screen',
        start: 'bottom 80%',
        toggleActions: 'play none none reverse',
      },
      opacity: 0,
      y: -20,
      duration: 0.5,
    })
  })
}

onMounted(async () => {
  await nextTick()

  setTimeout(() => {
    initGsapAnimations()
  }, 100)
})

onBeforeUnmount(() => {
  gsapContext.value?.revert()
  ScrollTrigger.getAll().forEach(st => st.kill())
})
</script>

<template>
  <main id="details" class="details-shell">
    <div class="halo halo-green" aria-hidden="true" />
    <div class="halo halo-pink" aria-hidden="true" />

    <!-- 固定侧边导航指示器 -->
    <nav class="side-nav">
      <button
        v-for="(shot, index) in screenshots"
        :key="shot.id"
        type="button"
        :class="{ active: activeShot === index }"
        :aria-label="`跳到 ${shot.label}`"
        @click="scrollToSection(index)"
      >
        <span class="dot" />
        <span class="label">{{ shot.label }}</span>
      </button>
    </nav>

    <!-- Hero 全屏首屏 -->
    <section class="full-screen hero-screen">
      <div class="hero-content">
        <p class="eyebrow">
          GAME DETAILS
        </p>
        <h1>Snake Spirit</h1>
        <p class="lede">
          基于Pygame和opencv打造的新型贪吃蛇小游戏<br>
          经典玩法 手势玩法 双重来袭<br>
          立即下载游玩！
        </p>

        <div class="stats">
          <div class="stat">
            <p class="stat-label">
              累计修复
            </p>
            <p class="stat-value">
              100+
            </p>
            <p class="stat-desc">
              Bugs
            </p>
          </div>
          <div class="stat">
            <p class="stat-label">
              累计完善
            </p>
            <p class="stat-value">
              20+
            </p>
            <p class="stat-desc">
              Issue / 邮件
            </p>
          </div>
          <div class="stat">
            <p class="stat-label">
              平均响应
            </p>
            <p class="stat-value">
              1s
            </p>
            <p class="stat-desc">
              全链路监控
            </p>
          </div>
        </div>

        <div class="scroll-hint">
          <span class="material-symbols-outlined">keyboard_double_arrow_down</span>
          <span>向下滚动</span>
        </div>
      </div>
    </section>

    <!-- 特性介绍全屏 -->
    <section class="full-screen highlights-screen">
      <div class="section-inner">
        <header class="section-header">
          <p class="tag">
            CORE FEATURES
          </p>
          <h2>核心特性</h2>
        </header>
        <div class="highlight-grid">
          <article v-for="(item) in highlights" :key="item.label" class="highlight-card">
            <p class="tag">
              {{ item.label }}
            </p>
            <h3>{{ item.title }}</h3>
            <p>{{ item.desc }}</p>
          </article>
        </div>
      </div>
    </section>

    <section
      v-for="(shot, index) in screenshots"
      :key="shot.id"
      :ref="el => recordSectionRef(el, index)"
      class="full-screen shot-screen"
      :class="{ active: activeShot === index }"
      :style="{ '--shot-bg': `url(${shot.file})` }"
    >
      <div class="shot-bg" aria-hidden="true" />
      <!-- <div class="shot-overlay" aria-hidden="true" /> -->

      <div class="halo halo-green absolute! op-60!" aria-hidden="true" />
      <div class="halo halo-pink absolute! op-60!" aria-hidden="true" />

      <div class="shot-content">
        <div class="shot-text">
          <span class="shot-index">{{ String(index + 1).padStart(2, '0') }}</span>
          <p class="shot-label">
            {{ shot.label }}
          </p>
          <h2 class="shot-title" v-html="shot.desc.replace('\n', '<br />')" />
          <div class="shot-nav">
            <button v-if="index > 0" class="nav-btn" @click="scrollToSection(index - 1)">
              <span class="material-symbols-outlined">arrow_upward</span>
            </button>
            <button v-if="index < screenshots.length - 1" class="nav-btn primary" @click="scrollToSection(index + 1)">
              <span class="material-symbols-outlined">arrow_downward</span>
            </button>
          </div>
        </div>
        <figure class="shot-figure">
          <div class="shot-wrapper">
            <div class="shot-wrapper-bg" />
            <div class="shot-wrapper-shadow" />
          </div>
          <img :src="shot.file" :alt="shot.label" loading="lazy">
        </figure>
      </div>

      <div class="scroll-hint absolute bottom-16 op-50">
        <span class="material-symbols-outlined">keyboard_double_arrow_down</span>
        <span>向下滚动</span>
      </div>
    </section>

    <!-- 规格与路线图全屏 -->
    <section class="full-screen spec-screen">
      <div class="section-inner spec-grid">
        <article class="spec-card">
          <header>
            <p class="tag">
              REQUIREMENTS
            </p>
            <h2>核心依赖</h2>
          </header>
          <dl>
            <div v-for="spec in specs" :key="spec.name" class="spec-row">
              <div class="phase-line">
                <span class="dot" />
              </div>
              <dt>{{ spec.name }}</dt>
              <!-- <dd>{{ spec.value }}</dd> -->
            </div>
          </dl>
        </article>

        <article class="timeline-card">
          <header>
            <p class="tag">
              ROADMAP
            </p>
            <h2>版本迭代</h2>
          </header>
          <ul>
            <li v-for="milestone in milestones" :key="milestone.phase">
              <div class="phase-line">
                <span class="dot" />
                <span class="divider" />
              </div>
              <div class="phase-copy">
                <div class="phase-head">
                  <span class="phase">{{ milestone.phase }}</span>
                  <span class="badge">{{ milestone.badge }}</span>
                </div>
                <p>{{ milestone.detail }}</p>
              </div>
            </li>
          </ul>
        </article>
      </div>
    </section>
  </main>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Permanent+Marker&family=Rajdhani:wght@500;700&display=swap');

:global(body) {
  background: #050505;
  overflow-x: hidden;
}

/**
 * `@property` is required for the animation to work.
 * Without it, the angle values won’t interpolate properly.
 *
 * @see https://dev.to/afif/we-can-finally-animate-css-gradient-kdk
 */
@property --angle {
  inherits: false;
  initial-value: 0deg;
  syntax: '<angle>';
}

/**
 * To animate the gradient, we set the custom property to 1 full
 * rotation. The animation starts at the default value of `0deg`.
 */
@keyframes spin {
  to {
    --angle: 360deg;
  }
}

.shot-wrapper {
  z-index: -1;
  position: absolute;

  top: 0;
  left: 0;

  width: 100%;
  height: 100%;

  border-radius: 24px;

  /* overflow: hidden; */
}

@keyframes rainbow-border {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

.shot-wrapper-bg {
  position: absolute;
  padding: 0.125rem;

  --dis: 0rem;

  top: var(--dis);
  left: var(--dis);

  width: calc(100% - calc(var(--dis) * 2));
  height: calc(100% - calc(var(--dis) * 2));

  border-radius: 24px;
  background: linear-gradient(
    135deg,
    #ff6b6b 0%,
    #feca57 17%,
    #48dbfb 34%,
    #ff9ff3 51%,
    #54a0ff 68%,
    #5f27cd 85%,
    #ff6b6b 100%
  );
  background-size: 300% 300%;
  animation: rainbow-border 4s ease infinite;
  -webkit-mask:
    linear-gradient(#fff 0 0) content-box,
    linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  pointer-events: none;
}

.shot-wrapper-shadow {
  z-index: 1;
  content: '';
  position: absolute;

  top: 0;
  left: 0;

  width: 100%;
  height: 100%;

  border-radius: 24px;
  background: linear-gradient(
    135deg,
    #ff6b6b 0%,
    #feca57 17%,
    #48dbfb 34%,
    #ff9ff3 51%,
    #54a0ff 68%,
    #5f27cd 85%,
    #ff6b6b 100%
  );
  background-size: 300% 300%;
  animation: rainbow-border 4s ease infinite;
  pointer-events: none;

  filter: blur(12px);

  transform: scale(1);
}
/* ========== 基础容器 ========== */
.details-shell {
  position: relative;
  color: #fff;
  font-family: 'Rajdhani', sans-serif;
  background: #050505;
}

/* ========== 光晕背景 ========== */
.halo {
  position: fixed;
  width: 600px;
  height: 600px;
  filter: blur(120px);
  opacity: 0.6;
  mix-blend-mode: screen;
  pointer-events: none;
  z-index: 0;
}

.halo-green {
  top: -150px;
  left: -150px;
  background: radial-gradient(circle, rgba(74, 222, 128, 0.6), transparent 65%);
  animation: drift 20s ease-in-out infinite alternate;
}

.halo-pink {
  bottom: -150px;
  right: -150px;
  background: radial-gradient(circle, rgba(236, 72, 153, 0.5), transparent 65%);
  animation: drift 20s ease-in-out infinite alternate-reverse;
}

/* ========== 侧边导航 ========== */
.side-nav {
  position: fixed;
  right: 2rem;
  top: 50%;
  transform: translateY(-50%);
  z-index: 100;
  display: flex;
  flex-direction: column;
  gap: 1rem;

  opacity: 0;
  transition: all 0.3s ease;

  &:hover {
    opacity: 1;
  }
}

.side-nav button {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
  transition: all 0.3s ease;
}

.side-nav .dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  border: 2px solid rgba(255, 255, 255, 0.3);
  transition: all 0.3s ease;
  flex-shrink: 0;
}

.side-nav .label {
  font-size: 0.75rem;
  letter-spacing: 0.15em;
  color: rgba(255, 255, 255, 0);
  white-space: nowrap;
  transition: all 0.3s ease;
  transform: translateX(-10px);
}

.side-nav button:hover .label,
.side-nav button.active .label {
  color: rgba(255, 255, 255, 0.8);
  transform: translateX(0);
}

.side-nav button.active .dot {
  background: #4ade80;
  border-color: #4ade80;
  box-shadow: 0 0 20px rgba(74, 222, 128, 0.6);
}

/* ========== 全屏区块基础 ========== */
.full-screen {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  padding: clamp(4rem, 8vw, 8rem) clamp(2rem, 6vw, 6rem);
  scroll-snap-align: start;
}

/* ========== GSAP 动画容器 ========== */
.hero-content,
.highlight-card,
.shot-text,
.shot-figure,
.shot-bg,
.shot-index,
.spec-card,
.timeline-card {
  will-change: transform, opacity;
}

/* ========== Hero 首屏 ========== */
.hero-screen {
  background: radial-gradient(ellipse 80% 50% at 50% 0%, rgba(74, 222, 128, 0.12), transparent),
    radial-gradient(ellipse 60% 40% at 80% 100%, rgba(236, 72, 153, 0.08), transparent);
}

.hero-content {
  text-align: center;
  max-width: 900px;
}

.eyebrow {
  font-family: 'Permanent Marker', cursive;
  letter-spacing: 0.5em;
  font-size: 0.9rem;
  color: #4ade80;
  margin-bottom: 1.5rem;
}

.hero-content h1 {
  margin: 0;
  font-size: clamp(2.5rem, 7vw, 5rem);
  letter-spacing: 0.1em;
  line-height: 1.1;
  background: linear-gradient(135deg, #fff 0%, rgba(255, 255, 255, 0.7) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.lede {
  margin-top: 2rem;
  font-size: clamp(1rem, 2vw, 1.35rem);
  color: rgba(255, 255, 255, 0.7);
  line-height: 2;
}

.stats {
  margin-top: clamp(3rem, 6vw, 5rem);
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
}

.stat {
  padding: 1.5rem;
  border-radius: 1.25rem;
  border: 1px solid rgba(255, 255, 255, 0.08);
  background: rgba(255, 255, 255, 0.02);
  backdrop-filter: blur(10px);
}

.stat-label {
  letter-spacing: 0.3em;
  font-size: 0.7rem;
  color: rgba(255, 255, 255, 0.5);
  margin: 0;
}

.stat-value {
  margin: 0.75rem 0 0.25rem;
  font-size: clamp(2rem, 4vw, 3rem);
  letter-spacing: 0.05em;
  color: #fff;
  font-weight: 700;
}

.stat-desc {
  margin: 0;
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.5);
}

.scroll-hint {
  margin-top: 4rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  color: rgba(255, 255, 255, 0.4);
  font-size: 0.8rem;
  letter-spacing: 0.2em;
  animation: bounce 2s infinite;
}

.scroll-hint .material-symbols-outlined {
  font-size: 2rem;
}

@keyframes bounce {
  0%,
  20%,
  50%,
  80%,
  100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(10px);
  }
  60% {
    transform: translateY(5px);
  }
}

/* ========== 特性区块 ========== */
.highlights-screen {
  background: radial-gradient(ellipse 70% 50% at 20% 50%, rgba(74, 222, 128, 0.08), transparent);
}

.section-inner {
  width: 100%;
  max-width: 1200px;
}

.section-header {
  text-align: center;
  margin-bottom: 3rem;
}

.section-header h2 {
  margin: 0.5rem 0 0;
  font-size: clamp(2rem, 5vw, 3.5rem);
  letter-spacing: 0.08em;
}

.tag {
  letter-spacing: 0.5em;
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.45);
  margin: 0;
}

.highlight-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.highlight-card {
  border-radius: 1.5rem;
  padding: 2.5rem;
  border: 1px solid rgba(255, 255, 255, 0.08);
  background: rgba(10, 10, 10, 0.8);
  backdrop-filter: blur(20px);
  display: flex;
  flex-direction: column;
  gap: 1rem;
  transition:
    transform 0.4s ease,
    border-color 0.4s ease;
}

.highlight-card:hover {
  transform: translateY(-8px);
  border-color: rgba(74, 222, 128, 0.3);
}

.highlight-card h3 {
  margin: 0;
  font-size: 1.5rem;
  letter-spacing: 0.05em;
}

.highlight-card p {
  margin: 0;
  color: rgba(255, 255, 255, 0.65);
  line-height: 1.8;
}

/* ========== 截图全屏区块 ========== */
.shot-screen {
  position: relative;
  /* overflow: hidden; */
}

.shot-bg {
  position: absolute;
  inset: -50px;
  background-image: var(--shot-bg);
  background-size: cover;
  background-position: center;
  filter: blur(80px) saturate(1.2);
  opacity: 0.125;
  transform: scale(1.1);
  transition: opacity 0.8s ease;
}

.shot-screen.active .shot-bg {
  opacity: 0.4;
}

.shot-overlay {
  position: absolute;
  inset: 0;
  background: radial-gradient(ellipse 80% 60% at 30% 50%, rgba(5, 5, 5, 0.3), rgba(5, 5, 5, 0.9));
}

.shot-content {
  position: relative;
  z-index: 1;
  display: grid;
  grid-template-columns: 1fr 1.2fr;
  gap: clamp(3rem, 6vw, 6rem);
  align-items: center;
  width: 100%;
  max-width: 1400px;
}

.shot-text {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.shot-index {
  font-size: clamp(4rem, 10vw, 8rem);
  font-weight: 700;
  letter-spacing: 0.1em;
  color: rgba(255, 255, 255, 0.2);
  line-height: 1;
}

.shot-label {
  margin: 0;
  letter-spacing: 0.4em;
  font-size: 0.8rem;
  color: #4ade80;
}

.shot-title {
  margin: 0;
  font-size: clamp(1.5rem, 3vw, 2.5rem);
  letter-spacing: 0.05em;
  line-height: 1.4;
  color: #f8fafc;
}

.shot-nav {
  display: flex;
  gap: 1rem;
  margin-top: 1.5rem;
}

.nav-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  border-radius: 999px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  background: rgba(255, 255, 255, 0.05);
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.85rem;
  letter-spacing: 0.15em;
  cursor: pointer;
  transition: all 0.3s ease;
}

.nav-btn:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.4);
}

.nav-btn.primary {
  background: rgba(74, 222, 128, 0.15);
  border-color: rgba(74, 222, 128, 0.4);
  color: #4ade80;
}

.nav-btn.primary:hover {
  background: rgba(74, 222, 128, 0.25);
}

.nav-btn .material-symbols-outlined {
  font-size: 1.1rem;
}

.shot-figure {
  margin: 0;
  position: relative;
  transform-origin: center;
  transition: transform 0.6s ease;
}

.shot-figure::before {
  content: '';
  position: absolute;
  inset: -20px;
  background: radial-gradient(ellipse at center, rgba(74, 222, 128, 0.15), transparent 70%);
  border-radius: 2rem;
  z-index: -1;
}

.shot-figure img {
  width: 100%;
  display: block;
  border-radius: 1.5rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 40px 100px rgba(0, 0, 0, 0.5);
}

.shot-screen.active .shot-figure {
  transform: scale(1.02);
}

/* ========== 规格区块 ========== */
.spec-screen {
  background: radial-gradient(ellipse 60% 40% at 80% 50%, rgba(236, 72, 153, 0.06), transparent);
}

.spec-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2.5rem;
}

.spec-card,
.timeline-card {
  border-radius: 1.75rem;
  padding: clamp(2rem, 4vw, 3rem);
  border: 1px solid rgba(255, 255, 255, 0.08);
  background: rgba(10, 10, 10, 0.8);
  backdrop-filter: blur(20px);
}

.spec-card header h2,
.timeline-card header h2 {
  margin: 0.75rem 0 1.5rem;
  font-size: clamp(1.75rem, 3vw, 2.25rem);
  letter-spacing: 0.05em;
}

dl {
  margin: 0;
  display: grid;

  grid-template-columns: 1fr 1fr;
}

.spec-row {
  display: flex;
  padding: 0.25rem 0;

  align-items: center;
  /* border-bottom: 1px solid rgba(255, 255, 255, 0.06); */
}

.spec-row:last-child {
  border-bottom: 0;
}

dt {
  font-size: 1.75rem;
  letter-spacing: 0.3em;
  color: rgba(255, 255, 255, 0.85);
}

/* dd {
  margin: 0.5rem 0 0;
  font-size: 1.1rem;
  color: #f8fafc;
} */

.timeline-card ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.timeline-card li {
  display: flex;
}

.phase-line {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-right: 1.25rem;
}

.phase-line .dot {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: linear-gradient(145deg, #4ade80, #22c55e);
  box-shadow: 0 0 20px rgba(34, 197, 94, 0.5);
  flex-shrink: 0;
}

.divider {
  flex: 1;
  width: 2px;
  margin-top: 0.75rem;
  background: linear-gradient(to bottom, rgba(74, 222, 128, 0.4), transparent);
}

.phase-copy {
  flex: 1;
  padding-top: 0.1rem;
}

.phase-head {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.phase {
  letter-spacing: 0.4em;
  font-size: 0.9rem;
  font-weight: 600;
}

.badge {
  padding: 0.2rem 0.75rem;
  border-radius: 999px;
  font-size: 0.7rem;
  letter-spacing: 0.1em;
  background: rgba(74, 222, 128, 0.12);
  border: 1px solid rgba(74, 222, 128, 0.35);
  color: #4ade80;
}

.phase-copy p {
  margin: 0.5rem 0 0;
  color: rgba(255, 255, 255, 0.65);
  line-height: 1.7;
}

/* ========== 动画 ========== */
@keyframes drift {
  from {
    transform: translate(0, 0) scale(1);
  }
  to {
    transform: translate(30px, -30px) scale(1.1);
  }
}

/* ========== 响应式 ========== */
@media (max-width: 1024px) {
  .shot-content {
    grid-template-columns: 1fr;
    text-align: center;
  }

  .shot-text {
    align-items: center;
  }

  .shot-nav {
    justify-content: center;
  }

  .side-nav {
    right: 1rem;
  }

  .side-nav .label {
    display: none;
  }
}

@media (max-width: 768px) {
  .stats {
    grid-template-columns: 1fr;
  }

  .full-screen {
    padding: 6rem 1.5rem;
    min-height: auto;
  }

  .side-nav {
    display: none;
  }

  .shot-index {
    font-size: 3rem;
  }

  .highlight-grid {
    grid-template-columns: 1fr;
  }

  .spec-grid {
    grid-template-columns: 1fr;
  }
}
</style>
