<script setup lang="ts">
const router = useRouter()
const route = useRoute()

const navLinks = [
  { label: '下载渠道', to: { path: '/', hash: '#download-area' } },
  { label: '游戏详情', to: { path: '/details', hash: '#details' } },
  { label: '提交反馈', to: { path: '/feedback', hash: '#feedback' } },
  { label: '特别感谢', to: '/special-thanks', hash: '#special-thanks' },
]

const isHome = computed(() => route.path === '/')

function goHome() {
  router.push('/')
}

function handleNavButtonClick() {
  if (isHome.value)
    return
  goHome()
}

function isActive(link: { to: string | { path: string } }) {
  if (typeof link.to === 'string')
    return route.path === link.to
  return route.path === link.to.path
}
</script>

<template>
  <div class="nav-layout" :class="{ 'absolute w-full': !isHome }">
    <nav class="site-nav">
      <div class="brand cursor-pointer" @click="router.push('/')">
        <span class="brand-icon material-symbols-outlined" aria-hidden="true">gesture</span>
        <span class="brand-text">SNAKE<span>SPIRIT</span></span>
      </div>

      <div class="nav-links">
        <RouterLink
          v-for="link in navLinks"
          :key="link.label"
          :to="link.to"
          :class="{ active: isActive(link) }"
          :data-floating="link.label === '游戏详情' ? 'true' : null"
        >
          {{ link.label }}
        </RouterLink>
      </div>

      <button
        class="nav-button"
        type="button"
        :disabled="isHome"
        @click="handleNavButtonClick"
      >
        {{ isHome ? "登录测试服" : "返回主界面" }}
      </button>
    </nav>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Permanent+Marker&family=Rajdhani:wght@500;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,1,0');

.nav-layout {
  background: transparent;
  color: #fff;
  font-family: 'Rajdhani', sans-serif;
  display: flex;
  flex-direction: column;
}

.site-nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.25rem clamp(1.5rem, 5vw, 3rem);
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
  position: relative;
  z-index: 5;
  backdrop-filter: blur(4px);
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
  gap: 2rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: rgba(255, 255, 255, 0.6);
}

.nav-links a {
  transition: color 0.3s ease;
}

.nav-links a[data-floating='true'] {
  position: relative;
  padding: 0.2rem 0.25rem;
  color: #f8fafc;
  text-shadow: 0 3px 10px rgba(26, 255, 167, 0.35);
  isolation: isolate;
}

.nav-links a[data-floating='true']::before,
.nav-links a[data-floating='true']::after {
  content: '';
  position: absolute;
  left: 50%;
  top: 50%;
  width: 120%;
  height: 160%;
  transform: translate(-50%, -50%);
  border-radius: 999px;
  pointer-events: none;
  opacity: 0.9;
  z-index: -1;
  background: radial-gradient(circle at 30% 30%, rgba(74, 222, 128, 0.35), transparent 65%);
  animation: floatGlow 5s ease-in-out infinite;
}

.nav-links a[data-floating='true']::after {
  top: 50%;
  width: 80%;
  height: 0.22rem;
  background: linear-gradient(90deg, transparent, rgba(74, 222, 128, 0.6), transparent);
  filter: blur(0.5px);
  animation: floatUnderline 6s ease-in-out infinite;
}

.nav-links .active {
  color: #fff;
}

.nav-button {
  padding: 0.45rem 1.5rem;
  border: 1px solid rgba(34, 197, 94, 0.5);
  border-radius: 999px;
  background: transparent;
  color: #4ade80;
  font-weight: 700;
  letter-spacing: 0.1em;
  transition: all 0.3s ease;
}

.nav-button:hover:not(:disabled) {
  background: #22c55e;
  color: #050505;
}

.nav-button:disabled {
  opacity: 0.6;
  border-color: rgba(34, 197, 94, 0.3);
  cursor: default;
}

.nav-body {
  flex: 1;
  min-height: 0;
}

.material-symbols-outlined {
  font-variation-settings:
    'FILL' 0,
    'wght' 400,
    'GRAD' 0,
    'opsz' 24;
}

@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
    opacity: 1;
  }

  50% {
    transform: scale(1.15);
    opacity: 0.6;
  }
}

@keyframes floatGlow {
  0%,
  100% {
    transform: translate(-50%, -48%) scale(0.95);
    opacity: 0.6;
  }

  45% {
    transform: translate(-50%, -52%) scale(1.05);
    opacity: 1;
  }
}

@keyframes floatUnderline {
  0%,
  100% {
    transform: translate(-50%, 0) scaleX(0.7);
    opacity: 0.4;
  }

  50% {
    transform: translate(-50%, -0.25rem) scaleX(1.1);
    opacity: 1;
  }
}

@media (min-width: 960px) {
  .nav-links {
    display: flex;
  }
}

@media (max-width: 640px) {
  .nav-button {
    display: none;
  }

  .nav-links {
    display: none;
  }
}
</style>
