<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'

interface MouseState {
  x: number
  y: number
  prevX: number
  prevY: number
  speed: number
}

interface SceneConfig {
  gridSize: number
  snakeColor: string
  snakeGlow: string
  secondaryColor: string
  segmentDist: number
}

interface Dimensions {
  w: number
  h: number
}

interface SceneContext {
  dims: Dimensions
  mouse: MouseState
  config: SceneConfig
}

interface Segment {
  x: number
  y: number
}

const stats = [
  { id: 'players', value: '10M+', label: '全球指划玩家' },
  { id: 'rating', value: '4.9', label: '平均评分' },
  { id: 'league', value: 'TOP 1', label: '手势电竞联赛' },
]

const canvasRef = ref<HTMLCanvasElement | null>(null)
let cleanupScene: (() => void) | undefined

onMounted(() => {
  if (!canvasRef.value)
    return
  cleanupScene = createScene(canvasRef.value) ?? undefined
})

onBeforeUnmount(() => {
  cleanupScene?.()
  cleanupScene = undefined
})

function createScene(canvas: HTMLCanvasElement) {
  const ctx = canvas.getContext('2d', { alpha: false })
  if (!ctx)
    return undefined

  const dims: Dimensions = {
    w: window.innerWidth,
    h: window.innerHeight,
  }
  const mouse: MouseState = {
    x: dims.w / 2,
    y: dims.h / 2,
    prevX: dims.w / 2,
    prevY: dims.h / 2,
    speed: 0,
  }
  const config: SceneConfig = {
    gridSize: 60,
    snakeColor: '#4ade80',
    snakeGlow: '#22c55e',
    secondaryColor: '#ec4899',
    segmentDist: 8,
  }
  const scene: SceneContext = { dims, mouse, config }

  const snake = new Snake(scene)
  const grid = new Grid(scene)
  const runes = Array.from({ length: 15 }, () => new Rune(scene))

  const resize = () => {
    scene.dims.w = canvas.width = window.innerWidth
    scene.dims.h = canvas.height = window.innerHeight
    snake.resetPosition()
  }

  const handleMouse = (event: MouseEvent) => {
    mouse.prevX = mouse.x
    mouse.prevY = mouse.y
    mouse.x = event.clientX
    mouse.y = event.clientY
    const dx = mouse.x - mouse.prevX
    const dy = mouse.y - mouse.prevY
    mouse.speed = Math.sqrt(dx * dx + dy * dy)
  }

  let frameId = 0
  const animate = () => {
    ctx.fillStyle = 'rgba(5, 5, 5, 0.3)'
    ctx.fillRect(0, 0, scene.dims.w, scene.dims.h)

    grid.draw(ctx)
    runes.forEach((rune) => {
      rune.update()
      rune.draw(ctx)
    })

    snake.update()
    snake.draw(ctx)
    frameId = window.requestAnimationFrame(animate)
  }

  window.addEventListener('resize', resize)
  window.addEventListener('mousemove', handleMouse)

  resize()
  frameId = window.requestAnimationFrame(animate)

  return () => {
    window.cancelAnimationFrame(frameId)
    window.removeEventListener('resize', resize)
    window.removeEventListener('mousemove', handleMouse)
  }
}

class Snake {
  private segments: Segment[]
  private angle = 0

  constructor(private scene: SceneContext) {
    this.segments = Array.from({ length: 50 }, () => ({
      x: this.scene.dims.w / 2,
      y: this.scene.dims.h / 2,
    }))
  }

  resetPosition() {
    this.segments.forEach((segment) => {
      segment.x = this.scene.dims.w / 2
      segment.y = this.scene.dims.h / 2
    })
  }

  update() {
    const head = this.segments[0]
    const dx = this.scene.mouse.x - head.x
    const dy = this.scene.mouse.y - head.y
    this.angle = Math.atan2(dy, dx)
    head.x += dx * 0.15
    head.y += dy * 0.15

    for (let i = 1; i < this.segments.length; i++) {
      const current = this.segments[i]
      const previous = this.segments[i - 1]
      const distX = previous.x - current.x
      const distY = previous.y - current.y
      const dist = Math.sqrt(distX * distX + distY * distY)
      if (dist > this.scene.config.segmentDist) {
        const direction = Math.atan2(distY, distX)
        current.x = previous.x - Math.cos(direction) * this.scene.config.segmentDist
        current.y = previous.y - Math.sin(direction) * this.scene.config.segmentDist
      }
    }
  }

  draw(ctx: CanvasRenderingContext2D) {
    ctx.save()
    ctx.shadowBlur = 20
    ctx.shadowColor = this.scene.config.snakeGlow
    ctx.strokeStyle = this.scene.config.snakeColor
    ctx.lineCap = 'round'
    ctx.lineWidth = 6 + Math.min(this.scene.mouse.speed * 0.2, 10)
    ctx.beginPath()
    ctx.moveTo(this.segments[0].x, this.segments[0].y)
    for (let i = 1; i < this.segments.length - 2; i++) {
      const xc = (this.segments[i].x + this.segments[i + 1].x) / 2
      const yc = (this.segments[i].y + this.segments[i + 1].y) / 2
      ctx.quadraticCurveTo(this.segments[i].x, this.segments[i].y, xc, yc)
    }
    ctx.stroke()
    ctx.restore()

    ctx.save()
    ctx.strokeStyle = '#fff'
    ctx.lineWidth = 1
    ctx.beginPath()
    for (let i = 0; i < this.segments.length - 1; i += 2) {
      const segment = this.segments[i]
      const jitterX = (Math.random() - 0.5) * 6
      const jitterY = (Math.random() - 0.5) * 6
      if (i === 0)
        ctx.moveTo(segment.x + jitterX, segment.y + jitterY)
      else ctx.lineTo(segment.x + jitterX, segment.y + jitterY)
    }
    ctx.globalAlpha = 0.6
    ctx.stroke()

    ctx.beginPath()
    for (let i = 0; i < this.segments.length; i += 4) {
      const segment = this.segments[i]
      const perpendicular = this.angle + Math.PI / 2
      const size = 6
      ctx.moveTo(
        segment.x - Math.cos(perpendicular) * size,
        segment.y - Math.sin(perpendicular) * size,
      )
      ctx.lineTo(
        segment.x + Math.cos(perpendicular) * size,
        segment.y + Math.sin(perpendicular) * size,
      )
    }
    ctx.globalAlpha = 0.4
    ctx.stroke()
    ctx.restore()

    const head = this.segments[0]
    this.drawSketchyCircle(ctx, head.x, head.y, 10, '#fff')
  }

  private drawSketchyCircle(
    ctx: CanvasRenderingContext2D,
    x: number,
    y: number,
    radius: number,
    color: string,
  ) {
    ctx.save()
    ctx.strokeStyle = color
    ctx.lineWidth = 2
    ctx.beginPath()
    for (let i = 0; i < 10; i++) {
      const angle = (i / 10) * Math.PI * 2
      const rad = radius + (Math.random() - 0.5) * 3
      const px = x + Math.cos(angle) * rad
      const py = y + Math.sin(angle) * rad
      if (i === 0)
        ctx.moveTo(px, py)
      else ctx.lineTo(px, py)
    }
    ctx.closePath()
    ctx.stroke()
    ctx.restore()
  }
}

class Grid {
  constructor(private scene: SceneContext) {}

  draw(ctx: CanvasRenderingContext2D) {
    ctx.strokeStyle = 'rgba(74, 222, 128, 0.05)'
    ctx.lineWidth = 1

    const { gridSize } = this.scene.config
    const { w, h } = this.scene.dims

    for (let x = 0; x <= w; x += gridSize) {
      ctx.beginPath()
      for (let y = 0; y <= h; y += 20) {
        const dist = Math.hypot(x - this.scene.mouse.x, y - this.scene.mouse.y)
        let dx = 0
        if (dist < 200) {
          const force = (200 - dist) / 200
          dx = (x - this.scene.mouse.x) * force * 0.2
        }
        if (y === 0)
          ctx.moveTo(x + dx, y)
        else ctx.lineTo(x + dx, y)
      }
      ctx.stroke()
    }

    for (let y = 0; y <= h; y += gridSize) {
      ctx.beginPath()
      for (let x = 0; x <= w; x += 20) {
        const dist = Math.hypot(x - this.scene.mouse.x, y - this.scene.mouse.y)
        let dy = 0
        if (dist < 200) {
          const force = (200 - dist) / 200
          dy = (y - this.scene.mouse.y) * force * 0.2
        }
        if (x === 0)
          ctx.moveTo(x, y + dy)
        else ctx.lineTo(x, y + dy)
      }
      ctx.stroke()
    }
  }
}

class Rune {
  private x = 0
  private y = 0
  private size = 0
  private speedY = 0
  private speedX = 0
  private color = ''
  private rotation = 0

  constructor(private scene: SceneContext) {
    this.reset()
  }

  reset() {
    this.x = Math.random() * this.scene.dims.w
    this.y = Math.random() * this.scene.dims.h
    this.size = Math.random() * 10 + 5
    this.speedY = Math.random() * 0.5 - 0.25
    this.speedX = Math.random() * 0.5 - 0.25
    this.color
      = Math.random() > 0.5
        ? this.scene.config.snakeColor
        : this.scene.config.secondaryColor
    this.rotation = Math.random() * Math.PI
  }

  update() {
    this.x += this.speedX
    this.y += this.speedY
    this.rotation += 0.02

    const dist = Math.hypot(this.x - this.scene.mouse.x, this.y - this.scene.mouse.y)
    if (dist < 50)
      this.reset()

    if (
      this.x < 0
      || this.x > this.scene.dims.w
      || this.y < 0
      || this.y > this.scene.dims.h
    ) {
      this.reset()
    }
  }

  draw(ctx: CanvasRenderingContext2D) {
    ctx.save()
    ctx.translate(this.x, this.y)
    ctx.rotate(this.rotation)
    ctx.strokeStyle = this.color
    ctx.globalAlpha = 0.4
    ctx.lineWidth = 2
    ctx.beginPath()
    ctx.rect(-this.size, -this.size, this.size * 2, this.size * 2)
    ctx.stroke()
    ctx.restore()
  }
}

const router = useRouter()

function handleNavClick(path: string) {
  router.push(path)
}
</script>

<template>
  <main class="snake-shell">
    <canvas ref="canvasRef" class="canvas-layer" aria-hidden="true" />
    <div class="content-layer">
      <nav class="site-nav">
        <div class="brand">
          <span class="brand-icon material-symbols-outlined" aria-hidden="true">gesture</span>
          <span class="brand-text">SNAKE<span>SPIRIT</span></span>
        </div>
        <div class="nav-links">
          <a href="#download-area">下载渠道</a>
          <a href="#download-area">排行榜</a>
          <a href="#download-area">玩家社区</a>
          <a @click="handleNavClick('/special-thanks')">特别感谢</a>
        </div>
        <button class="nav-button" type="button">
          登录测试服
        </button>
      </nav>

      <section class="hero">
        <div class="annotation">
          <p>像画符一样操控</p>
          <svg width="100" height="80" viewBox="0 0 100 80" aria-hidden="true">
            <path d="M10,10 Q50,5 80,60" />
            <path d="M80,60 L60,55 M80,60 L75,40" />
          </svg>
        </div>

        <div class="title-block">
          <p class="eyebrow">
            THE NEXT HAND-DRAWN EXPERIENCE
          </p>
          <h1 class="glitch headline" data-text="SNAKESPIRIT">
            SNAKESPIRIT
          </h1>
        </div>

        <p class="intro">
          <span class="highlight">蛇灵 SnakeSpirit</span> 把传统贪吃蛇撕得粉碎。
          依托手势识别 AI 引擎，你只需在空气中勾勒线条， 就能驱动一条带有<span
            class="highlight"
          >赛博墨迹</span>的灵蛇在无限画布中穿梭。 手感像写毛笔，速度却能冲进全球排行榜。
        </p>

        <div id="download-area" class="cta-group">
          <div class="cta-halo" aria-hidden="true" />

          <a
            class="cta-card github-card"
            href="https://github.com/pdfgame/Dual-modeSnakeGame"
            target="_blank"
            rel="noreferrer"
          >
            <span class="material-symbols-outlined cta-icon" aria-hidden="true">download</span>
            <div class="cta-copy">
              <span class="cta-eyebrow">Download on</span>
              <span class="cta-title">GitHub</span>
            </div>
            <span class="shine" aria-hidden="true" />
          </a>
        </div>

        <div class="stats-grid">
          <div v-for="stat in stats" :key="stat.id" class="stat-card">
            <div class="stat-value">
              {{ stat.value }}
            </div>
            <div class="stat-label">
              {{ stat.label }}
            </div>
          </div>
        </div>
      </section>

      <div class="tagline">
        Gesture Controlled • Infinite Canvas • Multiplayer Arena
      </div>
    </div>
  </main>
</template>

<route lang="yaml">
meta:
  layout: home
</route>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Permanent+Marker&family=Rajdhani:wght@500;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,1,0');

::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #111;
}

::-webkit-scrollbar-thumb {
  background: #22c55e;
  border-radius: 4px;
}

.snake-shell {
  min-height: 100vh;
  background-color: #050505;
  color: #fff;
  font-family: 'Rajdhani', sans-serif;
  position: relative;
  overflow-x: hidden;
}

.canvas-layer {
  position: fixed;
  inset: 0;
  width: 100vw;
  height: 100vh;
  z-index: 0;
  background: #050505;
}

.content-layer {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  padding-bottom: 3rem;
}

.site-nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem clamp(1.5rem, 5vw, 3.5rem);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(8px);
}

.brand {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.08em;
}

.brand-icon {
  font-size: 2.25rem;
  color: #4ade80;
  animation: pulse 2s infinite;
}

.brand-text span {
  color: #4ade80;
}

.nav-links {
  display: none;
  gap: 2.5rem;
  font-weight: 700;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.6);
}

.nav-links a {
  transition: color 0.3s ease;
}

.nav-links a:hover {
  color: #fff;
}

.nav-button {
  padding: 0.5rem 1.75rem;
  border: 1px solid rgba(34, 197, 94, 0.5);
  border-radius: 999px;
  background: transparent;
  color: #4ade80;
  font-weight: 700;
  letter-spacing: 0.1em;
  transition: all 0.3s ease;
}

.nav-button:hover {
  background: #22c55e;
  color: #050505;
}

.hero {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: clamp(2rem, 6vw, 5rem) clamp(1.5rem, 6vw, 4rem);
  position: relative;
}

.annotation {
  position: absolute;
  top: 18%;
  left: 15%;
  transform: rotate(-10deg);
  opacity: 0.85;
  display: none;
}

.annotation p {
  font-family: 'Permanent Marker', cursive;
  color: #ec4899;
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.annotation path {
  stroke: #ec4899;
  stroke-width: 2;
  fill: none;
  stroke-dasharray: 5 5;
  stroke-linecap: round;
}

.title-block {
  margin-bottom: 2rem;
  mix-blend-mode: screen;
}

.eyebrow {
  color: #4ade80;
  font-family: 'Permanent Marker', cursive;
  letter-spacing: 0.35em;
  font-size: 1.25rem;
  transform: rotate(-2deg);
  display: inline-block;
}

.headline {
  font-size: clamp(3rem, 10vw, 8rem);
  font-weight: 900;
  line-height: 1;
  text-transform: uppercase;
  text-shadow: 0 0 40px rgba(255, 255, 255, 0.4);
  letter-spacing: 0.08em;
}

.glitch {
  position: relative;
  color: transparent;
  background: linear-gradient(180deg, #fff 0%, #9ca3af 100%);
  -webkit-background-clip: text;
}

.glitch::before,
.glitch::after {
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  color: transparent;
  background: #050505;
}

.glitch::before {
  left: 2px;
  text-shadow: -1px 0 #ff00c1;
  clip-path: inset(44px 0 56px 0);
  animation: glitch-anim 5s infinite linear alternate-reverse;
}

.glitch::after {
  left: -2px;
  text-shadow: -1px 0 #00fff9;
  clip-path: inset(44px 0 56px 0);
  animation: glitch-anim2 5s infinite linear alternate-reverse;
}

.intro {
  max-width: 720px;
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.8;
  border-left: 4px solid #22c55e;
  padding: 1rem 1.5rem;
  text-align: left;
  background: rgba(0, 0, 0, 0.35);
  backdrop-filter: blur(10px);
}

.highlight {
  color: #fff;
  font-weight: 700;
}

.cta-group {
  margin-top: 3rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
  position: relative;
  width: 100%;
  max-width: 360px;
  margin-left: auto;
  margin-right: auto;
}

.cta-halo {
  position: absolute;
  inset: -30%;
  border-radius: 999px;
  background: radial-gradient(circle, rgba(34, 197, 94, 0.2), transparent 60%);
  filter: blur(40px);
  pointer-events: none;
}

.cta-card {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 1.25rem;
  width: 100%;
  padding: 1rem 1.25rem;
  border-radius: 1rem;
  border: 1px solid rgba(255, 255, 255, 0.08);
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(12px);
  text-align: left;
  overflow: hidden;
  align-self: center;
  transition:
    transform 0.3s ease,
    border-color 0.3s ease,
    box-shadow 0.3s ease;
}

.cta-card:hover {
  transform: translateY(-4px);
  border-color: rgba(74, 222, 128, 0.6);
  box-shadow: 0 0 30px rgba(34, 197, 94, 0.25);
}

.cta-icon {
  font-size: 2.5rem;
  color: #fff;
}

.cta-copy {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.cta-eyebrow {
  font-size: 0.75rem;
  letter-spacing: 0.35em;
  color: rgba(255, 255, 255, 0.6);
  text-transform: uppercase;
}

.cta-title {
  font-size: 1.5rem;
  font-weight: 700;
}

.shine {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(120deg, transparent, rgba(255, 255, 255, 0.25), transparent);
  transform: skewX(-15deg);
  transition: transform 0.5s ease;
}

.cta-card:hover .shine {
  transform: translateX(200%) skewX(-15deg);
}

.github-card {
  border-color: rgba(147, 197, 253, 0.4);
  background: linear-gradient(120deg, rgba(255, 255, 255, 0.08), rgba(59, 130, 246, 0.1));
}

.github-card .cta-title {
  letter-spacing: 0.15em;
}

.stats-grid {
  margin-top: 3rem;
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1.25rem;
  opacity: 0.8;
}

.stat-card {
  text-align: center;
}

.stat-value {
  font-size: 2rem;
  font-weight: 800;
}

.stat-label {
  font-size: 0.8rem;
  letter-spacing: 0.25em;
  color: #4ade80;
  text-transform: uppercase;
}

.tagline {
  text-align: center;
  letter-spacing: 1em;
  font-size: 0.65rem;
  color: rgba(255, 255, 255, 0.3);
  padding: 1rem;
  margin-top: auto;
}

.material-symbols-outlined {
  font-variation-settings:
    'FILL' 1,
    'wght' 400,
    'GRAD' 0,
    'opsz' 24;
}

@keyframes glitch-anim {
  0% {
    clip-path: inset(15px 0 80px 0);
  }

  20% {
    clip-path: inset(60px 0 20px 0);
  }

  40% {
    clip-path: inset(20px 0 70px 0);
  }

  60% {
    clip-path: inset(80px 0 10px 0);
  }

  100% {
    clip-path: inset(45px 0 45px 0);
  }
}

@keyframes glitch-anim2 {
  0% {
    clip-path: inset(70px 0 5px 0);
  }

  20% {
    clip-path: inset(5px 0 65px 0);
  }

  40% {
    clip-path: inset(50px 0 25px 0);
  }

  60% {
    clip-path: inset(25px 0 50px 0);
  }

  100% {
    clip-path: inset(10px 0 75px 0);
  }
}

@keyframes pulse {
  0%,
  100% {
    opacity: 1;
  }

  50% {
    opacity: 0.4;
  }
}

@media (min-width: 768px) {
  .nav-links {
    display: flex;
  }

  .annotation {
    display: block;
  }

  .cta-group {
    flex-direction: column;
    align-items: center;
  }
}

@media (max-width: 640px) {
  .eyebrow {
    letter-spacing: 0.2em;
  }

  .intro {
    border-left: none;
    border-top: 4px solid #22c55e;
    text-align: center;
  }

  .stats-grid {
    grid-template-columns: repeat(1, minmax(0, 1fr));
  }

  .tagline {
    letter-spacing: 0.5em;
  }
}
</style>
