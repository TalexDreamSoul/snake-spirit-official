<script setup lang="ts">
const pillars = [
  {
    title: '创意破浪e',
    text: '不搞烂大街的套路，套路都是别人，枫寻语团队的创意一出手就能把人拉回游戏的灵魂。',
  },
  {
    title: '速度再造',
    text: '每周部署都像开夜店，不拖泥带水，Bug 和体验槽点一出现就要迅速逼退。',
  },
  {
    title: '温柔守护',
    text: '婆娘说得对，用户体验就是战场卫生，哪怕再暴躁也要把界面打磨到像陶瓷一样光滑。',
  },
]

const members = [
  {
    name: '枫寻语',
    role: '主理人 / 架构疯子',
    focus: '甩锅从不，甩BUG一把梭，一句话把需求拆得比骰子还精准。',
  },
  {
    name: '雾火',
    role: '后端战术',
    focus: 'Python、FastAPI、数据库什么的都能当武器，带着日志去打赢性能战。',
  },
  {
    name: '绿珑',
    role: '界面美术',
    focus: '灯光、粒子都由她调教，黑夜旳霓虹都是她偷偷放的烟花。',
  },
  {
    name: '柳碎',
    role: '体验联络',
    focus: '把玩家说的话记在心里，沟通、翻译、连线，简直比老李修发还细心。',
  },
]

const rituals = [
  '每周五凌晨 2 点「枫语会」，披着夜色复盘一周血泪史。',
  '一起在五金店里搞出新工具，只要有创意就能砍掉重来。',
  '遇到报错谁都不能躲，必须有人站出来把控制台狂骂一遍再修复。',
]
</script>

<template>
  <main id="team" class="team-shell">
    <div class="aurora aurora-1" aria-hidden="true" />
    <div class="aurora aurora-2" aria-hidden="true" />

    <section class="hero-card">
      <p class="eyebrow">TEAM · SNAKE SPIRIT</p>
      <h1>枫寻语 团队</h1>
      <p class="lead">
        这群自称“枫寻语”的疯子在五金店的夜晚里修BUG、拉体验、一指禅打出绚烂感官，
        只要你敢来加入，就要做好跟我们一起把黑夜点亮的准备。
      </p>
      <p class="tagline">设计师说的那叫平衡，老王说的那叫“震撼到爆炸”。</p>
    </section>

    <section class="mission-grid">
      <article class="mission-card" v-for="pillar in pillars" :key="pillar.title">
        <h3>{{ pillar.title }}</h3>
        <p>{{ pillar.text }}</p>
      </article>
    </section>

    <section class="member-section">
      <header>
        <p class="eyebrow">CREW</p>
        <h2>最硬核的队友，相互拆台也要一起上</h2>
      </header>
      <div class="member-grid">
        <article class="member-card" v-for="member in members" :key="member.name">
          <p class="member-role">{{ member.role }}</p>
          <h3>{{ member.name }}</h3>
          <p>{{ member.focus }}</p>
        </article>
      </div>
    </section>

    <section class="ritual-card">
      <h3>秘笈</h3>
      <p class="lead">不是日常任务，是枫寻语团队专属的沉浸 Ritual。</p>
      <ul>
        <li v-for="ritual in rituals" :key="ritual">
          <span class="bullet" aria-hidden="true" />
          {{ ritual }}
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

.team-shell {
  position: relative;
  min-height: calc(100vh - 80px);
  color: #f8fafc;
  font-family: 'Rajdhani', sans-serif;
  padding: clamp(1.5rem, 6vw, 5rem);
  background: radial-gradient(circle at 10% 20%, rgba(74, 222, 128, 0.2), transparent 45%),
    radial-gradient(circle at 80% 0%, rgba(236, 72, 153, 0.25), transparent 35%),
    #050505;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  gap: clamp(2rem, 5vw, 3.5rem);
}

.aurora {
  position: absolute;
  width: 420px;
  height: 420px;
  filter: blur(60px);
  opacity: 0.6;
  pointer-events: none;
}

.aurora-1 {
  top: 10%;
  left: 5%;
  background: radial-gradient(circle, rgba(74, 222, 128, 0.4), transparent 70%);
}

.aurora-2 {
  bottom: 8%;
  right: 12%;
  background: radial-gradient(circle, rgba(37, 99, 235, 0.35), transparent 75%);
}

.hero-card,
.mission-card,
.member-card,
.ritual-card {
  position: relative;
  z-index: 1;
  border-radius: 1.75rem;
  padding: clamp(1.75rem, 4vw, 3rem);
  background: rgba(5, 5, 5, 0.85);
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 25px 60px rgba(0, 0, 0, 0.45);
}

.hero-card {
  border: 1px solid rgba(74, 222, 128, 0.4);
}

.eyebrow {
  font-family: 'Permanent Marker', cursive;
  letter-spacing: 0.4em;
  color: #4ade80;
  text-transform: uppercase;
  margin-bottom: 0.8rem;
}

.hero-card h1 {
  font-size: clamp(2.8rem, 6vw, 4.5rem);
  margin: 0;
  letter-spacing: 0.2em;
}

.hero-card .lead {
  margin-top: 1.2rem;
  line-height: 1.7;
  color: rgba(255, 255, 255, 0.85);
}

.tagline {
  margin-top: 1rem;
  color: rgba(255, 255, 255, 0.6);
  letter-spacing: 0.2em;
}

.mission-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
}

.mission-card {
  border: 1px solid rgba(74, 222, 128, 0.25);
}

.mission-card h3 {
  margin: 0;
  font-size: 1.6rem;
  letter-spacing: 0.1em;
}

.mission-card p {
  margin-top: 0.75rem;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.75);
}

.member-section header h2 {
  margin: 0.2rem 0 1rem;
  font-size: clamp(2rem, 4vw, 3rem);
}

.member-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
}

.member-card h3 {
  margin: 0.35rem 0 0.2rem;
  font-size: 1.8rem;
}

.member-role {
  text-transform: uppercase;
  font-size: 0.8rem;
  letter-spacing: 0.4em;
  color: rgba(255, 255, 255, 0.55);
}

.member-card p {
  margin: 0;
  color: rgba(255, 255, 255, 0.72);
  line-height: 1.6;
}

.ritual-card h3 {
  margin-bottom: 0.15rem;
  font-size: 2rem;
}

.ritual-card .lead {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
}

.ritual-card ul {
  margin: 1rem 0 0;
  padding: 0;
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
}

.ritual-card li {
  display: flex;
  align-items: center;
  gap: 0.65rem;
  color: rgba(255, 255, 255, 0.8);
  letter-spacing: 0.1em;
}

.bullet {
  width: 0.65rem;
  height: 0.65rem;
  border-radius: 999px;
  background: linear-gradient(135deg, #4ade80, #22d3ee);
}

@media (max-width: 640px) {
  .hero-card,
  .mission-card,
  .member-card,
  .ritual-card {
    border-radius: 1.25rem;
    padding: 1.25rem;
  }
}
</style>
