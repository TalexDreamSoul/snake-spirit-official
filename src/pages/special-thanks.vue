<script setup lang="ts">
const players = ['刘欣荣', '唐子贤', '伍鑫宇', '黄俊伟', '廖子然', '蔡子菱']
</script>

<template>
  <main class="thanks-shell">
    <div class="bg-grid" aria-hidden="true" />
    <div class="ink-splash splash-1" aria-hidden="true" />
    <div class="ink-splash splash-2" aria-hidden="true" />

    <section class="thanks-card">
      <p class="eyebrow">
        SPECIAL THANKS
      </p>
      <h1>特别感谢</h1>
      <p class="intro">
        游戏内测时提出宝贵建议的玩家：
      </p>

      <ul class="player-list">
        <li v-for="player in players" :key="player">
          <span class="player-dot" aria-hidden="true" />
          {{ player }}
        </li>
      </ul>

      <p class="outro">
        感谢唐子贤为本游戏网页做出的巨大贡献，我们会让每一次墨迹都留在赛博卷轴上。
      </p>

      <div class="badge-row">
        <span>Gesture Dev</span>
        <span>Inner Testers</span>
        <span>SnakeSpirit Crew</span>
      </div>
    </section>
  </main>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Permanent+Marker&family=Rajdhani:wght@500;700&display=swap');

.thanks-shell {
  min-height: 100vh;
  background: #050505;
  color: #fff;
  font-family: 'Rajdhani', sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: clamp(2rem, 6vw, 4rem);
  position: relative;
  overflow: hidden;
}

.bg-grid {
  position: absolute;
  inset: 0;
  background-image: linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
  background-size: 140px 140px;
  opacity: 0.6;
  animation: drift 24s linear infinite;
}

.ink-splash {
  position: absolute;
  width: 320px;
  height: 320px;
  border-radius: 50%;
  filter: blur(20px);
  opacity: 0.5;
  mix-blend-mode: screen;
  animation: float 12s ease-in-out infinite;
}

.splash-1 {
  background: radial-gradient(circle, rgba(34, 197, 94, 0.3), transparent 70%);
  top: 10%;
  left: 8%;
}

.splash-2 {
  background: radial-gradient(circle, rgba(236, 72, 153, 0.25), transparent 70%);
  bottom: 5%;
  right: 12%;
  animation-delay: 4s;
}

.thanks-card {
  position: relative;
  max-width: 720px;
  width: 100%;
  border-radius: 2rem;
  padding: clamp(2rem, 4vw, 3rem);
  background: rgba(5, 5, 5, 0.75);
  border: 1px solid rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(20px);
  box-shadow: 0 40px 70px rgba(0, 0, 0, 0.5);
  overflow: hidden;
}

.thanks-card::after {
  content: '';
  position: absolute;
  inset: 2px;
  border-radius: inherit;
  border: 1px solid transparent;
  background: linear-gradient(120deg, rgba(74, 222, 128, 0.35), rgba(236, 72, 153, 0.35));
  mask:
    linear-gradient(#fff 0 0) content-box,
    linear-gradient(#fff 0 0);
  mask-composite: exclude;
  pointer-events: none;
}

.eyebrow {
  font-family: 'Permanent Marker', cursive;
  letter-spacing: 0.35em;
  color: #4ade80;
  text-transform: uppercase;
  margin-bottom: 0.5rem;
}

h1 {
  font-size: clamp(2.5rem, 8vw, 4rem);
  margin: 0;
  letter-spacing: 0.15em;
}

.intro {
  margin-top: 1rem;
  color: rgba(255, 255, 255, 0.8);
  font-size: 1.25rem;
}

.player-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 1rem;
  list-style: none;
  padding: 2rem 0 0;
  margin: 0;
}

.player-list li {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-size: 1.4rem;
  letter-spacing: 0.05em;
}

.player-dot {
  width: 0.85rem;
  height: 0.85rem;
  border-radius: 50%;
  background: radial-gradient(circle, #4ade80, transparent);
  animation: pulse 1.8s infinite;
}

.outro {
  margin-top: 2rem;
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.75);
  line-height: 1.8;
}

.badge-row {
  margin-top: 1.5rem;
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.badge-row span {
  border: 1px solid rgba(74, 222, 128, 0.6);
  padding: 0.35rem 1.25rem;
  border-radius: 999px;
  letter-spacing: 0.25em;
  font-size: 0.75rem;
  text-transform: uppercase;
}

@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
    opacity: 1;
  }

  50% {
    transform: scale(1.2);
    opacity: 0.4;
  }
}

@keyframes drift {
  from {
    transform: translateY(0);
  }

  to {
    transform: translateY(140px);
  }
}

@keyframes float {
  0% {
    transform: translate3d(0, 0, 0);
  }

  50% {
    transform: translate3d(0, -40px, 0);
  }

  100% {
    transform: translate3d(0, 0, 0);
  }
}

@media (max-width: 640px) {
  .player-list {
    grid-template-columns: 1fr;
  }

  .player-list li {
    font-size: 1.2rem;
  }
}
</style>
