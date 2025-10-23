![[12_Nuxt.js.png]]
# 概述
---
## 简介
---
Nuxt.js 是一个基于 Vue.js 的高层框架，用来构建现代 Web 应用，特别适合做服务器渲染（SSR）、静态网站（SSG）、单页面应用（SPA）和全栈开发。
Vue.js 负责写前端组件，Nuxt.js 负责组织完整项目（路由、渲染、SEO、目录结构、性能优化、全栈能力）。
## 需求
---
在使用 Vue.js 写前端项目时，Vue.js 只解决了视图层的问题，其它问题需要自己解决，而 Nuxt,js 为 Vue.js 的开发解决了这些通用而繁琐的问题：
- **路由配置**
	- 传统：使用 vue-router 自己维护 routes，文件多了很乱。
	- Nuxt：基于 `pages/` 目录，自动生成 vue-router 路由。
- **页面 SEO**
	- 传统：纯前端 CSR，不利于搜索引擎收录，SEO 很差。
	- Nuxt：自动处理 meta 标签、head、OG 标签。
- **服务端渲染**
	- 传统：自己搭 SSR 方案复杂，门槛高。
	- Nuxt：配置即可选择渲染模式
- **页面结构规范**
	- 传统：自定义目录结构，没有统一规范。
	- Nuxt：页面共享布局管理。
- **数据获取**
	- 传统：自己在组件里写 fetch / axios，缺统一入口。
	- Nuxt：内置 useFetch / useAsyncData 无需自己封装 axios。
- **多模式支持（SSR、SSG、SPA）**
	- 传统：自己拆分逻辑，容易出错。
	- Nuxt：配置即可选择渲染模式
- **性能优化**
	- 传统：自己配 webpack、vite，配置繁琐。
	- Nuxt：内置 Vite/Webpack 最佳实践，无需手动配置。
除此之外，Nuxt.js 还提供了统一的插件管理，内置 Nitro，支持 API 接口，可以进行全栈开发。
## 对比
---
类似框架还有以 React 为基础的 Next.js，它们之间的对比如下：

| **维度**            | **Next.js**                               | **Nuxt.js**                                    |
| ----------------- | ----------------------------------------- | ---------------------------------------------- |
| **所属生态**          | React.js                                  | Vue.js                                         |
| **初始定位**          | SSR (服务端渲染)                               | SSR (服务端渲染)                                    |
| **支持模式**          | SSR、SSG、ISR、SPA、CSR                       | SSR、SSG、SPA、CSR                                |
| **文件路由**          | 自动，基于 `pages` 目录                          | 自动，基于 `pages` 目录                               |
| **API接口能力**       | API Routes 内置 API server                  | Nitro Server 支持 API routes                     |
| **插件生态**          | 强大，React 官方生态丰富                           | Vue 插件生态丰富，支持自定义模块                             |
| **状态管理**          | 自选（推荐 Zustand, Redux, SWR, React Query 等） | 推荐 Pinia、Vuex（老）                               |
| **全局布局**          | App Router / Layout 机制                    | layouts 目录                                     |
| **路由守卫**          | 自定义中间件                                    | middleware 目录                                  |
| **配置风格**          | config 文件 + App Router                    | nuxt.config.ts 一站式配置                           |
| **TypeScript 支持** | 一流                                        | 一流                                             |
| **自动导入**          | 部分（组件、hooks手动为主）                          | 几乎所有（组件、composables、api 自动导入）                  |
| **渲染机制**          | Vercel 驱动的 ISR (增量静态渲染) 特别强大              | Nuxt Island、Payload Extraction 技术提升 SSR/SSG 效率 |
| **社区&文档**         | 极其完善                                      | 也非常友好，但Next更大                                  |
| **开发体验**          | 稳定成熟、清晰规范                                 | 极度自动化、省心省力                                     |
| **官方推荐 Hosting**  | Vercel                                    | Vercel、Netlify、Cloudflare、Node server 都友好      |
> Next.js 像乐高——每一块都清晰规范，但需要你自己组合搭建。
> Nuxt.js 像积木城堡套件——大部分东西都自动帮你拼好了，只管玩。
# 开始
---
## 安装
---
先决条件：
- Node.js 版本大于 18.x，尽量使用偶数版本。
- IDE 使用 WebStorm 或者安装了 Vue 官方插件和 Nuxtr 插件的 VScode。

**新版命令**
1. 初始化 Nuxt 项目并安装依赖：
	```shell
	npm create nuxt <project-name>
	```
2. 启动项目：
	```shell
	npm run dev
	```
	
**旧版命令**
1. 初始化 Nuxt 项目：
	```shell
	npx nuxi init <project-name>
	```
2. 安装依赖：
	```shell
	npm i
	```
3. 启动项目：
	```shell
	npm run dev
	```
## 使用
---
Nuxt 项目目录：
```markdown
my-nuxt-app/
├── .nuxt/                # 自动生成的临时构建文件（不要手动改）
├── .output/              # build 后用于部署的产物（build 输出目录）
├── assets/               # 静态资源（SCSS、图片、字体等）不经过 hash 处理
├── components/           # Vue 组件（自动导入）
├── composables/          # 封装的 composable 函数（自动导入，类似 hooks）
├── content/              # Markdown 内容（需安装 @nuxt/content 才会有）
├── layouts/              # 布局组件（默认布局、错误页布局等）
├── middleware/           # 路由中间件（守卫、鉴权等逻辑）
├── pages/                # 页面视图（自动路由，核心目录）
├── plugins/              # 插件注入（注册第三方库、全局方法）
├── public/               # 静态资源（原样输出）url 直接引用
├── server/               # 服务端 API（Nuxt 全栈能力所在）
│   ├── api/              # API 路由（自动生成 RESTful 接口）
│   └── middleware/       # server 端中间件（处理请求/响应）
├── app.vue               # App 根组件（入口）
├── nuxt.config.ts        # Nuxt 配置文件（全局配置）
├── package.json          # 项目依赖
├── tsconfig.json         # TypeScript 配置
└── README.md             # 项目说明
```

Nuxt 制定了文件结构规范，特定内容应该放在特定的目录下



# 概念
---
## Views
---
### app.vue
---
![[12_Nuxt.js-1.png]]

默认情况下，Nuxt 把这个文件视为应用程序的入口点，并为每个路由呈现内容。
底层封装了 Vue.js 的 `main.js` 文件。
### Components
---
![[12_Nuxt.js-2.png]]

对于可重复使用的用户组件放在该目录下，可自动添加到应用程序，而无需导入。
### Pages
---
![[12_Nuxt.js-3.png]]

代表特定路由模式视图的页面放在该目录下，将自动根据页面生成路由。需要将 **`<NuxtPage />`** 组件添加到 **app.vue**。
### Layouts
---
![[12_Nuxt.js-4.png]]

对于多个页面通用的布局界面放在该目录下，布局使用 **`<slot />`** 组件显示页面内容。默认情况下，使用 **`layouts/default.vue`** 文件。如果只有一种布局，建议使用  **`app.vue`**  和  **`<NuxtPage />`** 代替。并且如果 layouts 目录下存在布局文件，则需要在 **`app.vue`** 中添加 **`<NuxtLayout />`**。可以将自定义布局设置为页面元数据的一部分。
```html
<template>
  <NuxtLayout>
	<NuxtPage />
  </NuxtLayout>
</template>	
```
### 扩展 HTML 模板
---
TODO


## Assets
---






# 配置
---




# 思考



# 附录
---
> [!website]
    > [Nuxt 官方网站](https://nuxt.com/)
    > [Nuxt 官方中文网站](https://nuxt.com.cn/)
    > [Nuxt 模块网站](https://nuxt.com/modules)

> [!code]
    > https://github.com/nuxt/nuxt

> [!video]
    > 

> [!doc]
    >-  [官方文档](https://nuxt.com/docs/)
    >- [官方中文文档](https://nuxt.com.cn/docs/getting-started/introduction) 

