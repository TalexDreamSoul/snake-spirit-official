<script setup lang="ts">
const githubIssueUrl = 'https://github.com/TalexDreamSoul/snake-spirit-official/issues'
const emailAddress = 'phasisx_pdf_pxf@163.com'

const quickTips = [
  '一句话写清问题 + 设备/版本。',
  '复现步骤按 1、2、3 列出来。',
  '附截图或录屏，并写期望结果。',
]
</script>

<template>
  <main id="feedback" class="feedback-shell">
    <div class="constellation" aria-hidden="true" />
    <div class="laser laser-1" aria-hidden="true" />
    <div class="laser laser-2" aria-hidden="true" />

    <section class="hero-card">
      <p class="eyebrow">
        FEEDBACK
      </p>
      <h1>Snake Spirit 快速反馈</h1>
      <p class="lead">
        直接照着首页的干脆节奏来，BUG、体验槽点或灵感都往这丢。GitHub 走流程，邮箱秒发，24 小时内必读。
      </p>
    </section>

    <section class="channel-grid">
      <article class="channel-card">
        <header>
          <p class="channel-tag">
            GITHUB · 推荐
          </p>
          <h2>提一个 Issue</h2>
        </header>
        <p>
          最推荐的方式。创建 Issue 带上标签，能看到分派、修复、回归的完整链路，需要跟进的直接 @。
        </p>
        <a class="cta" :href="githubIssueUrl" target="_blank" rel="noopener">
          打开 GitHub
          <span aria-hidden="true">↗</span>
        </a>
      </article>

      <article class="channel-card">
        <header>
          <p class="channel-tag">
            EMAIL · 便捷
          </p>
          <h2>发封邮件</h2>
        </header>
        <p>
          不想登录 GitHub？直接写邮件到 <strong>{{ emailAddress }}</strong>，顺带塞截图和日志，收到后我们会帮你录入。
        </p>
        <a class="cta" :href="`mailto:${emailAddress}`">
          撰写邮件
          <span aria-hidden="true">✉</span>
        </a>
      </article>
    </section>

    <section v-if="false" class="tips-card">
      <h3>写反馈就看这 3 条</h3>
      <ul>
        <li v-for="tip in quickTips" :key="tip">
          <span class="bullet" aria-hidden="true" />
          {{ tip }}
        </li>
      </ul>
    </section>
  </main>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Permanent+Marker&family=Rajdhani:wght@500;700&display=swap');

:global(body) {
  background: #050505;
}

.feedback-shell {
  position: relative;
  min-height: calc(100vh - 80px);
  font-family: 'Rajdhani', sans-serif;
  background: radial-gradient(circle at top, rgba(74, 222, 128, 0.15), transparent 45%), #050505;
  color: #fff;
  padding: clamp(1.5rem, 5vw, 3.5rem);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  gap: clamp(2rem, 4vw, 3.5rem);
}

.constellation {
  position: absolute;
  inset: 0;
  background-image: linear-gradient(rgba(255, 255, 255, 0.04) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.04) 1px, transparent 1px);
  background-size: 180px 180px;
  opacity: 0.4;
  animation: slow-pan 30s linear infinite;
  pointer-events: none;
}

.laser {
  position: absolute;
  width: 320px;
  height: 320px;
  border-radius: 50%;
  filter: blur(45px);
  opacity: 0.45;
  mix-blend-mode: screen;
  pointer-events: none;
}

.laser-1 {
  background: radial-gradient(circle, rgba(74, 222, 128, 0.4), transparent 65%);
  top: 10%;
  left: 6%;
}

.laser-2 {
  background: radial-gradient(circle, rgba(236, 72, 153, 0.35), transparent 70%);
  bottom: 5%;
  right: 12%;
}

.hero-card {
  position: relative;
  z-index: 1;
  border-radius: 1.75rem;
  padding: clamp(1.75rem, 4vw, 3rem);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.08);
  background: rgba(5, 5, 5, 0.75);
  box-shadow: 0 40px 80px rgba(0, 0, 0, 0.45);
}

.eyebrow {
  font-family: 'Permanent Marker', cursive;
  letter-spacing: 0.4em;
  color: #4ade80;
  margin-bottom: 0.8rem;
}

.hero-card h1 {
  margin: 0;
  font-size: clamp(2.4rem, 6vw, 4rem);
  letter-spacing: 0.15em;
}

.lead {
  margin-top: 1rem;
  font-size: 1.2rem;
  color: rgba(255, 255, 255, 0.85);
  line-height: 1.7;
}

.channel-grid {
  position: relative;
  z-index: 1;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: clamp(1.25rem, 4vw, 2.25rem);
}

.channel-card {
  border-radius: 1.5rem;
  padding: clamp(1.5rem, 3vw, 2.25rem);
  background: rgba(5, 5, 5, 0.85);
  border: 1px solid rgba(255, 255, 255, 0.08);
  min-height: 240px;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.channel-card h2 {
  margin: 0.15rem 0 0;
  font-size: 2rem;
}

.channel-card p {
  flex: 1;
  color: rgba(255, 255, 255, 0.78);
  line-height: 1.7;
}

.channel-tag {
  letter-spacing: 0.35em;
  color: rgba(255, 255, 255, 0.55);
  font-size: 0.75rem;
}

.cta {
  align-self: flex-start;
  display: inline-flex;
  align-items: center;
  gap: 0.35rem;
  padding: 0.5rem 1.5rem;
  border-radius: 999px;
  border: 1px solid rgba(74, 222, 128, 0.6);
  color: #4ade80;
  letter-spacing: 0.2em;
  font-size: 0.85rem;
  text-transform: uppercase;
  transition: all 0.25s ease;
}

.cta:hover {
  background: #22c55e;
  color: #050505;
}

.tips-card {
  position: relative;
  z-index: 1;
  border-radius: 1.25rem;
  border: 1px solid rgba(255, 255, 255, 0.08);
  padding: clamp(1.25rem, 4vw, 2rem);
  background: rgba(5, 5, 5, 0.85);
}

.tips-card h3 {
  margin: 0;
  font-size: 1.6rem;
  letter-spacing: 0.08em;
}

.tips-card ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.tips-card li {
  display: flex;
  gap: 0.75rem;
  align-items: flex-start;
  line-height: 1.6;
}

.bullet {
  width: 0.8rem;
  height: 0.8rem;
  border-radius: 50%;
  background: radial-gradient(circle, #ec4899, transparent);
  flex-shrink: 0;
  margin-top: 0.4rem;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
    opacity: 1;
  }

  50% {
    transform: scale(1.25);
    opacity: 0.6;
  }
}

@keyframes slow-pan {
  0% {
    transform: translate(0, 0);
  }

  100% {
    transform: translate(0, 180px);
  }
}

@media (max-width: 640px) {
  .tips-card {
    padding: 1.25rem;
  }
}
</style>
