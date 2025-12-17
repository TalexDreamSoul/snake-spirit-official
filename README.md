<p align='center'>
  <img src='https://user-images.githubusercontent.com/11247099/154486817-f86b8f20-5463-4122-b6e9-930622e757f2.png' alt='Vitesse - Opinionated Vite Starter Template' width='600'/>
</p>

<p align='center'>
Mocking up web app with <b>Vitesse</b><sup><em>(speed)</em></sup><br>
</p>

<br>

<p align='center'>
<a href="https://vitesse.netlify.app/">Live Demo</a>
</p>

<br>

> **Note**: This template is created during the early transition of Vue 3 and Vite. At this moment, if you are seeking for better Vue developer experience and more consistent maintenance, we recommend using [Nuxt](https://nuxt.com) instead (it also works perfectly with SPA or SSG as needed). This template still serves as a reference, but expect slower updates.

<br>

<p align='center'>
<b>English</b> | <a href="https://github.com/antfu-collective/vitesse/blob/main/README.zh-CN.md">简体中文</a>
<!-- Contributors: Thanks for getting interested, however we DON'T accept new translations to the README, thanks. -->
</p>

<br>

## Snake Spirit 现状速记

- 首页 ZIP 下载按钮读取 `VITE_ZIP_DOWNLOAD_URL`（默认指向 GitHub Releases 最新资产），仓库不再托管 500MB 压缩包，发版时把包上传到 Release Asset 即可。
- 新增 `/feedback` 页面，承接 GitHub Issue 与邮箱 `paxfen@163.com` 的反馈链路，视觉与首页保持同频。
- 顶部导航包含「反馈」入口，可直接抵达该页面的核心内容区。
- 「游戏详情」导航入口新增丝滑浮动高亮，兼顾沉浸感与易用性。
- 新增 `/details` 页面，集中展示玩法亮点、技术规格与版本里程碑，和首页视觉保持一体化。
- `/details` 实机截图区升级为竖向整屏轮播，一屏一图当背景，滚动/按钮都能丝滑切换。

### ZIP 下载包策略

1. 构建或获取 `Dual-mode Snake Game.zip`，保持文件名一致但不要将其加入 Git。
2. 上传到 GitHub Releases（推荐命名为 `Dual-mode Snake Game.zip`），或任意 CDN/对象存储并记录 URL。
3. 在部署环境里设置 `VITE_ZIP_DOWNLOAD_URL` 指向该 URL，未配置时页面会使用 Releases 最新下载链接。
4. 若本地调试需要把 ZIP 放进 `public/assets/download/`，仓库的 `.gitignore` 会保护它不被提交。

## Features

- ⚡️ [Vue 3](https://github.com/vuejs/core), [Vite](https://github.com/vitejs/vite), [pnpm](https://pnpm.io/), [esbuild](https://github.com/evanw/esbuild) - born with fastness

- 🗂 [File based routing](./src/pages)

- 📦 [Components auto importing](./src/components)

- 🍍 [State Management via Pinia](https://pinia.vuejs.org/)

- 📑 [Layout system](./src/layouts)

- 📲 [PWA](https://github.com/antfu/vite-plugin-pwa)

- 🎨 [UnoCSS](https://github.com/antfu/unocss) - the instant on-demand atomic CSS engine

- 😃 [Use icons from any icon sets with classes](https://github.com/antfu/unocss/tree/main/packages/preset-icons)

- 🌍 [I18n ready](./locales)

- 🔎 [Component Preview](https://github.com/johnsoncodehk/vite-plugin-vue-component-preview)

- 🗒 [Markdown Support](https://github.com/unplugin/unplugin-vue-markdown)

- 🔥 Use the [new `<script setup>` syntax](https://github.com/vuejs/rfcs/pull/227)

- 📥 [APIs auto importing](https://github.com/antfu/unplugin-auto-import) - use Composition API and others directly

- 🖨 Static-site generation (SSG) via [vite-ssg](https://github.com/antfu/vite-ssg)

- 🦔 Critical CSS via [beasties](https://github.com/danielroe/beasties)

- 🦾 TypeScript, of course

- ⚙️ Unit Testing with [Vitest](https://github.com/vitest-dev/vitest), E2E Testing with [Cypress](https://cypress.io/) on [GitHub Actions](https://github.com/features/actions)

- ☁️ Deploy on Netlify, zero-config

<br>

## Pre-packed

### UI Frameworks

- [UnoCSS](https://github.com/antfu/unocss) - The instant on-demand atomic CSS engine.

### Icons

- [Iconify](https://iconify.design) - use icons from any icon sets [🔍Icônes](https://icones.netlify.app/)
- [Pure CSS Icons via UnoCSS](https://github.com/antfu/unocss/tree/main/packages/preset-icons)

### Plugins

- [Vue Router](https://github.com/vuejs/router)
  - [`unplugin-vue-router`](https://github.com/posva/unplugin-vue-router) - file system based routing
  - [`vite-plugin-vue-layouts`](https://github.com/JohnCampionJr/vite-plugin-vue-layouts) - layouts for pages
- [Pinia](https://pinia.vuejs.org) - Intuitive, type safe, light and flexible Store for Vue using the composition api
- [`unplugin-vue-components`](https://github.com/antfu/unplugin-vue-components) - components auto import
- [`unplugin-auto-import`](https://github.com/antfu/unplugin-auto-import) - Directly use Vue Composition API and others without importing
- [`unplugin-vue-macros`](https://github.com/sxzz/unplugin-vue-macros) - Explore and extend more macros and syntax sugar to Vue.
- [`vite-plugin-pwa`](https://github.com/antfu/vite-plugin-pwa) - PWA
- [`unplugin-vue-markdown`](https://github.com/unplugin/unplugin-vue-markdown) - Markdown as components / components in Markdown
  - [`@shikijs/markdown-it`](https://github.com/shikijs/shiki) - [Shiki](https://github.com/shikijs/shiki) for syntax highlighting
- [Vue I18n](https://github.com/intlify/vue-i18n-next) - Internationalization
  - [`unplugin-vue-i18n`](https://github.com/intlify/bundle-tools/tree/main/packages/unplugin-vue-i18n) - unplugin for Vue I18n
- [VueUse](https://github.com/antfu/vueuse) - collection of useful composition APIs
- [`vite-ssg-sitemap`](https://github.com/jbaubree/vite-ssg-sitemap) - Sitemap generator
- [`@unhead/vue v2`](https://github.com/unjs/unhead) - manipulate document head reactively
- [`vite-plugin-vue-devtools`](https://github.com/vuejs/devtools-next) - Designed to enhance the Vue developer experience.

### Coding Style

- Use Composition API with [`<script setup>` SFC syntax](https://github.com/vuejs/rfcs/pull/227)
- [ESLint](https://eslint.org/) with [@antfu/eslint-config](https://github.com/antfu/eslint-config), single quotes, no semi.

### Dev tools

- [TypeScript](https://www.typescriptlang.org/)
- [Vitest](https://github.com/vitest-dev/vitest) - Unit testing powered by Vite
- [Cypress](https://cypress.io/) - E2E testing
- [pnpm](https://pnpm.js.org/) - fast, disk space efficient package manager
- [`vite-ssg`](https://github.com/antfu/vite-ssg) - Static-site generation
  - [beasties](https://github.com/danielroe/beasties) - Critical CSS
- [Netlify](https://www.netlify.com/) - zero-config deployment
- [VS Code Extensions](./.vscode/extensions.json)
  - [Vite](https://marketplace.visualstudio.com/items?itemName=antfu.vite) - Fire up Vite server automatically
  - [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) - Vue 3 `<script setup>` IDE support
  - [Iconify IntelliSense](https://marketplace.visualstudio.com/items?itemName=antfu.iconify) - Icon inline display and autocomplete
  - [i18n Ally](https://marketplace.visualstudio.com/items?itemName=lokalise.i18n-ally) - All in one i18n support
  - [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

## Variations

As this template is strongly opinionated, the following provides a curated list for community-maintained variations with different preferences and feature sets. Check them out as well. PR to add yours is also welcome!

###### Official

- [vitesse-lite](https://github.com/antfu/vitesse-lite) - Lightweight version of Vitesse
- [vitesse-nuxt3](https://github.com/antfu/vitesse-nuxt3) - Vitesse for Nuxt 3
- [vitesse-nuxt-bridge](https://github.com/antfu/vitesse-nuxt-bridge) - Vitesse for Nuxt 2 with Bridge
- [vitesse-webext](https://github.com/antfu/vitesse-webext) - WebExtension Vite starter template

###### Community

- [vitesse-ssr-template](https://github.com/frandiox/vitesse-ssr-template) by [@frandiox](https://github.com/frandiox) - Vitesse with SSR
- [vitailse](https://github.com/zynth17/vitailse) by [@zynth17](https://github.com/zynth17) - Like Vitesse but with TailwindCSS
- [vitesse-modernized-chrome-ext](https://github.com/xiaoluoboding/vitesse-modernized-chrome-ext) by [@xiaoluoboding](https://github.com/xiaoluoboding) - ⚡️ Modernized Chrome Extension Manifest V3 Vite Starter Template
- [vitesse-stackter-clean-architect](https://github.com/shamscorner/vitesse-stackter-clean-architect) by [@shamscorner](https://github.com/shamscorner) - A modular clean architecture pattern in vitesse template
- [vitesse-enterprise](https://github.com/FranciscoKloganB/vitesse-enterprise) by [@FranciscoKloganB](https://github.com/FranciscoKloganB) - Consistent coding styles regardless of team-size.
- [vitecamp](https://github.com/nekobc1998923/vitecamp) by [@nekobc1998923](https://github.com/nekobc1998923) - Like Vitesse but without SSG/SSR/File based routing, includes Element Plus
- [vitesse-h5](https://github.com/YunYouJun/vitesse-h5) by [@YunYouJun](https://github.com/YunYouJun) - Vitesse for Mobile
- [bat](https://github.com/olgam4/bat) by [@olgam4](https://github.com/olgam4) - Vitesse for SolidJS
- [vitesse-solid](https://github.com/xbmlz/vitesse-solid) by [@xbmlz](https://github.com/xbmlz) - Vitesse for SolidJS, build with [`SolidStart`](https://start.solidjs.com/), includes [UnoCSS](https://github.com/unocss/unocss).
- [vue3-vant-mobile](https://github.com/easy-temps/vue3-vant-mobile) by [CharleeWa](https://github.com/CharleeWa) - Like Vitesse but without SSG/SSR, includes Vant

## Try it now!

> Vitesse requires Node >=14.18

### GitHub Template

[Create a repo from this template on GitHub](https://github.com/antfu-collective/vitesse/generate).

### Clone to local

If you prefer to do it manually with the cleaner git history

```bash
npx degit antfu-collective/vitesse my-vitesse-app
cd my-vitesse-app
pnpm i # If you don't have pnpm installed, run: npm install -g pnpm
```

## Checklist

When you use this template, try follow the checklist to update your info properly

- [ ] Change the author name in `LICENSE`
- [ ] Change the title in `App.vue`
- [ ] Change the hostname in `vite.config.ts`
- [ ] Change the favicon in `public`
- [ ] Remove the `.github` folder which contains the funding info
- [ ] Clean up the READMEs and remove routes

And, enjoy :)

## Usage

### Development

Just run and visit http://localhost:3333

```bash
pnpm dev
```

### Build

To build the App, run

```bash
pnpm build
```

And you will see the generated file in `dist` that ready to be served.

### Deploy on Netlify

Go to [Netlify](https://app.netlify.com/start) and select your clone, `OK` along the way, and your App will be live in a minute.

### Docker Production Build

First, build the vitesse image by opening the terminal in the project's root directory.

```bash
docker buildx build . -t vitesse:latest
```

Run the image and specify port mapping with the `-p` flag.

```bash
docker run --rm -it -p 8080:80 vitesse:latest
```

## Why

I have created several Vite apps recently. Setting the configs up is kinda the bottleneck for me to make the ideas simply come true within a very short time.

So I made this starter template for myself to create apps more easily, along with some good practices that I have learned from making those apps. It's strongly opinionated, but feel free to tweak it or even maintain your own forks. [(see community maintained variation forks)](#variations)
