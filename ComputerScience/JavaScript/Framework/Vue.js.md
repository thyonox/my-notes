# 核心概念
## 响应式系统




#  概述
## 简介
Vue（Vue.js）是一个开源的 JavaScript 框架，用于构建用户界面和单页应用（SPA）。它由 Evan You 于2014年创建，以简单、灵活和高性能著称。Vue 采用组件化开发，结合响应式数据绑定和虚拟 DOM 技术，使开发者能够高效构建交互式 Web 应用。其核心特点包括：
- **轻量级**：核心库体积小，加载快，适合中小型项目。
- **响应式数据绑定**：通过数据驱动视图，简化状态管理。
- **组件化**：支持模块化开发，便于代码复用和维护。
- **易上手**：学习曲线平缓，文档清晰，适合初学者和专业开发者。
- **生态丰富**：配合Vue Router、Vuex（或Pinia）等工具，支持复杂应用开发。
## 需求
1. **繁琐的 DOM 操作**：
    - **问题**：传统 JavaScript 或 jQuery 开发中，开发者需要频繁操作 DOM 来更新界面，代码容易变得冗长且难以调试。
    - **Vue 的解决方案**：通过响应式数据绑定，当数据变化时，Vue 自动更新视图，开发者只需关注数据状态，无需直接操作 DOM。
2. **代码组织和维护难度**：
    - **问题**：在大型项目中，HTML、CSS 和 JavaScript 代码混合在一起，难以模块化和维护。
    - **Vue 的解决方案**：通过组件化开发，Vue 将界面逻辑、样式和行为分成独立的可复用组件，代码结构清晰，便于维护和扩展。
3. **性能问题**：
    - **问题**：频繁的 DOM 操作会导致性能瓶颈，尤其是在动态更新的复杂界面中。
    - **Vue 的解决方案**：采用虚拟 DOM 技术，仅对必要的部分进行更新，显著提高渲染性能。
4. **状态管理复杂性**：
    - **问题**：在复杂应用中，管理多个组件之间的状态同步和通信非常麻烦。
    - **Vue 的解决方案**：结合 Vuex 或 Pinia 提供集中式状态管理，简化组件间的数据共享和状态更新。
5. **学习曲线和团队协作**：
    - **问题**：许多框架（如早期的 Angular）学习成本高，团队协作需要更高的技术门槛。
    - **Vue 的解决方案**：提供简洁的 API 和清晰的文档，降低学习难度，适合不同经验水平的开发者协作开发。
6. **跨框架兼容性**：
    - **问题**：不同框架之间的集成可能存在兼容性问题，限制了技术选型的灵活性。
    - **Vue 的解决方案**：Vue 设计轻量且灵活，可逐步引入现有项目，支持与其他库（如 jQuery 或 React 组件）混合使用。

## 对比

| 特性         | Vue.js               | React                             | Angular                  |
| ---------- | -------------------- | --------------------------------- | ------------------------ |
| **类型**     | 渐进式框架                | 库（UI 组件库）                         | 完整框架                     |
| **发布年份**   | 2014                 | 2013                              | 2016 (Angular 2+)        |
| **核心理念**   | 简单易用、灵活、渐进式          | 组件化、单向数据流、虚拟 DOM                  | 完整 MVC 框架、依赖注入           |
| **学习曲线**   | 平缓，易上手               | 中等，需掌握 JSX 和生态工具                  | 较陡峭，需学习 TypeScript 和框架概念 |
| **数据绑定**   | 双向绑定（v-model）+ 响应式数据 | 单向数据流                             | 双向绑定（ngModel）            |
| **虚拟 DOM** | 支持                   | 支持                                | 不完全依赖，使用变更检测机制           |
| **组件化**    | 支持，单文件组件（.vue）       | 支持，JSX 组件                         | 支持，基于 TypeScript 的组件     |
| **状态管理**   | Vuex / Pinia         | Redux / MobX / Context API        | NgRx / 服务                |
| **路由**     | Vue Router           | React Router                      | Angular Router           |
| **构建工具**   | Vue CLI / Vite       | Create React App / Vite / Webpack | Angular CLI              |
| **文件大小**   | ~20KB（核心库）           | ~40KB（React + ReactDOM）           | ~100KB+（打包后较大）           |
| **生态系统**   | 丰富，灵活                | 非常丰富，社区驱动                         | 完整，官方支持强                 |

# 开始
## 安装
1. **通过 CDN 引入**
	- 方式：在 HTML 文件中通过 `<script>` 标签直接引入 Vue 的 CDN 链接。
	- 适用场景：
		- 快速原型开发或学习 Vue。
		- 小型项目或静态页面。
		- 无需构建工具，适合直接嵌入 HTML。
	- 注意：
		- 可选择 `vue.global.js`（完整版，包含编译器）或 `vue.runtime.global.js`（运行时版本，体积更小，但需预编译模板）。
		- 生产环境建议指定版本号（如 `vue@3.4.21`）以避免不兼容更新。
	- 代码示例
		```html
		<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>		
		```
2. **通过 npm/yarm 安装**
	- 方式：使用 Node.js 的包管理工具安装 Vue，适合现代前端开发。
	- 步骤：
		1. 确保安装 Node.js（附带 npm）。
		2. 在项目目录运行：
			```bash
			npm install vue
			或
			yarn add vue
			```
		3. 在 JavaScript 文件中引入：
			```js
			import { createApp } from 'vue';
			import App from './App.vue';
			
			const app = createApp(App);
			app.mount('#app');
			```
	- 适用场景：
		- 单页应用（SPA）或复杂项目。
		- 使用构建工具（如 Vite、Webpack）或 Vue CLI。
		- 需要模块化开发和组件化。
3. **通过 Vue CLI 快速搭建**
	- 方式：使用 Vue CLI 快速生成项目脚手架。
	- 步骤：
		1. 全局安装 Vue CLI：
			```bash
			npm install -g @vue/cli
			```
		2. 创建项目：
			```bash
			vue create my-project
			```
		3. 按提示选择配置（如 Vue 3、TypeScript、Babel 等）。
		4. 启动开发服务器：
			```bash
			cd my-project
			npm run serve
			```
	- 适用场景：
	    - 需要快速搭建标准化的 Vue 项目。
	    - 适合中大型项目，包含路由、状态管理和测试工具。
	- 注意：Vue CLI 现已被 Vite 取代，推荐新项目使用 Vite。
4. **通过 Vite 快速搭建**
	- 方式：使用 Vite 创建 Vue 项目，构建速度快，配置简单。
	- 步骤：
		1. 运行命令创建项目：
			```bash
			npm create vite@latest my-vue-app -- --template vue
			```
		2. 进入项目目录并安装依赖：
			```bash
			cd my-vue-app
			npm install
			```
		3. 启动开发服务器：
			```bash
			npm run dev
			```
	- **适用场景**：
	    - 现代前端开发，追求快速构建和热更新。
	    - 适合新手和专业开发者，配置简单。
	- **优点**：比 Vue CLI 更快，支持 TypeScript 和现代 ES 模块。
	- 
5. **直接下载源码**
	- 方式：从 Vue 官网或 GitHub 下载源码，手动引入项目。
	- 步骤：
		1. 下载 Vue 的 JavaScript 文件（https://vuejs.org/ 或 GitHub 仓库）。
		2. 放入项目目录，HTML 中通过 `<script>` 引入：
			```html
			<script src="./path/to/vue.js"></script>
			```
	- 适用场景：
	    - 离线开发或无法使用 CDN 的环境。
	    - 小型项目或测试场景。
	- 注意：手动管理版本更新，维护成本较高。
6. **通过 ESM CDN（如 esm.sh）**
	- 方式：使用支持 ES 模块的 CDN，适合现代浏览器。
	- 代码示例：
		```html
		<script type="module">
		  import { createApp } from 'https://esm.sh/vue@3';
		  createApp({ /* 组件配置 */ }).mount('#app');
		</script>
		```
	-  适用场景：
	    - 现代浏览器开发，无需构建工具。
	    - 轻量级项目，支持模块化导入。
	- 注意：仅支持支持 ES 模块的浏览器（如 Chrome、Edge）。
7. **在现有项目中集成**
	- 方式：将 Vue 逐步引入现有项目（如 jQuery 项目）。
	- 步骤：
		1. 通过 CDN 或 npm 引入 Vue。
		2. 在指定 DOM 元素上挂载 Vue 应用：
			```html
			<div id="app">{{ message }}</div>
			<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
			<script>
			  const { createApp } = Vue;
			  createApp({
			    data() {
			      return { message: 'Hello Vue!' };
			    },
			  }).mount('#app');
			</script>
			```
	- 适用场景：
		- 逐步迁移旧项目到 Vue。
		- 在现有页面中添加动态功能。
## 使用
```text
my-vue-app/
├── node_modules/             # 依赖包目录
├── public/                   # 静态资源目录
│   ├── favicon.ico           # 网站图标
│   └── other-static-files    # 其他静态资源（如图片、字体）
├── src/                      # 源代码目录
│   ├── assets/               # 静态资源（如图片、CSS、字体）
│   ├── components/           # Vue 组件目录
│   │   ├── MyComponent.vue   # 自定义组件
│   │   └── ...               # 其他组件
│   ├── views/pages/          # 页面级组件（通常与路由对应）
│   │   ├── Home.vue          # 主页组件
│   │   └── About.vue         # 关于页面组件
│   ├── App.vue               # 根组件
│   ├── main.js               # 入口文件（初始化 Vue 应用）
│   ├── router/               # 路由配置文件
│   │   └── index.js          # 定义路由规则
│   ├── store/                # 状态管理（如 Pinia 或 Vuex）
│   │   └── index.js          # 状态管理配置文件
│   └── styles/               # 全局样式目录
│       └── main.css          # 全局 CSS 文件
├── .gitignore                # Git 忽略文件
├── index.html                # 项目入口 HTML 文件
├── package.json              # 项目依赖和脚本配置
├── vite.config.js            # Vite 配置文件
└── README.md                 # 项目说明文档
```
1. **node_modules/**
	- **作用**：存储项目依赖的第三方包，由 npm 或 yarn 自动生成。
	- **说明**：包括 Vue 核心库、插
	- 件、工具等。无需手动修改，通常在 .gitignore 中忽略。
2. **public/**
	- **作用**：存放静态资源，这些文件在构建时直接复制到输出目录（通常是 dist）。
	- **常见文件**：
	    - favicon.ico：网站图标。
	    - 其他静态资源（如图片、JSON 文件），可通过 / 路径直接访问（例如 /favicon.ico）。
	- **适用场景**：适合不需通过构建工具处理的文件，如 favicon 或第三方脚本。
3. **src/**
	- **作用**：项目核心代码目录，包含所有源代码。
	- **子目录和文件**：
	    - **assets/**：
	        - 存放图片、CSS、字体等静态资源。
	        - 这些资源通常通过导入（如 import img from '@/assets/image.png'）在组件中使用，构建时会被优化（如压缩图片）。
	    - **components/**：
	        - 存放可复用的 Vue 组件（如按钮、导航栏）。
	        - 组件文件以 .vue 结尾，包含模板、脚本和样式（单文件组件）。
	        - 示例：Button.vue、Header.vue。
	    - **views/**：
	        - 存放页面级组件，通常与路由对应。
	        - 示例：Home.vue（首页）、About.vue（关于页面）。
	    - **App.vue**：
	        - 项目根组件，包含整个应用的顶层结构。
	        - 通常包含 `<router-view>`（用于渲染路由对应的页面组件）。
	        - 示例结构：
				```html
				<template>
				  <div id="app">
				    <router-view />
				  </div>
				</template>
				```
		- **main.js**：
			- 项目入口文件，初始化 Vue 应用。
			- 负责创建应用实例、挂载到 DOM、配置路由和状态管理。
			- 示例：
				```js
				import { createApp } from 'vue';
				import App from './App.vue';
				import router from './router';
				import { createPinia } from 'pinia';
				
				const app = createApp(App);
				app.use(router);
				app.use(createPinia());
				app.mount('#app');
				```
		- **router/**：
			- 存放路由配置文件，通常命名为 index.js。
			- 使用 Vue Router 定义路由规则，映射路径到组件。
			- 示例：
				```js
				import { createRouter, createWebHistory } from 'vue-router';
				import Home from '../views/Home.vue';
				
				const routes = [
				  { path: '/', component: Home },
				  { path: '/about', component: () => import('../views/About.vue') },
				];
				
				const router = createRouter({
				  history: createWebHistory(),
				  routes,
				});
				
				export default router;
				```
		- **store/**：
			- 存放状态管理配置文件，通常使用 Pinia（Vue 3 推荐）或 Vuex（Vue 2 常用）。
			- 示例（Pinia）：
				```js
				import { defineStore } from 'pinia';
				
				export const useStore = defineStore('main', {
				  state: () => ({ count: 0 }),
				  actions: { increment() { this.count++; } },
				});
				```
		- **styles/**：
			- 存放全局 CSS 或 SCSS 文件。
			- 示例：main.css 可定义全局样式，导入到 main.js 或 App.vue。
4. **index.html**
	- **作用**：项目的入口 HTML 文件，Vue 应用挂载的容器。
	- **说明**：
	    - 包含 `<div id="app"></div>`，Vue 应用通过 `app.mount('#app')` 挂载到此元素。
	    - 可添加 meta 标签、标题、外部脚本等。
	    - 示例：
			```html
			<!DOCTYPE html>
			<html lang="en">
			  <head>
			    <meta charset="UTF-8">
			    <title>My Vue App</title>
			  </head>
			  <body>
			    <div id="app"></div>
			    <script type="module" src="/src/main.js"></script>
			  </body>
			</html>
			```
5. **package.json**
	- **作用**：定义项目依赖、脚本和元信息。
	- **内容**：
	    - 依赖：vue、vue-router、pinia 等。
	    - 脚本：如 npm run dev（启动开发服务器）、npm run build（构建生产包）。
	    - 示例：
			```json
			{
			  "name": "my-vue-app",
			  "dependencies": {
			    "vue": "^3.4.21",
			    "vue-router": "^4.2.0"
			  },
			  "scripts": {
			    "dev": "vite",
			    "build": "vite build",
			    "preview": "vite preview"
			  }
			}
			```
6. **vite.config.js**
	- **作用**：Vite 的配置文件，用于自定义构建行为。
	- **示例**：
		```js
		import { defineConfig } from 'vite';
		import vue from '@vitejs/plugin-vue';
		
		export default defineConfig({
		  plugins: [vue()],
		  resolve: {
		    alias: {
		      '@': '/src', // 设置 @ 指向 src 目录
		    },
		  },
		});
		```
	- 说明：可配置插件、路径别名、代理等。
7. **.gitignore**
	- **作用**：指定 Git 忽略的文件（如 node_modules、dist）。
	- **常见忽略**：node_modules/、dist/、.env。
8. **README.md**
	- **作用**：项目说明文档，描述项目功能、安装步骤和运行方法。
# 概念
## 核心概念
### 响应式系统
#### 核心响应式API
- `reactive`
	- 用于创建响应式对象，适用于对象和数组。
	- 使用 `Proxy` 拦截对象的属性操作（如读取、设置、删除等），实现响应式更新。
	- 注意：
		- `reactive` 创建的对象是深层响应式的，所有嵌套的属性都会被代理。
	- 示例：
		```js
		import { reactive } from 'vue';
		const state = reactive({ count: 0 });
		state.count++; // 触发响应式更新
		```
- `ref`
	- 用于创建基本数据类型（如字符串、数字、布尔值）的响应式引用。
	- 返回一个包含 `.value` 属性的对象，`.value` 用于访问和修改值。
	- 注意：
		- 在模板中，`ref` 的 `.value` 会被自动解包。
		- 如果对象或数组的属性不需要响应式（只是对象或数组本身需要响应式），也可以使用该API。
	- 示例：
		```js
		import { ref } from 'vue';
		const count = ref(0);
		count.value++; // 修改值，触发更新
		```
- `roRefs` 和 `toRef`
	- `toRefs`：将 `reactive` 对象的每个属性转换为独立的 `ref`，便于解构使用。
	- `toRef`：从 `reactive` 对象中提取单个属性的 `ref`，保持响应式连接。
	- 示例：
		```js
		import { reactive, toRefs } from 'vue';
		const state = reactive({ count: 0, name: 'Vue' });
		const { count, name } = toRefs(state); // count 和 name 是独立的 ref
		count.value++; // 修改 count，state.count 也会同步更新
		```
- `readonly`
	- 创建只读的响应式对象，防止数据被修改。
	- 常用于防止意外更改或向子组件传递只读数据。
	- 示例：
		```js
		import { reactive, readonly } from 'vue';
		const state = reactive({ count: 0 });
		const readOnlyState = readonly(state);
		readOnlyState.count = 1; // 抛出警告，无法修改
		```
- `shallowReactive` 和 `shallowRef`
	- 提供浅层响应式，仅对对象的第一层属性或 ref 的 .value 进行响应式追踪。
	- 适用于性能敏感场景，减少深层嵌套对象的代理开销。
	- 示例：
		```js
		import { shallowReactive } from 'vue';
		const state = shallowReactive({ obj: { count: 0 } });
		state.obj.count = 1; // 不触发响应式更新
		state.obj = { count: 2 }; // 触发响应式更新
		```
#### 响应式原理
- **Proxy 与 Reflect**
	- Vue3 使用 `Proxy` 拦截对象的 `getter` 和 `setter` 操作，动态追踪依赖并触发更新。
	- `Reflect` 用于规范化对象操作，确保属性访问和修改行为一致。
	- 相比 Vue2 的 `Object.defineProperty`，`Proxy` 支持数组操作、动态添加属性等，且性能更优。
- **依赖追踪**
	- Vue3 通过 `effect`（副作用函数）收集依赖，当响应式数据变化时，触发相关 `effect` 执行。
	- 依赖收集发生在数据访问时，自动绑定到当前组件的渲染函数或 `watch` 回调。
### Composition API
#### setup函数
- `setup` 是 Composition API 的入口，定义在组件中，用于组织响应式数据、计算属性、方法和生命周期钩子。
- 语法：
	- 选项式：`setup(props, context)`，接收 `props` 和上下文对象（包含 `attrs`、`slots`、`emit` 等）。
	- 语法糖：`<script setup>`，简化代码编写，自动暴露顶层变量。
- 示例：
	- 选项式：
		```js
		import { ref } from 'vue';
		export default {
		  setup(props, { emit }) {
		    const count = ref(0);
		    const increment = () => {
		      count.value++;
		      emit('update', count.value);
		    };
		    return { count, increment };
		  },
		};
		```
	- 语法糖：
		```js
		<script setup>
		import { ref } from 'vue';
		const count = ref(0);
		const increment = () => count.value++;
		</script>
		```
#### 响应式API
- `computed`
	- 创建计算属性，基于原始响应式数据的衍生属性，自动追踪依赖并缓存结果。
	- 支持 getter 和 setter。
	- 示例：
		```js
		import { ref, computed } from 'vue';
		const count = ref(0);
		const double = computed(() => count.value * 2); // 仅 getter
		const editableDouble = computed({
		  get: () => count.value * 2,
		  set: (val) => (count.value = val / 2),
		});
		```
- `watch`
	- 监听响应式数据的变化，执行回调函数。
	- 支持监听单个 `ref`、多个来源或深层对象。
	- 示例：
		```js
		import { ref, watch } from 'vue';
		const count = ref(0);
		watch(count, (newValue, oldValue) => {
		  console.log(`count changed from ${oldValue} to ${newValue}`);
		}, { deep: true });
		```
- `watchEffect`
	- 自动追踪依赖并立即执行，适合不需要明确指定监听目标的场景。
	- 示例：
		```js
		import { ref, watchEffect } from 'vue';
		const count = ref(0);
		watchEffect(() => {
		  console.log(`count is ${count.value}`);
		});
		```
#### 生命周期钩子
- Composition API 提供了与 Options API 等价的生命周期钩子，使用 `on` 前缀。
- 主要钩子：
	- `onBeforeMount`：组件挂载前
	- `onMounted`：组件挂载完成
	- `onBeforeUpdate`：组件更新前
	- `onUpdated`：组件更新完成
	- `onBeforeUnmount`：组件卸载前
	- `onUnmounted`：组件卸载完成
- 示例：
	```js
	import { onMounted, onUnmounted } from 'vue';
	export default {
	  setup() {
	    onMounted(() => {
	      console.log('Component mounted');
	    });
	    onUnmounted(() => {
	      console.log('Component unmounted');
	    });
	  },
	};
	```
#### provide 和 inject
- 用于跨组件层级传递数据，类似 Vue2 的 `provide/inject`。
- 支持响应式数据传递。
- 示例：
	```js
	import { provide, inject, ref } from 'vue';
	// 父组件
	export default {
	  setup() {
	    const theme = ref('light');
	    provide('theme', theme);
	  },
	};
	// 子组件
	export default {
	  setup() {
	    const theme = inject('theme'); // 获取 theme
	    return { theme };
	  },
	};
	```
### Options API
#### 核心选项
- `data`
	- 定义组件的响应式数据，返回一个对象。
	- 示例：
		```js
		export default {
		  data() {
		    return { count: 0 };
		  },
		};
		```
- `methods`
	- 定义组件的方法，供模板或逻辑调用。
	- 示例：
		```js
		export default {
		  methods: {
		    increment() {
		      this.count++;
		    },
		  },
		};
		```
- `computed`
	- 定义计算属性，自动缓存依赖结果。
	- 示例：
		```js
		export default {
		  computed: {
		    double() {
		      return this.count * 2;
		    },
		  },
		};
		```
- `watch`
	- 监听数据变化，执行回调。
	- 示例：
		```js
		  export default {
		  watch: {
		    count(newValue, oldValue) {
		      console.log(`count changed to ${newValue}`);
		    },
		  },
		};
		```
#### 生命周期方法
- 与 Composition API 的生命周期钩子对应：
	- `beforeCreate`、`created`
	- `beforeMount`、`mounted`
	- `beforeUpdate`、`updated`
	- `beforeUnmount`、`unmounted`
- 示例：
	```js
	export default {
	  mounted() {
	    console.log('Component mounted');
	  },
	};
	```
## 组件开发
### 组件基础
#### 组件注册
- **全局注册**
	- 使用 `app.component` 注册组件，适用于整个应用。
	- 注意：全局注册可能增加打包体积，建议谨慎使用。
	- 示例：
		```js
		import { createApp } from 'vue';
		import MyComponent from './components/MyComponent.vue';
		const app = createApp({});
		app.component('MyComponent', MyComponent); // 全局注册
		```
- **局部注册**
	- 在组件的 `components` 选项中注册，仅在当前组件可用。
	- 推荐：局部注册支持 Tree-shaking，优化打包效率。
	- 示例：
		```js
		import MyComponent from './MyComponent.vue';
		export default {
		  components: { MyComponent }, // 局部注册
		};
		```
- **`<script setup>` 语法糖**
	- 在 `<script setup>` 中直接导入组件，无需显式注册。
	- 示例：
		```js
		<script setup>
		import MyComponent from './MyComponent.vue';
		</script>
		<template>
		  <MyComponent />
		</template>
		```
#### 组件定义
- 组件通常包含模板（`<template>`）、逻辑（`<script>`）和样式（`<style>`）。
- **scoped 样式**：通过 `scoped` 属性限制 CSS 作用于当前组件，避免样式冲突。
- 示例：
	```js
	<template>
	  <div>{{ message }}</div>
	</template>
	<script setup>
	import { ref } from 'vue';
	const message = ref('Hello, Vue3!');
	</script>
	<style scoped>
	div { color: blue; }
	</style>
	```
#### 动态组件
- 使用 `<component :is>` 动态切换组件，适合选项卡或条件渲染场景。
- **注意**：动态组件切换可能导致组件销毁，需配合 `<keep-alive>` 缓存状态（见下文）。
- 示例：
	```js
	<script setup>
	import { ref } from 'vue';
	import ComponentA from './ComponentA.vue';
	import ComponentB from './ComponentB.vue';
	const currentComponent = ref('ComponentA');
	</script>
	<template>
	  <component :is="currentComponent" />
	  <button @click="currentComponent = 'ComponentB'">Switch to B</button>
	</template>
	```
#### 异步组件
- 使用 defineAsyncComponent 实现组件懒加载，优化首屏加载性能。
- 示例：
	```js
	import { defineAsyncComponent } from 'vue';
	const AsyncComponent = defineAsyncComponent(() =>
	  import('./components/AsyncComponent.vue')
	);
	export default {
	  components: { AsyncComponent },
	};
	```
- **高级用法**：
	- 支持加载中（`loadingComponent`）和错误（`errorComponent`）状态。
	- 示例：
		```js
		const AsyncComponent = defineAsyncComponent({
		  loader: () => import('./AsyncComponent.vue'),
		  loadingComponent: LoadingComponent,
		  errorComponent: ErrorComponent,
		  delay: 200, // 延迟显示加载组件
		  timeout: 3000, // 超时时间
		});
		```
### 插槽
#### 默认插槽
- 子组件使用 `<slot>` 定义插槽，父组件提供内容。
- **默认内容**：当父组件未提供内容时，显示子组件的默认内容。
- 示例：
	```js
	<!-- 子组件：Child.vue -->
	<template>
	  <div>
	    <slot>默认内容</slot>
	  </div>
	</template>
	<!-- 父组件 -->
	<template>
	  <Child>自定义内容</Child>
	</template>
	```
#### 具名插槽
- 使用 `name` 属性定义多个插槽，父组件通过 `v-slot` 指定插槽。
- **简写**：`v-slot:name` 可简写为 `#name`（如 `#header`）。
- 示例：
	```js
	<!-- 子组件：Child.vue -->
	<template>
	  <header><slot name="header"></slot></header>
	  <main><slot name="default"></slot></main>
	  <footer><slot name="footer"></slot></footer>
	</template>
	<!-- 父组件 -->
	<template>
	  <Child>
	    <template v-slot:header>Header Content</template>
	    <template v-slot:default>Main Content</template>
	    <template v-slot:footer>Footer Content</template>
	  </Child>
	</template>
	```
#### 作用域插槽
- 子组件通过插槽向父组件传递数据，父组件动态渲染内容。
- **用途**：常用于列表渲染、表格等需要父组件控制子组件内容显示的场景。
- 示例：
	```js
	<!-- 子组件：Child.vue -->
	<template>
	  <slot :item="item" :index="index">{{ item.name }}</slot>
	</template>
	<script setup>
	import { ref } from 'vue';
	const item = ref({ name: 'Vue' });
	const index = ref(0);
	</script>
	<!-- 父组件 -->
	<template>
	  <Child v-slot="{ item, index }">
	    <div>{{ index }}: {{ item.name }}</div>
	  </Child>
	</template>
	```
#### 动态插槽
- 使用动态插槽名称实现灵活的内容分发。
- 示例：
	```js
	<script setup>
	import { ref } from 'vue';
	const slotName = ref('header');
	</script>
	<template>
	  <Child>
	    <template #[slotName]>Dynamic Content</template>
	  </Child>
	</template>
	```
### 组件通信
#### Props
- 父组件通过 `props` 向子组件传递数据，支持响应式。
- **定义 props**：
	- 选项式：使用 `props` 选项。
	- 语法糖：使用 `defineProps`。
- **注意**：
	- Props 是只读的，子组件不能直接修改。
	- 支持类型校验（`String`、`Number` 等）和默认值。
- 示例：
	```js
	<!-- 子组件 -->
	<script setup>
	const props = defineProps({
	  message: String,
	  count: { type: Number, default: 0 },
	});
	</script>
	<template>
	  <div>{{ message }}: {{ count }}</div>
	</template>
	
	<!-- 父组件 -->
	<template>
	  <Child :message="'Hello'" :count="10" />
	</template>
	```
#### 自定义事件（Emits）
- 子组件通过 `emit` 触发事件，父组件监听处理。
- **定义 emits**：
	- 选项式：使用 `emits` 选项。
	- 语法糖：使用 `defineEmits`。
- **事件校验**：通过 `emits` 选项定义事件校验函数，提升类型安全性。
- 在模板中使用 `$emit` 触发事件
- 示例：
	```js
	<!-- 子组件 -->
	<script setup>
	const emit = defineEmits(['update']);
	const handleClick = () => emit('update', 42);
	</script>
	<template>
	  <button @click="handleClick">Update</button>
	</template>
	
	<!-- 父组件 -->
	<template>
	  <Child @update="value => console.log(value)" />
	</template>
	```
#### Provide 和 Inject
- 用于跨层级数据传递，适合祖孙组件通信。
- **注意**：支持响应式数据，provide 的值变化会触发后代组件更新。
- 示例：
	```js
	<!-- 祖先组件 -->
	<script setup>
	import { provide, ref } from 'vue';
	const theme = ref('light');
	provide('theme', theme);
	</script>
	
	<!-- 后代组件 -->
	<script setup>
	import { inject } from 'vue';
	const theme = inject('theme'); // 获取 theme
	</script>
	<template>
	  <div>Theme: {{ theme }}</div>
	</template>
	```

#### 事件总线（Event Bus）
- Vue3 移除内置事件总线，推荐使用外部库（如 mitt）实现全局事件通信。
- **替代方案**：复杂场景推荐使用 Pinia 或 Vuex 管理全局状态。
- 示例（使用 mitt）：
	```js
	import mitt from 'mitt';
	const emitter = mitt();
	// 组件 A
	emitter.emit('someEvent', { data: 'Hello' });
	// 组件 B
	emitter.on('someEvent', payload => console.log(payload));
	```
### 组件高级特性
#### 组件递归与自引用
- 组件通过自身名称调用实现递归，适合树形结构。
- **注意**：递归组件需显式指定 name，并设置终止条件避免无限递归。
- 示例：
	```js
	<script setup>
	import { ref } from 'vue';
	defineOptions({ name: 'TreeNode' }); // 显式指定组件名
	const props = defineProps(['node']);
	</script>
	<template>
	  <div>
	    {{ node.name }}
	    <TreeNode v-for="child in node.children" :key="child.id" :node="child" />
	  </div>
	</template>
	```
#### 自定义 v-model
- 允许子组件支持 `v-model` 双向绑定。
- 示例：
	```js
	<!-- 子组件 -->
	<script setup>
	const props = defineProps(['modelValue']);
	const emit = defineEmits(['update:modelValue']);
	const handleInput = (e) => emit('update:modelValue', e.target.value);
	</script>
	<template>
	  <input :value="modelValue" @input="handleInput" />
	</template>
	
	<!-- 父组件 -->
	<template>
	  <Child v-model="value" />
	</template>
	<script setup>
	import { ref } from 'vue';
	const value = ref('');
	</script>
	```
- **多 v-model**：Vue3 支持多个 v-model 绑定。
	- 示例：
		```js
		<!-- 子组件 -->
		<script setup>
		const props = defineProps(['firstName', 'lastName']);
		const emit = defineEmits(['update:firstName', 'update:lastName']);
		</script>
		<template>
		  <input :value="firstName" @input="emit('update:firstName', $event.target.value)" />
		  <input :value="lastName" @input="emit('update:lastName', $event.target.value)" />
		</template>
		<!-- 父组件 -->
		<template>
		  <Child v-model:firstName="first" v-model:lastName="last" />
		</template>
		```
#### 动态组件与 keep-alive
- `<keep-alive>`：缓存动态组件状态，避免销毁。
- **配置**：
	- include：指定缓存的组件名称。
	- exclude：排除某些组件不缓存。
	- max：限制缓存的最大组件数。
- 示例：
	```js
	<script setup>
	import { ref } from 'vue';
	import ComponentA from './ComponentA.vue';
	import ComponentB from './ComponentB.vue';
	const current = ref('ComponentA');
	</script>
	<template>
	  <keep-alive>
	    <component :is="current" />
	  </keep-alive>
	  <button @click="current = current === 'ComponentA' ? 'ComponentB' : 'ComponentA'">
	    Toggle
	  </button>
	</template>
	```
#### 组件事件
- 子组件通过 emit 触发自定义事件，父组件通过 @ 监听。
- 示例：
	```js
	<!-- 子组件 -->
	<script setup>
	const emit = defineEmits(['customEvent']);
	const trigger = () => emit('customEvent', 'payload');
	</script>
	<template>
	  <button @click="trigger">Trigger Event</button>
	</template>
	<!-- 父组件 -->
	<template>
	  <Child @customEvent="payload => console.log(payload)" />
	</template>
	```
## 指令
### 内置指令
#### 属性绑定：v-bind
- **作用**：动态绑定 DOM 元素的属性（如 `class`、`style`、`id` 等）。
- **语法**：
	- 完整形式：`v-bind:attribute="value"`
	- 简写：`:attribute="value"`
	- Vue 对类和样式的绑定做了增强，表达式除了字符串，也可以是对象和数组。
- **用法**：
	- 绑定单个属性：`:id="dynamicId"`
	- 绑定对象：`:class="{ 'card': true, 'active': isActive, [typeClass]: true }`
	- 绑定数组：`:class="['card', isActive ? 'active' : '', typeClass]"`
	- 混合绑定：`:class="['container', {'active': isToggle}]"`
	- 绑定样式：`:style="{ color: textColor, fontSize: fontSize + 'px' }"`
- **注意**：
	- 支持动态属性名：`:[attributeName]="value"`
	- 支持数组语法：`:class="[classA, classB]"`
- **示例**：
	```js
	<template>
	  <div :class="{ active: isActive }" :style="{ color: textColor }">Content</div>
	  <img :src="imageUrl" alt="Dynamic Image" />
	</template>
	<script setup>
	import { ref } from 'vue';
	const isActive = ref(true);
	const textColor = ref('blue');
	const imageUrl = ref('https://example.com/image.jpg');
	</script>
	```
- **注意**：
	- 绑定对象中的属性值，推荐添加引号，合法标识符可以不加引号，如 `text-red` 这种属性值就需要引号。
#### 事件监听：v-on
- **作用**：监听 DOM 事件并执行指定的方法或表达式。
- **语法**：
	- 完整形式：`v-on:event="handler"`
	- 简写：`@event="handler"`
- **用法**：
	- 绑定事件处理函数：`@click="handleClick"`
	- 内联表达式：`@click="count++"`
	- 支持事件修饰符：`.stop`、`.prevent`、`.capture`、`.self`、`.once`、`.passive`
	- 支持按键修饰符：`.enter`、`.tab`、`.esc` 等
- **修饰符说明**：
	- `.stop`：调用 `event.stopPropagation()`，阻止事件冒泡。
	- `.prevent`：调用 `event.preventDefault()`，阻止默认行为。
	- `.once`：事件只触发一次。
	- `.self`：仅当事件从绑定元素自身触发时执行。
	- `.passive`：提高滚动事件的性能。
- **自定义按键别名**：
	- 定义：`app.config.keyCodes.customKey = 13`
	- 使用：`@keyup.customKey="handler"`
- **示例**：
	```js
	<template>
	  <button @click="increment">Click me</button>
	  <form @submit.prevent="submitForm">Submit</form>
	  <input @keyup.enter="handleEnter" />
	</template>
	<script setup>
	import { ref } from 'vue';
	const count = ref(0);
	const increment = () => count.value++;
	const submitForm = () => console.log('Form submitted');
	const handleEnter = () => console.log('Enter key pressed');
	</script>
	```
#### 双向绑定：v-model
- **作用**：在表单元素和组件上实现双向数据绑定。
- **语法**：`v-model="value"`
- **用法**：
	- 支持表单元素：`<input>`, `<textarea>`, `<select>`, `<checkbox>`, `<radio>`
	- 支持自定义组件（通过 `modelValue` 和 `update:modelValue` 事件）。
- **示例（表单元素）**：
	```js
	<template>
	  <input v-model="text" placeholder="Enter text" />
	  <input type="checkbox" v-model="isChecked" />
	  <select v-model="selected">
	    <option value="option1">Option 1</option>
	    <option value="option2">Option 2</option>
	  </select>
	</template>
	<script setup>
	import { ref } from 'vue';
	const text = ref('');
	const isChecked = ref(false);
	const selected = ref('option1');
	</script>
	```
- **示例（自定义组件）**：
	```js
	<!-- 子组件 -->
	<template>
	  <input :value="modelValue" @input="$emit('update:modelValue', $event.target.value)" />
	</template>
	<script setup>
	defineProps(['modelValue']);
	defineEmits(['update:modelValue']);
	</script>
	<!-- 父组件 -->
	<template>
	  <Child v-model="value" />
	</template>
	<script setup>
	import { ref } from 'vue';
	const value = ref('');
	</script>
	```
- **多 v-model**：
	- Vue3 支持在组件上绑定多个 `v-model`。
	- 示例：
		```js
		<!-- 子组件 -->
		<template>
		  <input :value="firstName" @input="$emit('update:firstName', $event.target.value)" />
		  <input :value="lastName" @input="$emit('update:lastName', $event.target.value)" />
		</template>
		<script setup>
		defineProps(['firstName', 'lastName']);
		defineEmits(['update:firstName', 'update:lastName']);
		</script>
		<!-- 父组件 -->
		<template>
		  <Child v-model:firstName="first" v-model:lastName="last" />
		</template>
		<script setup>
		import { ref } from 'vue';
		const first = ref('');
		const last = ref('');
		</script>
		```
- **修饰符**：
	- `.lazy`：在 `change` 事件后更新，而不是 `input`。
	- `.number`：将输入值转换为数字。
	- `.trim`：去除首尾空格。
#### 条件渲染：v-if、v-else、v-else-if
- **作用**：根据条件控制 DOM 元素的显示或隐藏。
- **语法**：
	- `v-if="condition"`
	- `v-else`
	- `v-else-if="condition"`
- **用法**：
	- `v-if` 添加或移除 DOM 元素，适合不频繁切换的场景。
	- 必须连续使用，不能有其他元素中断。
- **与** `v-show` **的区别**：
	- `v-if`：控制 DOM 元素的创建/销毁，性能开销较大。
	- `v-show`：通过 `display: none` 控制显示，适合频繁切换。
- **示例**：
	```js
	<template>
	  <div v-if="isVisible">Visible Content</div>
	  <div v-else-if="isLoading">Loading...</div>
	  <div v-else>Error</div>
	</template>
	<script setup>
	import { ref } from 'vue';
	const isVisible = ref(true);
	const isLoading = ref(false);
	</script>
	```
#### 显示/隐藏：v-show
- **作用**：通过 CSS 的 `display` 属性控制元素显示。
- **语法**：`v-show="condition"`
- **注意**：
	- `v-show` 不支持 `<template>` 元素。
	- 适用于频繁切换的场景，性能优于 `v-if`。
- **示例**：
	```js
	<template>
	  <div v-show="isVisible">Visible Content</div>
	</template>
	<script setup>
	import { ref } from 'vue';
	const isVisible = ref(true);
	</script>
	```
#### 列表渲染：v-for
- **作用**：根据数组或对象渲染列表。
- **语法**：
	- 数组：`v-for="(item, index) in items"`
	- 对象：`v-for="(value, key, index) in object"`
- **用法**：
	- 推荐使用 `:key` 指定唯一标识，优化 DOM 更新。
	- 支持嵌套循环和 `v-if` 结合。
- **注意**：
	- `:key` 应为唯一值（如 ID），避免使用 `index` 作为 `key`。
	- 列表更新时，Vue 使用高效的 `diff` 算法，依赖 key 优化。
- **示例**：
	```js
	<template>
	  <ul>
	    <li v-for="(item, index) in items" :key="item.id">
	      {{ index }}: {{ item.name }}
	    </li>
	  </ul>
	  <div v-for="(value, key) in user" :key="key">
	    {{ key }}: {{ value }}
	  </div>
	</template>
	<script setup>
	import { ref } from 'vue';
	const items = ref([{ id: 1, name: 'Item 1' }, { id: 2, name: 'Item 2' }]);
	const user = ref({ name: 'Vue', version: 3 });
	</script>
	```
#### 插槽指令：v-slot
- **作用**：用于指定插槽内容，支持默认插槽、具名插槽和作用域插槽。
- **语法**：
	- 完整形式：`v-slot:slotName="slotProps"`
	- 简写：`#slotName="slotProps"`
- **注意**：
	- 仅在 `<template>` 或组件上使用。
	- 作用域插槽支持解构：`#default="{ item, index }"`.
- **示例**：
	```js
	<!-- 子组件 -->
	<template>
	  <slot name="header" :data="data">Default Header</slot>
	  <slot :item="item"></slot>
	</template>
	<script setup>
	import { ref } from 'vue';
	const data = ref('Header Data');
	const item = ref({ name: 'Vue' });
	</script>
	<!-- 父组件 -->
	<template>
	  <Child>
	    <template #header="{ data }">Header: {{ data }}</template>
	    <template #default="{ item }">Item: {{ item.name }}</template>
	  </Child>
	</template>
	```
#### 性能优化指令：v-once 和 v-memo
- `v-once`
	- 作用：元素和组件只渲染一次，后续不随数据变化重新渲染。
	- 用途：优化静态内容的渲染性能。
	- 示例：
		```js
		<template>
		  <div v-once>{{ staticContent }}</div>
		</template>
		<script setup>
		import { ref } from 'vue';
		const staticContent = ref('Static Text');
		</script>
		```
- `v-memo`
	- 作用：缓存 DOM 子树，仅当指定依赖变化时重新渲染。
	- 语法：`v-memo="[dep1, dep2]"`
	- 用途：优化复杂组件的渲染，类似 React 的 `memo`。
	- 示例：
		```js
		<template>
		  <div v-memo="[value]">
		    Expensive Content: {{ value }}
		  </div>
		</template>
		<script setup>
		import { ref } from 'vue';
		const value = ref(0);
		</script>
		```
#### 其他指令
- `v-pre`
	- 作用：跳过元素及其子元素的编译，直接输出原始内容。
	- 用途：优化静态内容，减少编译开销。
	- 示例：
		```js
		<div v-pre>{{ this will not be compiled }}</div>
		```
- `v-cloak`
	- 作用：隐藏未编译的模板，直到 Vue 完成渲染。
	- 用途：防止页面闪烁。
	- 示例：
		```js
		<style>
		[v-cloak] { display: none; }
		</style>
		<div v-cloak>{{ message }}</div>
		```
### 自定义指令
#### 定义与注册
- **全局注册**：
	- 使用 `app.directive` 注册。
	- 示例：
		```js
		import { createApp } from 'vue';
		const app = createApp({});
		app.directive('focus', {
		  mounted(el) {
		    el.focus();
		  },
		});
		```
- **局部注册**：
	- 在组件的 `directives` 选项中注册。
	- 示例：
		```js
		export default {
		  directives: {
		    focus: {
		      mounted(el) {
		        el.focus();
		      },
		    },
		  },
		};
		```
- `<script setup>` **语法糖**：
	- 使用 `v-` 前缀定义指令。
	- 示例：
		```js
		<script setup>
		const vFocus = {
		  mounted: (el) => el.focus(),
		};
		</script>
		<template>
		  <input v-focus />
		</template>
		```
#### 指令钩子
- 自定义指令支持以下生命周期钩子：
	- `created`：元素初始化时调用。
	- `beforeMount`：元素插入 DOM 前。
	- `mounted`：元素插入 DOM 后。
	- `beforeUpdate`：元素更新前。
	- `updated`：元素更新后。
	- `beforeUnmount`：元素卸载前。
	- `unmounted`：元素卸载后。
- **钩子参数**：
	- `el`：绑定指令的 DOM 元素。
	- `binding`：包含指令的值（`value`）、参数（`arg`）、修饰符（`modifiers`）等。
	- `vnode`：虚拟节点。
	- `prevVnode`：上一次的虚拟节点。
- **示例**：
	```js
	app.directive('highlight', {
	  mounted(el, binding) {
	    el.style.backgroundColor = binding.value; // 动态设置背景色
	  },
	  updated(el, binding) {
	    if (binding.value !== binding.oldValue) {
	      el.style.backgroundColor = binding.value;
	    }
	  },
	});
	
	<template>
	  <div v-highlight="'yellow'">Highlighted Text</div>
	</template>
	```
#### 参数与修饰符
- **参数**：通过 `v-directive:arg` 指定。
	- 示例：`v-directive:arg="value"`
- **修饰符**：通过 `.modifier` 添加额外行为。
	- 示例：`v-directive.modifier="value"`
- **示例**：
	```js
	<template>
	  <div v-resize:width.bold="size">Resizable Div</div>
	</template>
	<script setup>
	import { ref } from 'vue';
	const size = ref(100);
	const vResize = {
	  mounted(el, binding) {
	    el.style[binding.arg] = `${binding.value}px`; // 设置宽度或高度
	    if (binding.modifiers.bold) {
	      el.style.fontWeight = 'bold';
	    }
	  },
	};
	</script>
	```
#### 典型应用场景
- **拖拽指令**：
	- 示例：
		```js
		app.directive('drag', {
		  mounted(el) {
		    el.style.position = 'absolute';
		    let isDragging = false;
		    let currentX, currentY;
		    el.addEventListener('mousedown', (e) => {
		      isDragging = true;
		      currentX = e.offsetX;
		      currentY = e.offsetY;
		    });
		    document.addEventListener('mousemove', (e) => {
		      if (isDragging) {
		        el.style.left = `${e.clientX - currentX}px`;
		        el.style.top = `${e.clientY - currentY}px`;
		      }
		    });
		    document.addEventListener('mouseup', () => {
		      isDragging = false;
		    });
		  },
		});
		
		<template>
		  <div v-drag>Drag me</div>
		</template>
		```
- **权限控制**：
	- 示例：
		```js
		app.directive('permission', {
		  mounted(el, binding) {
		    const userPermissions = ['admin', 'editor']; // 模拟用户权限
		    if (!userPermissions.includes(binding.value)) {
		      el.remove(); // 无权限时移除元素
		    }
		  },
		});
		
		<template>
		  <button v-permission="'admin'">Admin Only</button>
		</template>
		```
## 模板语法
### 模板基础
#### 插值表达式
- **作用**：在模板中动态显示响应式数据。
- **语法**：使用双大括号 `{{ expression }}`。
- **用法**：
	- 显示变量值：`{{ message }}`
	- 计算表达式：`{{ count + 1 }}`
	- 调用方法：`{{ getFullName() }}`
- **自动解包**：
	- 对于 `ref` 类型的响应式数据，模板中会自动解包 `.value。`
	- 示例：`const count = ref(0)` 在模板中直接使用 `{{ count }}`，无需 `{{ count.value }}`。
- **限制**：
	- 仅支持简单的 JavaScript 表达式（如 `a + b`、`item.name`）。
	- 不支持语句（如 `if`、`for`）或复杂逻辑，需在 `<script>` 中处理。
- **示例**：
	```js
	<template>
	  <div>{{ message }}</div>
	  <div>Count: {{ count + 1 }}</div>
	</template>
	<script setup>
	import { ref } from 'vue';
	const message = ref('Hello, Vue3!');
	const count = ref(0);
	</script>
	```
#### JavaScript 表达式
- **支持的表达式**：
	- 算术运算：`{{ number + 1 }}`
    - 逻辑运算：`{{ isActive && isVisible }}`
    - 三元运算：`{{ isActive ? 'Active' : 'Inactive' }}`
    - 对象/数组访问：`{{ user.name }}`, `{{ items[0] }}`
    - 方法调用：`{{ formatDate(date) }}`
- **注意**：
	- 避免在模板中编写复杂逻辑，推荐使用计算属性（`computed`）或方法。
	- 表达式必须有返回值，不能是 `undefined` 或 `void`。
- **示例**：
	```js
	<template>
	  <div>{{ user.name.toUpperCase() }}</div>
	  <div>{{ count > 0 ? 'Positive' : 'Zero or Negative' }}</div>
	</template>
	<script setup>
	import { ref } from 'vue';
	const user = ref({ name: 'Vue' });
	const count = ref(0);
	</script>
	```
## 路由管理
### Vue Router 概述
#### 核心特性
- **声明式导航**：通过 `<router-link>` 组件定义导航链接。
- **编程式导航**：通过 `router.push`、`router.replace` 等方法控制导航。
- **动态路由**：支持参数化路由（如 `/user/:id`）。
- **嵌套路由**：支持多级路由嵌套。
- **导航守卫**：提供全局、路由独享和组件内的路由拦截。
- **路由模式**：支持 Hash 和 History 模式。
#### 安装与配置
- **安装**
	```js
	npm install vue-router@4
	```
- **配置路由**
	- 创建路由实例并挂载到 Vue 应用。
	- 示例：
		```js
		import { createApp } from 'vue';
		import { createRouter, createWebHistory } from 'vue-router';
		import App from './App.vue';
		import Home from './views/Home.vue';
		import About from './views/About.vue';
		
		const router = createRouter({
		  history: createWebHistory(),
		  routes: [
		    { path: '/', component: Home },
		    { path: '/about', component: About },
		  ],
		});
		
		const app = createApp(App);
		app.use(router);
		app.mount('#app');
		```
- **说明**：
	- `createWebHistory`：使用 HTML5 History 模式。
	- `createWebHashHistory`：使用 Hash 模式（URL 带 `#`）。
	- `routes`：定义路由映射，包含路径和组件。
### 路由定义
#### 基本路由
- **定义**：每个路由对象包含 path、component、name、meta 和 children。
- **示例**：
	```js
	const routes = [
	  { path: '/', component: Home },
	  { path: '/about', component: About },
	];
	```
- **渲染**：使用 `<router-view>` 渲染匹配的组件。
	```js
	<template>
	  <router-view></router-view>
	</template>
	```
#### 动态路由
- **作用**：通过动态路径参数匹配路由（如 /user/:id）。
- **示例**：
	```js
	const routes = [
	  { path: '/user/:id', component: User },
	];
	```
	```js
	<!-- User.vue -->
	<template>
	  <div>User ID: {{ $route.params.id }}</div>
	</template>
	<script setup>
	import { useRoute } from 'vue-router';
	const route = useRoute();
	console.log(route.params.id); // 访问动态参数
	</script>
	```
- **多参数**：
	```js
	{ path: '/user/:id/post/:postId', component: Post },
	```
- **可选参数**：使用 `?` 表示参数可选。
	```js
	{ path: '/user/:id?', component: User },
	```
#### 嵌套路由
- **作用**：支持子路由，适合复杂页面布局。
- **示例**：
	```js
	const routes = [
	  {
	    path: '/user/:id',
	    component: User,
	    children: [
	      { path: '', component: UserProfile }, // 默认子路由
	      { path: 'posts', component: UserPosts },
	    ],
	  },
	];
	```
	```js
	<!-- User.vue -->
	<template>
	  <div>
	    <h2>User Profile</h2>
	    <router-view></router-view>
	  </div>
	</template>
	```
- **说明**：
	- 父组件使用 `<router-view>` 渲染子路由组件。
	- 访问 `/user/123` 渲染 UserProfile，`/user/123/posts` 渲染 UserPosts。
#### 命名路由
- **作用**：通过 name 属性为路由命名，简化导航。
- **示例**：
```js
const routes = [
  { path: '/about', name: 'about', component: About },
];

<template>
  <router-link :to="{ name: 'about' }">Go to About</router-link>
</template>
```
#### 重定向与别名
- **重定向**
	- 使用 `redirect` 重定向到其他路由。
	- 示例：
		```js
		const routes = [
		  { path: '/home', redirect: '/' },
		  { path: '/old', redirect: { name: 'about' } },
		];
		```
- **别名**：
	- 使用 `alias` 为路由定义别名，同一组件支持多个 URL。
	- 示例：
		```js
		const routes = [
		  { path: '/about', component: About, alias: '/info' },
		];
		```
### 导航管理
#### 声明式导航
- **组件**：`<router-link>`
- **用法**：
	- 基本跳转：`<router-link to="/about">About</router-link>`
	- 动态跳转：`<router-link :to="{ path: '/user/' + userId }">User</router-link>`
	- 命名路由：`<router-link :to="{ name: 'about' }">About</router-link>`
- **示例**：
	```js
	<template>
	  <router-link to="/">Home</router-link>
	  <router-link :to="{ name: 'user', params: { id: 123 }}">User 123</router-link>
	</template>
	<script setup>
	import { ref } from 'vue';
	const userId = ref(123);
	</script>
	```
- **属性**：
	- `active-class`：设置激活时的类名。
	- `exact-active-class`：精确匹配时的类名。
	- 示例：
		```js
		<router-link to="/" active-class="active">Home</router-link>
		```
#### 编程式导航
- **方法**：
	- `router.push(location)`：导航并添加历史记录。
	- `router.replace(location)`：导航但不添加历史记录。
	- `router.go(n)`：前进或后退 n 步。
	- `router.back()`：后退一步。
	- `router.forward()`：前进一步。
- **示例**：
	```js
	<template>
	  <button @click="goToUser">Go to User</button>
	</template>
	<script setup>
	import { useRouter } from 'vue-router';
	const router = useRouter();
	const goToUser = () => {
	  router.push({ name: 'user', params: { id: 123 } });
	};
	</script>
	```
- **参数**：
	- 字符串：`router.push('/about')`
	- 对象：`router.push({ path: '/user/123', query: { tab: 'posts' } })`
	- 命名路由：`router.push({ name: 'user', params: { id: 123 } })`
#### 路由参数
- **路径参数**：通过 `:param` 定义，访问 `route.params.param`。
- **查询参数**：通过 `?key=value` 传递，访问 `route.query.key`。
- **示例**：
	```js
	<template>
	  <div>
	    User ID: {{ route.params.id }}
	    Tab: {{ route.query.tab }}
	  </div>
	</template>
	<script setup>
	import { useRoute } from 'vue-router';
	const route = useRoute();
	</script>
	```
### 导航守卫
#### 全局守卫
- **类型**：
	- `beforeEach`：导航触发前调用。
	- `beforeResolve`：路由解析完成后调用。
	- `afterEach`：导航完成后调用。
- **示例（权限控制）**：
	```js
	router.beforeEach((to, from, next) => {
	  const isAuthenticated = checkAuth(); // 假设的认证函数
	  if (to.meta.requiresAuth && !isAuthenticated) {
	    next('/login'); // 重定向到登录页
	  } else {
	    next(); // 继续导航
	  }
	});
	```
- **参数**：
	- `to`：目标路由对象。
	- `from`：当前路由对象。
	- `next`：控制导航（`next()`, `next('/path')`, `next(false)`）。
#### 路由独享守卫
- **定义**：在路由配置中添加 `beforeEnter`。
- **示例**：
	```js
	const routes = [
	  {
	    path: '/admin',
	    component: Admin,
	    beforeEnter: (to, from, next) => {
	      if (checkAdmin()) {
	        next();
	      } else {
	        next('/403');
	      }
	    },
	  },
	];
	```
#### 组件内守卫
- **钩子**：
	- `beforeRouteEnter`：进入组件前。
	- `beforeRouteUpdate`：路由更新时（参数变化）。
	- `beforeRouteLeave`：离开组件前。
- **示例**：
	```js
	<script>
	export default {
	  beforeRouteEnter(to, from, next) {
	    next(vm => {
	      // 通过 vm 访问组件实例
	      vm.loadData();
	    });
	  },
	  beforeRouteUpdate(to, from, next) {
	    this.loadData(to.params.id);
	    next();
	  },
	  beforeRouteLeave(to, from, next) {
	    if (confirm('Leave without saving?')) {
	      next();
	    } else {
	      next(false);
	    }
	  },
	  methods: {
	    loadData(id) {
	      // 加载数据
	    },
	  },
	};
	</script>
	```
- `<script setup>` **支持**：
	```js
	<script setup>
	import { useRoute, onBeforeRouteUpdate } from 'vue-router';
	const route = useRoute();
	onBeforeRouteUpdate((to, from) => {
	  console.log('Route updated:', to.params.id);
	});
	</script>
	```
### 路由懒加载与异步组件
- 路由懒加载通过动态导入组件减少首屏加载时间，结合 Vue3 的 defineAsyncComponent 实现。
- **示例**：
	```js
	import { defineAsyncComponent } from 'vue';
	const routes = [
	  {
	    path: '/about',
	    component: defineAsyncComponent(() => import('./views/About.vue')),
	  },
	];
	```
- **高级配置**：
	```js
	const About = defineAsyncComponent({
	  loader: () => import('./views/About.vue'),
	  loadingComponent: LoadingComponent,
	  delay: 200,
	  timeout: 3000,
	});
	```
- **好处**：
	- 按需加载组件，优化初始加载性能。
	- 支持加载中和错误状态的自定义组件。
### 路由元信息（Meta）
- **作用**：为路由添加元数据，用于权限控制、页面标题等。
- **示例**：
	```js
	const routes = [
	  {
	    path: '/admin',
	    component: Admin,
	    meta: { requiresAuth: true, title: 'Admin Panel' },
	  },
	];
	```
- **使用**：
	```js
	router.beforeEach((to, from, next) => {
	  document.title = to.meta.title || 'Default Title';
	  if (to.meta.requiresAuth && !isAuthenticated()) {
	    next('/login');
	  } else {
	    next();
	  }
	});
	```
### 路由模式
#### Hash 模式
- **特点**：URL 包含 `#`（如 `example.com/#/about）`。
- **配置**：
	```js
	import { createRouter, createWebHashHistory } from 'vue-router';
	const router = createRouter({
	  history: createWebHashHistory(),
	  routes: [],
	});
	```
- **优点**：
	- 无需服务器配置，适合静态部署。
	- 兼容性好，适用于所有浏览器。
- **缺点**：URL 不美观，SEO 不友好。
#### History 模式
- **特点**：使用标准 URL（如 `example.com/about`）。
- **配置**：
	```js
	import { createRouter, createWebHistory } from 'vue-router';
	const router = createRouter({
	  history: createWebHistory(),
	  routes: [],
	});
	```
- **优点**：URL 简洁，SEO 友好。
- **缺点**：
	- 需要服务器支持，配置 404 重定向到 `index.html`。
	- 示例（Nginx 配置）：
		```js
		location / {
		  try_files $uri $uri/ /index.html;
		}
		```
## 状态管理
### Vuex
#### Vuex 概述
- 作用：集中管理应用的共享状态，确保数据流可预测。
- **核心概念**：
	- **State**：单一状态树，存储所有共享数据。
	- **Getters**：计算属性，基于 state 派生数据。
	- **Mutations**：同步修改 state 的唯一方式。
	- **Actions**：处理异步逻辑，提交 mutations。
	- **Modules**：模块化组织状态，适合大型项目。
- **安装**：
	```js
	npm install vuex@next
	```
	```js
	import { createApp } from 'vue';
	import { createStore } from 'vuex';
	import App from './App.vue';
	
	const store = createStore({
	  state() {
	    return { count: 0 };
	  },
	});
	
	const app = createApp(App);
	app.use(store);
	```
#### 核心概念详解
- **State**
	- 存储应用的全局状态，响应式。
	- **访问方式**：
		- Options API：`this.$store.state.count`
		- Composition API：`useStore().state.count`
	- 示例：
		```js
		const store = createStore({
		  state() {
			return { count: 0, user: { name: 'Vue' } };
		  },
		});
		```
- **Getters**
	- 类似计算属性，基于 state 派生数据，缓存结果。
	- 访问：`this.$store.getters.doubleCount` 或 `useStore().getters.doubleCount`
	- 示例：
		```js
		const store = createStore({
		  state() {
		    return { count: 0 };
		  },
		  getters: {
		    doubleCount(state) {
		      return state.count * 2;
		    },
		  },
		});
		```
- **Mutations**
	- 同步修改 `state`，必须通过 `commit` 调用。
	- 调用：`this.$store.commit('increment', 1)`
	- 示例：
		```js
		const store = createStore({
		  state() {
		    return { count: 0 };
		  },
		  mutations: {
		    increment(state, payload) {
		      state.count += payload;
		    },
		  },
		});
		```
- **Actions**
	- 处理异步逻辑或复杂操作，通过 `dispatch` 调用。
	- 调用：`this.$store.dispatch('incrementAsync', 1)`
	- 示例：
		```js
		const store = createStore({
		  state() {
		    return { count: 0 };
		  },
		  mutations: {
		    increment(state, payload) {
		      state.count += payload;
		    },
		  },
		  actions: {
		    async incrementAsync({ commit }, payload) {
		      await new Promise(resolve => setTimeout(resolve, 1000));
		      commit('increment', payload);
		    },
		  },
		});
		```
- **Modules**
	- 将 `store` 拆分为模块，支持命名空间。
	- 访问：`this.$store.state.a.count`, `this.$store.commit('a/increment')`
	- 示例：
		```js
		const moduleA = {
		  state: () => ({ count: 0 }),
		  mutations: {
		    increment(state) {
		      state.count++;
		    },
		  },
		  actions: {
		    incrementAsync({ commit }) {
		      setTimeout(() => commit('increment'), 1000);
		    },
		  },
		};
		
		const store = createStore({
		  modules: {
		    a: moduleA,
		  },
		});
		```
#### Composition API 支持
- **useStore**
	- 在 `<script setup>` 或 Composition API 中使用。
	- 示例：
		```js
		<template>
		  <div>{{ count }} - {{ doubleCount }}</div>
		  <button @click="incrementAsync">Increment</button>
		</template>
		<script setup>
		import { useStore } from 'vuex';
		import { computed } from 'vue';
		
		const store = useStore();
		const count = computed(() => store.state.count);
		const doubleCount = computed(() => store.getters.doubleCount);
		const incrementAsync = () => store.dispatch('incrementAsync', 1);
		</script>
		```
- **响应式解构**
	- 使用 `computed` 确保 `state` 和 `getters` 的响应式。
	- 避免直接解构 `store.state`，否则丢失响应式。
#### Vuex 模块化与动态模块
- **模块化**
	- 使用命名空间（`namespaced: true`）避免命名冲突。
	- 示例：
		```js
		const moduleA = {
		  namespaced: true,
		  state: () => ({ count: 0 }),
		  mutations: {
		    increment(state) {
		      state.count++;
		    },
		  },
		};
		```
- **动态模块**
	- 运行时注册模块，适合按需加载。
	- 示例：
		```js
		store.registerModule('dynamicModule', {
		  state: () => ({ count: 0 }),
		  mutations: {
		    increment(state) {
		      state.count++;
		    },
		  },
		});
		```
#### Vuex 插件
- **作用**：扩展 Vuex 功能，如日志、持久化。
- **示例（日志插件）**：
	```js
	const logger = store => {
	  store.subscribe((mutation, state) => {
	    console.log(`Mutation: ${mutation.type}, Payload:`, mutation.payload);
	    console.log('State:', state);
	  });
	};
	
	const store = createStore({
	  state: () => ({ count: 0 }),
	  mutations: { increment(state) { state.count++; } },
	  plugins: [logger],
	});
	```
- **常见插件**：
	- `vuex-persistedstate`：持久化 `state` 到 `localStorage`。
	- 示例：
		```js
		import createPersistedState from 'vuex-persistedstate';
		
		const store = createStore({
		  state: () => ({ count: 0 }),
		  plugins: [createPersistedState()],
		});
		```
### Pinia
#### Pinia 概述
- **作用**：轻量级状态管理，支持 TypeScript 和 Composition API。
- **优势**：
	- 简单 API，无 `mutations`，直接修改 `state`。
	- 更好的 TypeScript 支持。
	- 支持 DevTools 调试。
	- 更小的打包体积。
- **安装**：
	```js
	npm install pinia
	```
	```js
	import { createApp } from 'vue';
	import { createPinia } from 'pinia';
	import App from './App.vue';
	
	const app = createApp(App);
	app.use(createPinia());
	```
#### 核心概念
- **Store**
	- 定义独立的 `store`，包含 `state`、`getters` 和 `actions`。
	- 示例：
		```js
		import { defineStore } from 'pinia';
		
		export const useCounterStore = defineStore('counter', {
		  state: () => ({
		    count: 0,
		    user: { name: 'Vue' },
		  }),
		  getters: {
		    doubleCount: state => state.count * 2,
		  },
		  actions: {
		    increment() {
		      this.count++;
		    },
		    async incrementAsync() {
		      await new Promise(resolve => setTimeout(resolve, 1000));
		      this.count++;
		    },
		  },
		});
		```
- **使用 Store**
	- 在组件中使用 `useCounterStore` 访问。
	- 示例：
		```js
		<template>
		  <div>{{ counter.count }} - {{ counter.doubleCount }}</div>
		  <button @click="counter.increment">Increment</button>
		  <button @click="counter.incrementAsync">Increment Async</button>
		</template>
		<script setup>
		import { useCounterStore } from './stores/counter';
		
		const counter = useCounterStore();
		</script>
		```
#### 响应式 Store
- **State 响应式**
	- Pinia 使用 Vue3 的响应式系统（`reactive` 和 `ref`）。
	- state 默认是 `reactive`，`getters` 类似 `computed`。
- **直接修改 State**
	- 无需 `mutations`，直接修改 `this.count`。
	- 示例：
		```js
		actions: {
		  increment() {
		    this.count++; // 直接修改
		  },
		}
		```
- **解构 Store**
	- 使用 `storeToRefs` 保持响应式。
	- 示例：
		```js
		<script setup>
		import { storeToRefs } from 'pinia';
		import { useCounterStore } from './stores/counter';
		
		const counter = useCounterStore();
		const { count, doubleCount } = storeToRefs(counter); // 保持响应式
		</script>
		```
#### 选项式与 Setup Store
- **选项式 Store**（如上例）：
	- 使用 `defineStore('id', { state, getters, actions })`。
	- 适合结构化代码。
- **Setup Store**：
	- 使用 `defineStore('id', () => { ... })`，类似 `<script setup>`。
	- 示例：
		```js
		import { ref, computed } from 'vue';
		import { defineStore } from 'pinia';
		
		export const useCounterStore = defineStore('counter', () => {
		  const count = ref(0);
		  const doubleCount = computed(() => count.value * 2);
		  const increment = () => count.value++;
		  return { count, doubleCount, increment };
		});
		```
	- 优势：更灵活，适合复杂逻辑，直接使用 Vue 的 Composition API。
#### Pinia 插件
- **作用**：扩展 `Store` 功能，如持久化、日志。
- **示例（自定义插件）**：
	```js
	import { createPinia } from 'pinia';
	
	const logPlugin = ({ store }) => {
	  store.$subscribe((mutation, state) => {
	    console.log(`Store ${store.$id} changed:`, state);
	  });
	};
	
	const pinia = createPinia();
	pinia.use(logPlugin);
	```
- **常见插件**：
	- `pinia-plugin-persistedstate`：持久化 Store 到 localStorage。
	- 示例：
		```js
		import { createPinia } from 'pinia';
		import piniaPluginPersistedstate from 'pinia-plugin-persistedstate';
		
		const pinia = createPinia();
		pinia.use(piniaPluginPersistedstate);
		
		export const useCounterStore = defineStore('counter', {
		  state: () => ({ count: 0 }),
		  persist: true, // 启用持久化
		});
		```
## 过渡与动画
### 过渡效果
#### `<transition>` 组件
- **作用**：为单个元素或组件的进入和离开添加过渡效果。
- **用法**：
	- 包裹需要过渡的元素或组件。
	- 配合 CSS 类或 JavaScript 钩子定义动画。
- **过渡类**： Vue 自动为 `<transition>` 元素添加以下 CSS 类：
	- 进入（Enter）：
		- `v-enter-from`：进入动画的起始状态。
		- `v-enter-active`：进入动画的执行阶段。
		- `v-enter-to`：进入动画的结束状态。
	- 离开（Leave）：
		- `v-leave-from`：离开动画的起始状态。
		- `v-leave-active`：离开动画的执行阶段。
		- `v-leave-to`：离开动画的结束状态。
- **示例（淡入淡出效果）**：
	```js
	<template>
	  <button @click="show = !show">Toggle</button>
	  <transition name="fade">
	    <div v-if="show" class="box">Content</div>
	  </transition>
	</template>
	<script setup>
	import { ref } from 'vue';
	const show = ref(true);
	</script>
	<style scoped>
	.fade-enter-active,
	.fade-leave-active {
	  transition: opacity 0.5s ease;
	}
	.fade-enter-from,
	.fade-leave-to {
	  opacity: 0;
	}
	.box {
	  width: 100px;
	  height: 100px;
	  background: blue;
	}
	</style>
	```
- **说明**：
	- `name="fade"` 定义过渡类前缀（如 `fade-enter-active`）。
	- `v-if` 控制元素显示，触发过渡。
	- `transition` CSS 属性定义动画效果。
	- 常用于用来包裹`<router-view>`组件，为组件切换提供过渡效果。
#### `<transition-group>` 组件
- **作用**：为列表中的元素（如 `v-for` 渲染的列表）添加进入、离开或移动动画。
- **特点**：
	- 每个列表项需要唯一的 `:key`。
	- 自动为每个子元素应用过渡类。
	- 支持列表项的移动动画（`v-move` 类）。
- **示例（列表渐变）**：
	```js
	<template>
	  <button @click="addItem">Add Item</button>
	  <transition-group name="list" tag="ul">
	    <li v-for="item in items" :key="item.id" class="item">
	      {{ item.name }}
	    </li>
	  </transition-group>
	</template>
	<script setup>
	import { ref } from 'vue';
	const items = ref([
	  { id: 1, name: 'Item 1' },
	  { id: 2, name: 'Item 2' },
	]);
	const addItem = () => {
	  items.value.push({ id: Date.now(), name: `Item ${items.value.length + 1}` });
	};
	</script>
	<style scoped>
	.list-enter-active,
	.list-leave-active {
	  transition: all 0.5s ease;
	}
	.list-enter-from,
	.list-leave-to {
	  opacity: 0;
	  transform: translateY(20px);
	}
	.list-move {
	  transition: transform 0.5s ease;
	}
	.item {
	  padding: 10px;
	}
	</style>
	```
- **说明**：
	- `tag="ul"` 指定渲染的 HTML 标签。
	- `.list-move` 类为列表项重新排序时提供平滑移动动画。
	- `:key` 确保列表更新时正确应用动画。
#### 过渡模式
- **作用**：控制进入和离开动画的执行顺序。
- **选项**：
	- `in-out`：新元素先进入，旧元素后离开。
	- `out-in`：旧元素先离开，新元素后进入。
- **示例**：
	```js
	<template>
	  <transition name="fade" mode="out-in">
	    <div :key="page">{{ page }}</div>
	  </transition>
	</template>
	<script setup>
	import { ref } from 'vue';
	const page = ref('Page 1');
	</script>
	```
### JavaScript 钩子
#### 钩子函数
- **可用钩子**：
	- `before-enter`：进入动画前。
	- `enter`：进入动画执行。
	- `after-enter`：进入动画完成。
	- `enter-cancelled`：进入动画取消。
	- `before-leave`：离开动画前。
	- `leave`：离开动画执行。
	- `after-leave`：离开动画完成。
	- `leave-cancelled`：离开动画取消。
- **示例（使用 GSAP）**：
	```js
	<template>
	  <transition
	    @before-enter="beforeEnter"
	    @enter="enter"
	    @leave="leave"
	    :css="false"
  >	
	    <div v-if="show" class="box">Animated Content</div>
	  </transition>
	</template>
	<script setup>
	import { ref } from 'vue';
	import { gsap } from 'gsap';
	
	const show = ref(true);
	
	const beforeEnter = (el) => {
	  gsap.set(el, { opacity: 0, x: 100 });
	};
	
	const enter = (el, done) => {
	  gsap.to(el, {
	    opacity: 1,
	    x: 0,
	    duration: 0.5,
	    onComplete: done, // 调用 done 完成过渡
	  });
	};
	
	const leave = (el, done) => {
	  gsap.to(el, {
	    opacity: 0,
	    x: -100,
	    duration: 0.5,
	    onComplete: done,
	  });
	};
	</script>
	<style scoped>
	.box {
	  width: 100px;
	  height: 100px;
	  background: purple;
	}
	</style>
	```
- **说明**：
	- `:css="false"` 禁用 CSS 过渡，完全由 JavaScript 控制。
	- done 回调必须调用以完成过渡。
#### 动态过渡
- **作用**：根据条件动态切换过渡效果。
- **示例**：
	```js
	<template>
	  <transition :name="transitionName">
	    <div v-if="show" class="box">Dynamic Transition</div>
	  </transition>
	</template>
	<script setup>
	import { ref } from 'vue';
	const show = ref(true);
	const transitionName = ref('fade');
	</script>
	<style scoped>
	.fade-enter-active,
	.slide-enter-active {
	  transition: all 0.3s ease;
	}
	.fade-enter-from,
	.fade-leave-to {
	  opacity: 0;
	}
	.slide-enter-from,
	.slide-leave-to {
	  transform: translateX(100%);
	}
	.box {
	  width: 100px;
	  height: 100px;
	  background: orange;
	}
	</style>
	```
### 动态组件与路由过渡
#### 动态组件过渡
- **作用**：为 `<component :is>` 的动态组件切换添加动画。
- **示例**：
	```js
	<template>
	  <transition name="fade" mode="out-in">
	    <component :is="currentComponent" />
	  </transition>
	  <button @click="currentComponent = currentComponent === 'CompA' ? 'CompB' : 'CompA'">
	    Toggle Component
	  </button>
	</template>
	<script setup>
	import { ref } from 'vue';
	import CompA from './CompA.vue';
	import CompB from './CompB.vue';
	const currentComponent = ref('CompA');
	</script>
	<style scoped>
	.fade-enter-active,
	.fade-leave-active {
	  transition: opacity 0.3s ease;
	}
	.fade-enter-from,
	.fade-leave-to {
	  opacity: 0;
	}
	</style>
	```
#### 路由过渡
- **作用**：为路由切换添加动画。
- **示例**：
	```js
	<template>
	  <router-view v-slot="{ Component }">
	    <transition name="fade" mode="out-in">
	      <component :is="Component" />
	    </transition>
	  </router-view>
	</template>
	<style scoped>
	.fade-enter-active,
	.fade-leave-active {
	  transition: opacity 0.3s ease;
	}
	.fade-enter-from,
	.fade-leave-to {
	  opacity: 0;
	}
	</style>
	```
- **动态路由过渡**：
	- 根据路由元信息动态应用过渡。
		```js
		const routes = [
		  {
		    path: '/page1',
		    component: Page1,
		    meta: { transition: 'slide' },
		  },
		  {
		    path: '/page2',
		    component: Page2,
		    meta: { transition: 'fade' },
		  },
		];
		
		<template>
		  <router-view v-slot="{ Component, route }">
		    <transition :name="route.meta.transition || 'fade'">
		      <component :is="Component" />
		    </transition>
		  </router-view>
		</template>
		```



# 配置





# 附录
> [!website]
    > 1. https://cn.vuejs.org/](https://cn.vuejs.org/
    > 2. fwef

> [!code]
    > https://github.com/vuejs/](https://github.com/vuejs/

> [!video]
    > 

> [!doc]
    > 




# 语法

## 指令

Vue 中指令是特殊的 HTML 属性，以 v- 开头，用于在模板中绑定数据或添加动态行为，职责是当数据变化时，将其效果响应式反映在 DOM 上

### 指令格式

![image.png|890x24](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/a67206f6-4f07-46ef-baf2-1c9ca34946d5/image.png)

- `Name`：指令的名称，以 v- 开头
- `Argument`：指令的参数，通常是 HTML 属性，动态参数（属性或表达式）要包裹在方括号中
- `Modifiers`：指令修饰符，表明指令以一些特殊方式被绑定
- `Value`：组件实例中的属性或者 JS 表达式

### 文本插值

```bash
<span>Message: {{ msg }}</span>
```

双括号的内容会被替换为相应组件实例中的 msg 属性值

### `v-html`

- **作用：**将元素的 innerHTML 与组件实例的变量绑定，渲染 HTML
    
- **语法：**`v-html="rawHtml”`
    
- **特性：**不能使用拼接模板，并且容易造成 XSS 漏洞，仅在内容安全可信时使用
    
- 使用示例
    
    ```html
    <template>
    	<p>Using text interpolation: {{ rawHtml }}</p>
    	<p>Using v-html directive: <span v-html="rawHtml"></span></p>
    </template>
    
    <script setup>
    	import { ref } from 'vue';
    	const rawHtml = '<span style="color: red">This should be red.</span>'
    </script>
    ```
    

### `v-bind`

- **作用：**将 HTML 属性与组件实例的变量绑定
- **语法：**`v-bind:id="dynamicId”` 简写为 `:id="dynamicId”`
    - 如果指令参数和参数值一致，可以进一步简化为 `:id`
    - 如果组件实例属性是布尔类型，则根据布尔类型决定该属性是否存在元素上
    - 可以使用不带参数值的 v-bind 指令绑定多个值
    
- **特性：**
    - 如果组件实例属性不存在（null 或者 undefined），属性将被移除
    - 如果属性值不是字符串，不管是不是常量都需要使用 `v-bind`，让引号内变成 JS 表达式
- 类和样式的绑定：
    - v-bind 对 class 和 style 属性的绑定做了增强，表达式的值除了字符串外，可以是对象和数组

### `v-if`

- **作用**：内容只会在指令表达式或变量为真值时才会渲染
- **语法：`v-if="awesome”`**
- **组合：**
    - 可以使用 v-else 为 v-if 添加一个 else 区块，必须跟在 v-if 或 v-else-if 后面
    - 可以使用 v-else-if 进行多个条件判断，必须跟在 v-if 或 v-else-if 后面
- **特性：**判断为假值的块将不会在 DOM 渲染中保留

### `v-show`

- **作用：**和 v-if 一致
- **语法：**和 v-if 一致
- **特性：**判断为假值的块将会在 DOM 渲染中保留，只是切换了元素上 `display`的属性值
- **比较：**
    - v-if 是真实的按条件渲染，在切换时，确保了区块内事件监听器及子组件都被销毁与重建
    - v-if 是惰性的，如果条件为 false，不会做任何事，只会在条件为真时才被渲染
    - v-show 只是对元素 display 属性值的切换
    - v-if 有更高的切换开销，v-show 有更高的初始渲染开销

### `v-for`

- **作用：**用于渲染数据、对象等动态数据
    
- **语法：**
    
    - `v-for` 的值需要 `item in items` 的形式，`items` 是源数据，`item` 是迭代项的别名
    - 需要给每个元素唯一的 `key`，用于优化渲染性能 1️⃣
    - 如果列表项是对象，在定义迭代项时可以直接使用解构，从对象中提取属性
    - 可以在 `<template>` 标签上使用 `v-for`，渲染具有多个元素的块
    - `v-if`的优先级大于 `v-for`，两个必须同时使用时，建议将 `v-for` 的元素包裹 `v-if` 的元素
- 渲染数组
    
    ```html
    <template>
      <ul>
        <li v-for="item in items" :key="item.id">
          {{ item.name }}
        </li>
      </ul>
    </template>
    
    <script setup>
      const items = [
        { id: 1, name: 'Item 1' },
        { id: 2, name: 'Item 2' },
        { id: 3, name: 'Item 3' }
      ]
    </script>
    ```
    
    除了迭代项参数外，还支持第二个参数，表示当前项的位置索引
    
    可以在定义迭代项别名时，使用解构，直接从对象中提取属性
    
    ```html
    <template>
      <ul>
        <li v-for="({id,name},index) in items" :key="id">
          {{ index }}.{{ name }}
        </li>
      </ul>
    </template>
    ```
    
- 渲染对象
    
    ```html
    <template>
      <ul>
        <li v-for="(value, key, index) in user" :key="key">
          {{ index }}.{{ key }}: {{ value }}
        </li>
      </ul>
    </template>
    
    <script setup>
      const user = {
        name: 'Alice',
        age: 25,
        city: 'Beijing'
      }
    </script>
    ```
    
    除了 value 参数外，有两个可选参数：key 和 index，表示属性名和位置索引
    
    这里遍历的顺序以及索引，是对该对象调用 `Object.values()` 的返回值来决定的
    
- 嵌套渲染
    
    ```html
    <template>
      <ul>
        <li v-for="category in categories" :key="category.id">
          {{ category.name }}
          <ul>
            <li v-for="item in category.items" :key="item.id">
              {{ item.name }}
            </li>
          </ul>
        </li>
      </ul>
    </template>
    
    <script setup>
      const categories = [
        {
          id: 1,
          name: 'Fruits',
          items: [{ id: 101, name: 'Apple' }, { id: 102, name: 'Banana' }]
        },
        {
          id: 2,
          name: 'Vegetables',
          items: [{ id: 201, name: 'Carrot' }, { id: 202, name: 'Lettuce' }]
        }
      ]
    </script>
    ```
    
    每个 v-for 作用域都可以访问到父级作用域
    

### `v-on`

- 作用：监听 DOM 事件，并在事件触发时执行对应的事件处理器
    
- 语法：
    
    - `v-on:click="handler”` 简写为 `@click="handler”`
- 事件处理器：
    
    - 内联事件处理器： `@click="count++”` 、`@click=”foo()”`
    - 方法事件处理器：接收某个方法名或者某个方法的调用
- 事件修饰符：
    
    - `.stop`：阻止事件冒泡到父元素（点击子元素，父元素的点击事件将不执行）
    - `.prevent`：阻止事件默认行为
    - `.self`：仅限目标元素触发，子元素将不被触发
    - `.capture`：使用事件捕获模式，加在父元素上，优先父元素事件调用
    - `.once`：事件只触发一次，绑定的事件在首次触发后将被移除
    - `.passive`：告知浏览器不会调用`event.preventDefault()` ，提升移动端滚动性能
- 按键修饰符：`@keyup.=""` 、`@click.=""`
    
    - `.enter`
    - `.tab`
    - `.delete`：捕获 Delete 和 Backspace 两个按键
    - `.esc`
    - `.space`
    - `.up`
    - `.down`
    - `.left`
    - `.right`
    - `.ctrl`
    - `.alt`
    - `.shift`
    - `.meta`：Windows 中的 win 键，Mac 中的 Command 键
    - `.exact`：用于精确控制系统按键组合，表示只能按下该按键才能触发，不能组合按键
- 鼠标按键修饰符
    
    - `.left`
    - `.right`
    - `.middle`
- 冒泡和捕获
    
    - 捕获：事件从最顶层（window）沿着 DOM 向下传播到达目标元素
    - 冒泡：事件从目标元素向上冒泡，一次经过祖先元素，到达 window
- 内联事件处理器中访问事件参数
    
    ```html
    <template>
    	<!-- 使用特殊的 $event 变量 -->
    	<button @click="warn('Form cannot be submitted yet.', $event)">
    	  Submit
    	</button>
    	
    	<!-- 使用内联箭头函数 -->
    	<button @click="(event) => warn('Form cannot be submitted yet.', event)">
    	  Submit
    	</button>
    </template>
    
    <script setup>
    	function warn(message, event) {
    	  // 这里可以访问原生事件
    	  if (event) {
    	    event.preventDefault()
    	  }
    	  alert(message)
    	}
    </script>
    ```
    
- 组件中使用自定义事件
    
    ```html
    <template>
      <button @click="emitCustomEvent">Click Me</button>
    </template>
    
    <script setup>
    const emit = defineEmits(["customEvent"]); // 定义支持的事件
    
    const emitCustomEvent = () => {
      emit("customEvent", "Hello from ChildComponent!"); // 触发事件并传递参数
    };
    </script>
    ```
    
    ```html
    <template>
      <div>
        <ChildComponent @customEvent="handleCustomEvent" />
      </div>
    </template>
    
    <script setup>
    import ChildComponent from './ChildComponent.vue';
    
    const handleCustomEvent = (message) => {
      alert(message)
      console.log("Received message:", message);
    };
    </script>
    ```
    

### `v-model`

- **作用：**将表单控件和 JS 中的变量双向绑定
    
- **语法：**`v-model="text”`，这是 Vue 提供的语法糖，实际绑定：
    
    - value 属性：为表单控件提供初始值，以及当控件被选中时，等于绑定的变量的值
    - input 事件：当用户输入时，触发更新数据
- **默认行为：**
    
    - 对于复选框，默认绑定布尔类型参数
    - 如果元素上存在 value 属性，选中该表单时，value 的值将作为绑定变量的值或子项
- **值绑定：**
    
    - 对于单选框、复选框和下拉框默认绑定的静态字符串
        
        ```html
        <!-- `picked` 在被选择时是字符串 "a" -->
        <input type="radio" v-model="picked" value="a" />
        
        <!-- `toggle` 只会为 true 或 false -->
        <input type="checkbox" v-model="toggle" />
        
        <!-- `selected` 在第一项被选中时为字符串 "abc" -->
        <select v-model="selected">
          <option value="abc">ABC</option>
        </select>
        ```
        
    - 可以使用 v-bind 绑定动态值
        
        ```html
        <input type="radio" v-model="pick" :value="first" />
        <input type="radio" v-model="pick" :value="second" />
        <select v-model="selected">
          <!-- 内联对象字面量，选项选中时，将selected的值设置为{number: 123} -->
          <option :value="{ number: 123 }">123</option>
        </select>
        ```
        
- **修饰符：**
    
    - `.lazy`：默认情况下，在每次 input 事件后更新数据，更改为在 change 事件后更新数据
    - `.number`：将用户输入自动转为数字，当先输入数字时，后续的非数字将不能更新到变量
    - `.trim`：去除输入内容的两端空格
- 将多个复选框绑定到同一个数据或集合的值
    
    ```html
    <template>
    	<div>Checked names: {{ checkedNames }}</div>
    	
    	<input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
    	<label for="jack">Jack</label>
    	
    	<input type="checkbox" id="john" value="John" v-model="checkedNames" />
    	<label for="john">John</label>
    	
    	<input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
    	<label for="mike">Mike</label>
    </template>
    
    <script setup>
    	import { ref } from 'vue';
    	const checkedNames = ref([])
    </script>
    ```
    
- 动态渲染下拉框
    
    ```html
    <template>
    <select v-model="selected">
      <option v-for="option in options" :value="option.value">
        {{ option.text }}
      </option>
    </select>
    
    <div>Selected: {{ selected }}</div>
    </template>
    
    <script setup>
    import { ref } from 'vue';
    const selected = ref('A')
    
    const options = ref([
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ])
    </script>
    ```
    

## 响应式基础

### 响应式API

### 响应式的底层原理

## 计算属性

计算属性（computed）是一种基于响应式数据生成派生值的特性，他在模板或逻辑中充当动态值得角色

- 计算属性举例
    
    不使用计算属性：
    
    ```html
    <script setup>
    import { reactive, computed } from 'vue'
    
    const author = reactive({
      name: 'John Doe',
      books: [
        'Vue 2 - Advanced Guide',
        'Vue 3 - Basic Guide',
        'Vue 4 - The Mystery'
      ]
    })
    </script>
    
    <template>
      <p>Has published books:</p>
      <span>{{ author.books.length > 0 ? 'Yes' : 'No' }}</span>
    </template>
    ```
    
    使用计算属性：
    
    ```html
    <script setup>
    import { reactive, computed } from 'vue'
    
    const author = reactive({
      name: 'John Doe',
      books: [
        'Vue 2 - Advanced Guide',
        'Vue 3 - Basic Guide',
        'Vue 4 - The Mystery'
      ]
    })
    
    // 一个计算属性 ref
    const publishedBooksMessage = computed(() => {
      return author.books.length > 0 ? 'Yes' : 'No'
    })
    </script>
    
    <template>
      <p>Has published books:</p>
      <span>{{ publishedBooksMessage }}</span>
    </template>
    ```
    

计算属性依赖于响应式数据，否则将不会去计算

相比较普通方法，计算属性值会基于其响应式依赖被缓存，只有当响应式依赖发生更改时，计算属性才会重新计算，而不像普通方法总是在重渲染时执行

计算属性默认是只读的，可以提供 getter 和 setter 来创建可写属性

- 计算属性的可写
    
    ```html
    <template>
      <div>
        <p>摄氏温度：<input v-model="celsius" /></p>
        <p>华氏温度：<input v-model="fahrenheit" /></p>
      </div>
    </template>
    
    <script setup>
    import { ref, computed } from 'vue';
      const celsius = ref(0); // 摄氏温度
      const fahrenheit = computed({
        get: () => (celsius.value * 9) / 5 + 32, // 计算华氏温度
        set: (val) => {
          celsius.value = ((val - 32) * 5) / 9; // 反向计算摄氏温度
        },
      });
    </script>
    ```
    

## 生命周期

Vue 的声明周期是指 Vue 实例从创建到销毁的一系列过程，这期间会触发 Vue 提供的生命周期钩子函数，可以在特定阶段执行相应逻辑

![image.png|890x24](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/61c2a3f1-f805-4df4-a50d-e5a6d8e14189/image.png)

声明周期钩子函数需要在 setup 中使用

以 onMounted 钩子为例，表示在组件在完成初始渲染并创建 DOM 节点后执行函数

```html
<script setup>
	import { onMounted } from 'vue'

	onMounted(() => {
	  console.log(`the component is now mounted.`)
	})
</script>
```

|**阶段**|**钩子名**|**常见用途**|
|---|---|---|
|**创建阶段**|`beforeCreate`|初始化之前的操作，如打印日志（很少用）。|
||`created`|初始化完成后，如获取初始数据、设置全局事件。|
|**挂载阶段**|`beforeMount`|挂载前操作，比如查看模板编译内容。|
||`mounted`|DOM 挂载完成后，适合操作 DOM，进行异步请求。|
|**更新阶段**|`beforeUpdate`|数据更新前的逻辑处理，比如记录旧状态。|
||`updated`|数据更新后进行 DOM 操作或状态检查。|
|**销毁阶段**|`beforeDestroy`|实例销毁前，清理定时器或事件监听器。|
||`destroyed`|实例销毁后，完成清理工作。|

完整声明周期 API索引：[https://cn.vuejs.org/api/composition-api-lifecycle.html](https://cn.vuejs.org/api/composition-api-lifecycle.html)

## 侦听器

Vue 中 `watch()` 函数作为侦听器用于监控数据变化，并执行相应逻辑

watch() 函数具有三个参数：

- 需要监控的数据
- 数据变化时执行的回调函数
- 可选的 deep、immediate、once

# 组件

## 组件基础

Vue 中组件将 UI 拆分成可重用且独立的部分，和 HTML 类似，界面的呈现通过组件的嵌套展示

![image.png|890x24](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/a24a216a-c58f-47de-9dd8-3f17448607ca/image.png)

## 注册

组件只有被注册才能使用，表示组件的引入方式，有两种类型：全局注册和局部注册

### 全局注册

全局注册通过 Vue 实例的 component() 方法进行组件注册，可以在应用的任意组件中被使用

```jsx
import { createApp } from 'vue'
import App from './App.vue'
import MyComponentA from './App.vue'
import MyComponentB from './App.vue'
import MyComponentC from './App.vue'

const app = createApp(App)
app
  .component('ComponentA', ComponentA)
  .component('ComponentB', ComponentB)
  .component('ComponentC', ComponentC)
app.mount('#app')
```

```html
<!-- 这在当前应用的任意组件中都可用 -->
<ComponentA/>
<ComponentB/>
<ComponentC/>
```

### 局部注册

全局注册有两个问题：

- 被全局注册的组件，即使没有被使用，也会出现在打包后的 JS 文件中，不能被自动移除
- 全局注册的组件可以在任意组件中被使用，使得依赖关系不明确，不利于维护

局部注册是在父组件中引入子组件，在 setup 的情况下，引入的子组件可以在父组件中直接使用

```html
<script setup>
	import ComponentA from './ComponentA.vue'
</script>

<template>
  <ComponentA />
</template>
```

### 组件名格式

对于组件的命名可以使用大驼峰形式，这样可以区分原生的 HTML 元素，同时 IDE 提供了更好的自动补全

在模板中使用组件时，可以使用大驼峰形式，也可以使用小写短横线形式，Vue 会自动将模板中的小写短横线形式标签解析为大驼峰的组件

```html
<template>
  <MyComponent />
  <my-component />
</template>
```

## Props

props 用于父组件向子组件传递数据

![image.png|890x24](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/9bfab544-f3ff-4717-8bfd-750930d2d0ff/image.png)

### Props 声明

组件需要显式声明它所接受的 props ，这样 Vue 才会知道外部传入的那些是 props

props 的声明需要用到 `defineProps()`宏，在其中声明的 props 会自动暴露给模板，`defineProps()` 会返回一个对象，包含可以传递给组件的所有 props

```html
<script setup>
const props = defineProps(['title'])
</script>

<template>
  <h4>{{ title }}</h4>
  <h4>{{ props.title }}</h4>
</template>
```

`defineProps()` 的参数除了可以是字符串，还可以是对象，当以对象的形式声明每个属性时，key 是 Props 的名称，值是 Props 预期类型的构造函数

```jsx
// 使用 <script setup>
defineProps({
	id: Number,
  title: String
})
```

### 解构 Props 的响应式

在 3.5 及以上版本中，解构出的 `defineProps` 变量，Vue 编译器会自动在前面添加 `prop.`，使其具有响应式

```jsx
const { foo } = defineProps(['foo'])

watchEffect(() => {
  // 在 3.5 之前只运行一次
  // 在 3.5+ 中在 "foo" prop 变化时重新执行
  console.log(foo)
})
```

以上代码等同于以下代码

```jsx
const props = defineProps(['foo'])

watchEffect(() => {
  // `foo` 由编译器转换为 `props.foo`
  console.log(props.foo)
})
```

可以使用原生 JS 语法为 props 添加默认值

```jsx
const { foo = 'hello' } = defineProps<{ foo?: string }>()
```

在将 defineProps 解构出的变量传递给函数时，不能直接使用该 props，因为该 props 实际上是一个响应式数据源，需要包裹在 getter里面

```jsx
const { foo } = defineProps(['foo'])

watch(() => foo, /* ... */)
```

### 传递 Props 的细节

如果一个 props 名字很长，推荐在传递 props 时使用 kebab-case 形式，而在声明时，使用 camelCase形式

```jsx
defineProps({
  greetingMessage: String
})
----------------------------------------
<MyComponent greeting-message="hello" />
```

可以使用 `v-bind` 动态绑定 props，传入各种不同的类型，也可以使用不带参数的 `v-bind` 绑定多个参数

```jsx
<!-- 虽然 `42` 是个常量，我们还是需要使用 v-bind -->
<!-- 因为这是一个 JavaScript 表达式而不是一个字符串 -->
<BlogPost :likes="42" />

<!-- 根据一个变量的值动态传入 -->
<BlogPost :likes="post.likes" />
```

### 单向数据流

Props 遵循单向绑定原则，Props 因父组件的更新而变化，新的状态会自动流向子组件，而不会逆向传递

所以不应该在子组件中去修改 props 的值，如果需要在子组件中对 props 的值进行操作，可以使用计算属性，或者设置一个中间值

```jsx
const props = defineProps(['initialCounter'])

// 计数器只是将 props.initialCounter 作为初始值
// 像下面这样做就使 prop 和后续更新无关了
const counter = ref(props.initialCounter)
```

### Prop 校验

当 defineProps() 宏的参数是对象时，每个属性的 key 是 Props 的名称，值是 Props 预期类型的构造函数，通过这种方式实现 Props 的校验，当传入的值不满足类型要求时，Vue 会在控制台抛出警告

```jsx
defineProps({

  // 基础类型检查
  // （给出 `null` 和 `undefined` 值则会跳过任何类型检查）
  propA: Number,
  
  // 多种可能的类型
  propB: [String, Number],
  
  // 必传，且为 String 类型
  propC: {
    type: String,
    required: true
  },
  
  // 必传但可为 null 的字符串
  propD: {
    type: [String, null],
    required: true
  },
  
  // Number 类型的默认值
  propE: {
    type: Number,
    default: 100
  },
  
  // 对象类型的默认值
  propF: {
    type: Object,
    // 对象或数组的默认值
    // 必须从一个工厂函数返回。
    // 该函数接收组件所接收到的原始 prop 作为参数。
    default(rawProps) {
      return { message: 'hello' }
    }
  },
  
  // 自定义类型校验函数
  // 在 3.4+ 中完整的 props 作为第二个参数传入
  propG: {
    validator(value, props) {
      // The value must match one of these strings
      return ['success', 'warning', 'danger'].includes(value)
    }
  },
  
  // 函数类型的默认值
  propH: {
    type: Function,
    // 不像对象或数组的默认，这不是一个
    // 工厂函数。这会是一个用来作为默认值的函数
    default() {
      return 'Default function'
    }
  }
})
```

## 事件

## 插槽

插槽用于将父组件的模板内容传递给子组件并展示

![image.png|890x24](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/c9dc0bb5-c5ae-4d72-ba65-a53688e3ae21/image.png)

### 内容和出口

`<slot>` 元素是一个插槽出口，展示了父元素提供的插槽内容

子组件仅渲染插槽外层内容及样式，而插槽内容及样式由父组件提供

插槽内容不局限于字符串，还包括多个元素甚至是组件

```html
<FancyButton>
  <span style="color:red">Click me!</span>
  <AwesomeIcon name="plus" />
</FancyButton>
```

### 渲染作用域

在 Vue 中，父组件模板中的表达式只能访问父组件的作用域，子组件模板中的表达式只能访问子组件的作用域

插槽中的内容是在父组件中定义的，所以只能访问父组件作用域

```html
<span>{{ message }}</span>
<FancyButton>{{ message }}</FancyButton>
```

`{{ message }}` 渲染内容一致

### 默认内容

当外部没有提供任何内容时，可以在 `<slot>` 元素中提供默认内容，当外部提供内容时，外部内容将取代默认内容

```html
<button type="submit">
  <slot>
    Submit <!-- 默认内容 -->
  </slot>
</button>
```

### 具名插槽

当需要将多个模板内容传递给子组件时，就需要具名插槽，确定每个模板内容应该传递给那个插槽

![image.png|890x24](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/48f9f648-5acf-484a-af7e-e44d80524861/image.png)

`<slot>` 元素需要一个 `name` 属性，父组件传递内容时需要在 `<template>` 元素中的 `v-slot` 指令中传入插槽的 `name` 值，没有命名的 `<slot>` 元素会隐式命名为 `default`

`v-slot` 指令简写为 `#` ，可以使用动态指令参数 `<template #[dynamicSlotName]>`

- 子组件
    
    ```html
    <div class="container">
      <header>
        <slot name="header"></slot>
      </header>
      <main>
        <slot></slot>
      </main>
      <footer>
        <slot name="footer"></slot>
      </footer>
    </div>
    ```
    
- 父组件
    
    ```html
    <BaseLayout>
      <template #header>
        <h1>Here might be a page title</h1>
      </template>
    
      <!-- 隐式的默认插槽 -->
      <p>A paragraph for the main content.</p>
      <p>And another one.</p>
      <!-- 显式的默认插槽 -->
      <template #default>
        <p>A paragraph for the main content.</p>
        <p>And another one.</p>
      </template>
    
      <template #footer>
        <p>Here's some contact info</p>
      </template>
    </BaseLayout>
    ```
    
- 渲染结果
    
    ```html
    <div class="container">
      <header>
        <h1>Here might be a page title</h1>
      </header>
      <main>
        <p>A paragraph for the main content.</p>
        <p>And another one.</p>
      </main>
      <footer>
        <p>Here's some contact info</p>
      </footer>
    </div>
    ```
    

### 条件插槽

用于父组件传递给某个插槽内容时，该插槽才渲染，否则不渲染

结合 `v-if` 指令和 `$slots` 属性来实现，`$slots` 存储父组件 `v-slot` 指令的值

- 父组件
    
    ```html
      <Card>
        <template #header>
          <h1>This is the header</h1>
        </template>
    
        <template #footer>
          <em>This is the footer</em>
        </template>
      </Card>
    ```
    
- 子组件
    
    ```html
      <div class="card">
        <div v-if="$slots.header" class="card-header">
          <slot name="header" />
        </div>
        
        <div v-if="$slots.default" class="card-content">
          <slot />
        </div>
        
        <div v-if="$slots.footer" class="card-footer">
          <slot name="footer" />
        </div>
      </div>
    ```
    

### 作用域插槽

父组件提供的模板内容无法访问子组件作用域，如果需要子组件向父组件传递信息，可以通过作用域插槽，在子组件渲染时，将一部分数据传递给插槽

![image.png|890x24](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/c92e1fdf-f2ab-46f0-b4f0-8c419bb69f09/image.png)

子组件传入的 props 作为 v-slot 指令的值，可以在插槽模板内的表达式中使用，也可以在 v-slot 指令值中直接解构出来

如果使用具名插槽，对应子组件插槽的 props 会传递给对应的 v-slot 指令的值

- 默认插槽
    
    ```html
    <!-- <MyComponent> 的模板 -->
    <div>
      <slot :text="greetingMessage" :count="1"></slot>
    </div>
    ```
    
    ```html
    <MyComponent v-slot="slotProps">
      {{ slotProps.text }} {{ slotProps.count }}
    </MyComponent>
    ```
    
- 具名插槽
    
    ```html
    <!-- <MyComponent> template -->
    <div>
      <slot :message="hello"></slot>
      <slot name="footer" age="18"/>
    </div>
    ```
    
    ```html
    <MyComponent v-slot="{ message }">
      <p>{{ message }}</p>
      <template #footer="{ age }">
        <p>{{ age}}</p>
      </template>
    </MyComponent>
    ............................................
    <MyComponent>
      <!-- 使用显式的默认插槽 -->
      <template #default="props">
        <p>{{ props.message }}</p>
      </template>
    
      <template #footer="props">
        <p>{{ props.age }}</p>
      </template>
    </MyComponent>
    ```
    

# `<script setup>`

## defineExpose

`defineExpose` 是 Vue 3 中组合式 API 的一个新特性，允许子组件显式地暴露给父组件使用的方法和属性。通常情况下，父组件只能通过 `props` 或事件来与子组件进行交互，但是有时需要直接访问子组件的内部方法或数据，`defineExpose` 就是为了解决这一问题。

### 使用场景

- **父组件直接访问子组件实例**：当父组件需要调用子组件的一些方法或访问子组件的一些内部状态时，可以通过 `defineExpose` 来显式暴露。
- **封装组件的私有逻辑**：通过暴露必要的 API，而将其他部分的实现细节封装在子组件内部。

### 基本用法

### 1. 子组件暴露方法或属性

在子组件中，可以使用 `defineExpose` 来显式地暴露一些方法或属性给父组件使用。

### 例子：

假设你有一个 `Counter` 组件，父组件想通过 `ref` 调用子组件的 `increment` 方法来增加计数。

**子组件（Counter.vue）**：

```
vue
复制编辑
<template>
  <div>
    <p>{{ count }}</p>
    <button @click="increment">增加</button>
  </div>
</template>

<script setup>
import { ref, defineExpose } from 'vue'

const count = ref(0)

const increment = () => {
  count.value++
}

defineExpose({
  increment,  // 将 increment 方法暴露给父组件
})
</script>

```

**父组件（Parent.vue）**：

```
vue
复制编辑
<template>
  <div>
    <Counter ref="counterRef" />
    <button @click="handleIncrement">调用子组件方法</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Counter from './Counter.vue'

const counterRef = ref(null)

const handleIncrement = () => {
  counterRef.value.increment()  // 通过 ref 调用子组件暴露的方法
}
</script>

```

在上面的例子中，`Counter` 组件通过 `defineExpose` 暴露了 `increment` 方法，父组件通过 `ref` 引用 `Counter` 组件实例，然后调用暴露的方法。

### 2. 暴露响应式数据

你也可以暴露响应式数据，让父组件直接访问这些数据。

**子组件（Counter.vue）**：

```
vue
复制编辑
<template>
  <div>
    <p>{{ count }}</p>
  </div>
</template>

<script setup>
import { ref, defineExpose } from 'vue'

const count = ref(0)

defineExpose({
  count,  // 将 count 变量暴露给父组件
})
</script>

```

**父组件（Parent.vue）**：

```
vue
复制编辑
<template>
  <div>
    <Counter ref="counterRef" />
    <p>子组件计数：{{ counterRef?.count }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Counter from './Counter.vue'

const counterRef = ref(null)
</script>

```

这里，父组件可以直接访问子组件的 `count` 数据。

### 注意事项

1. **父组件访问的是暴露的内容**：`defineExpose` 暴露的属性或方法只能通过 `ref` 获取，且父组件无法访问子组件内未暴露的内容。
2. **暴露的属性是响应式的**：通过 `defineExpose` 暴露的响应式数据会保持响应式，因此父组件可以实时获取到子组件中数据的变化。
3. **暴露的内容仅限于当前组件的作用域**：`defineExpose` 仅限于子组件中使用，且暴露的是当前组件内的私有状态，不能暴露其他组件的数据。
4. **与 `ref` 配合使用**：通常与 `ref` 一起使用，因为 `defineExpose` 是暴露给父组件的，因此父组件通常需要通过 `ref` 来获取暴露的内容。

### 总结

`defineExpose` 为 Vue 3 提供了一个更简洁且灵活的方式来让子组件暴露内部方法或数据给父组件使用。它能够帮助开发者更好地封装组件的内部逻辑，并提供有限的接口供父组件调用，从而增强了代码的可维护性和复用性。


为什么Vue使用cosnt声明变量
## 🧱 一、JavaScript 中的 `const` 到底是“常量”吗？

### ✅ 关键点：

> `const` 声明的变量**不能被重新赋值（reassigned）**，但**并不意味着值不可变**。

### 🌰 示例1：原始类型

js

复制编辑

`const a = 10; a = 20; // ❌ TypeError: Assignment to constant variable`

### 🌰 示例2：引用类型

js

复制编辑

`const obj = { name: "Maktub" }; obj.name = "GPT"; // ✅ OK console.log(obj.name); // "GPT"  obj = {}; // ❌ TypeError`

🔍 解释：  
`const` 阻止的是**变量绑定的地址发生改变**，但对于引用类型，**地址内的数据是可以变的**！

---

## 🧬 二、Vue 中用 `const` 的核心原因

Vue 3（尤其是 Composition API）中我们常写：

js

复制编辑

`import { ref, reactive } from 'vue';  const count = ref(0); const user = reactive({ name: 'Maktub' });`

### ❓为啥不是 `let` 或 `var`？而是 `const`？

### ✅ 原因1：ref/ reactive 返回的是**响应式引用对象**

- `ref(0)` 返回一个对象：`{ value: 0 }`
    
- `reactive({...})` 返回一个 Proxy 对象
    
- **这些对象本身不会变，它们的属性在变**
    

js

复制编辑

`count.value = 10; // ✅ OK count = ref(20);  // ❌ 不允许（const）`

我们**不会也不应该重新赋值给 `count` 本身**，只是在改变其 `.value` 的内容。这和 `const` 的语义完全契合！

---

### ✅ 原因2：Composition API 推崇**函数式风格 + 不变引用**

Vue 3 设计理念中提倡：

- 变量声明后不再随意重新绑定（增强可读性、可维护性）
    
- 依赖自动追踪也需要变量绑定稳定
    

使用 `const`：

- 保证了响应式变量本身不被替换
    
- 可以减少“错误地重新赋值”的可能
    
- 能被类型推导清晰识别（配合 TypeScript 更明显）
    

---

### ✅ 原因3：防止响应式丢失

举个例子：

js

复制编辑

`let count = ref(0);  function resetCount() {     count = ref(0); // ⚠️ 问题大了！ }`

这样 `count` 的引用被换掉了，原先模板或其他 watch/computed 对 `count` 的绑定就失效了。

🧠 所以我们希望 `count` 的**引用地址保持不变** —— 用 `const` 是最好的选择。

---

## 📦 三、总结一句话

> 在 Vue 中，使用 `const` 声明响应式变量，是为了确保变量“引用不变、内容可变”，符合响应式设计原则、避免逻辑错误，也更接近函数式编程风格。

---

## 🧠 Bonus：那什么时候用 `let` 呢？

当你**确实需要替换变量绑定**时，比如在循环中或状态切换中使用：

js

复制编辑

`let currentTab;  if (route.path === '/home') {   currentTab = 'home'; } else {   currentTab = 'about'; }`

在这种“绑定可变”的场景中，`let` 就是合理的。

---

需要我结合 Vue 的模板渲染、组件响应式原理进一步深入分析 `ref` 和 `reactive` 吗，Maktub？也可以讲讲响应式失效的陷阱与 `const` 的保护机制。







# 概述

## 简介

- **Pinia** 是一款替代 **Vuex** 的状态管理（State Management）工具。
- 它提供了更强大的功能、更简洁的语法以及对 TypeScript 更好的语法支持。

## 需求

对于状态管理工具，主要应用于单页应用（SPA）或者大型应用，在没有使用状态管理工具时，有以下痛点：

- **状态共享问题：**随着应用越来越复杂，组件间（兄弟、父子、祖孙）的数据传递会变得非常困难，而且组件间的状态往往不止在一个页面，可能需要跨页面跨路由共享数据，像 Vue 使用 Props 和 Event 会变得不宜维护。
- **数据一致性问题：**当多个组件需要修改同一数据，如何保证数据的一致性和正确性是一个挑战。

状态管理工具解决了以下问题：

- **集中管理状态：**Vuex 和 Pinia 提供了一个中央存储（Store），集中管理应用的全局状态，所有需要共享的数据都可以放到 Store 中，组件只需要访问 Store 即可。
- **确保数据的一致性：**Vuex 和 Pinia 提供了统一的对数据修改的规则（Vuex 中的 mutations，Pinia 中的 actions），避免直接修改状态导致的不可预测的结果，保证了数据的一致性和正确性。
- **简化组件间的通信：**使用 Props 等手段时，随着组件层级的深入，数据传递变得困难且不易维护，使用 Vuex 和 Pinia 只需要访问 Store 即可获取和操作所需数据。
- **解耦和逻辑复用：**当将逻辑和状态提取到 Store 中，不同组件共享数据，可以实现更好的解耦和代码复用。

## 对比

Vuex 和 Pinia 都是 Vue 官方推荐的状态管理工具，在选择之前需要进行对比，选择更符合项目要求的工具。

- **状态管理：**
    - **Vuex：**使用 state 定义状态，getters 获取状态，mutations 修改状态且是同步操作，actions处理异步操作并且需要通过`commit` 调用 mutations 修改状态
    - **Pinia：**使用 state 定义状态，actions 修改状态且支持同步或异步操作
- **支持版本：**
    - **Vuex：**支持 Vue2 和 Vue3，使用选项式 API
    - **Pinia：**支持 Vue2 和 Vue3，可以使用选项式 API，更好的支持了组合式 API
- **store 定义方式：**
    - **Vuex：**通过 `createStore()` 创建 store
    - **Pinia：**使用 `defineStore()` 定义 store
- **模板化支持：**
    - **Vuex：**通过命名空间隔离各个模板
    - **Pinia：**默认一个 `defineStore()` 就是一个模板

总的来说，Vuex 适用于 Vue2，Pinia 适用于 Vue3

# 开始

## 安装

使用包管理器安装 Pinia：

```bash
yarn add pinia
# 或者使用 npm
npm install pinia
```

将 Pinia 实例（根 store）传递给应用：

```jsx
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

app.use(pinia)
app.mount('#app')
```

## 使用

在使用 Pinia 之前需要定义 store，可以使用选项式 API 或者组合式 API：

```jsx
export const useCounterStore = defineStore('counter', {
  state: () => ({ 
	  count: 0, 
	  name: 'Eduardo' 
	}),
  getters: {
    doubleCount: (state) => state.count * 2,
  },
  actions: {
    increment() {
      this.count++
    },
  },
})
```

```jsx
export const useCounterStore = defineStore('counter', () => {
  const count = ref(0)
  const doubleCount = computed(() => count.value * 2)
  function increment() {
    count.value++
  }

  return { count, doubleCount, increment }
})
```

定义好之后，在任意组件逻辑或者模板中可以使用 useCounterStore() 访问状态或者操作状态，actions 中支持异步操作。

# 概念

## Store

- Store 是通过 `defineStore()` 定义的，第一个参数是这个 store 的唯一 ID，第二个参数是一个 Setup 函数或者 Option 对象
    
- Setup 函数中的 ref()、computed()、function() 对应着 Option 对象中的 state、getters 和 actions
    
- Setup store 可以依赖于全局提供的属性，比如路由。任何**应用层面提供**的属性都可以在 store 中使用 `inject()` 访问，就像在组件中一样：
    
    ```jsx
    import { inject } from 'vue'
    import { useRoute } from 'vue-router'
    import { defineStore } from 'pinia'
    
    export const useSearchFilters = defineStore('search-filters', () => {
      const route = useRoute()
      // 这里假定 `app.provide('appProvided', 'value')` 已经调用过
      const appProvided = inject('appProvided')
    
      // ...
    
      return {
        // ...
      }
    })
    ```
    
- store 的名称推荐以 use 开头，以 store 结尾
    
- 不同的 store 应该定义在不同文件中
    
- 不能直接解构 state 和 getters，如果要解构可以使用 `storeToRefs()`，它会为每个响应式属性创建引用，保证响应性，function 可以直接解构。
    
    ```jsx
    <script setup>
    	import { storeToRefs } from 'pinia'
    	const store = useCounterStore()
    
    	const { name, doubleCount } = storeToRefs(store)
    	const { increment } = store
    </script>
    ```
    

## State

### 访问

可以使用 store 实例来对 state 进行读写：

```jsx
const store = useStore()

store.count++
```

### 重置

选项式 API 中，可以使用 `$reset()` 方法将 state 重置为初始值：

```jsx
const store = useStore()

store.$reset()
```

而在组合式 API 中，则需要自己手动实现方法

### 变更

除了直接修改 state 属性外，还可以使用 `$patch` 传入多个对象直接修改多个属性：

```jsx
store.$patch({
  count: store.count + 1,
  age: 120,
  name: 'DIO',
})
```

### 替换

Pinia 不能直接替换掉 store，可以使用 patch。

### 订阅

Pinia 可以通过 `$subscribe()` 侦听 state 的变化，这个方法只会在 patch 之后触发一次：

```jsx
cartStore.$subscribe((mutation, state) => {
  // import { MutationType } from 'pinia'
  mutation.type // 'direct' | 'patch object' | 'patch function'
  // 和 cartStore.$id 一样
  mutation.storeId // 'cart'
  // 只有 mutation.type === 'patch object'的情况下才可用
  mutation.payload // 传递给 cartStore.$patch() 的补丁对象。

  // 每当状态发生变化时，将整个 state 持久化到本地存储。
  localStorage.setItem('cart', JSON.stringify(state))
})
```

state subscript 会被绑定到定义的组件上，在组件被卸载时它们将会被删除，如果想在组件被卸载时保留它们，可以使用第二个参数 `{ detached: true }` ，将 state subscript 从组件中分离。

```jsx
<script setup>
	const someStore = useSomeStore()
	someStore.$subscribe(callback, { detached: true })
</script>
```

## Getter

## Actions

## 插件

# 配置

# 附录

<aside> 🌐

**官网地址：** [https://pinia.vuejs.org/zh/](https://pinia.vuejs.org/zh/)

</aside>

<aside> 💾

**代码地址：** [https://github.com/vuejs/pinia](https://github.com/vuejs/pinia)

</aside>

<aside> 📓

**其它文档：**

</aside>






![image.png](attachment:27ffd183-5528-41f4-9c9e-6a4044411e40:image.png)

# 概述

## 简介

- Vue Router 基于 Vue 的组件系统构建
- 用于在单页面应用（SPA）中管理不同页面（视图）之间的导航
- 定义了 URL 和视图组件之间的映射关系，使得切换页面时不需要从服务器重新加载

## 需求

`Vue Router` 主要解决了单页面应用（SPA）中的路由管理问题。在传统的多页面应用（MPA）中，每个页面都有一个独立的 URL 和完整的 HTML 页面，当用户点击链接时，浏览器会加载一个新的页面。而在单页面应用中，整个应用只由一个 HTML 页面构成，页面内容根据路由的不同动态地改变，而不需要重新加载整个页面。

## 对比

# 开始

## 安装

```bash
npm install vue-router@4
```

## 使用

### 创建路由实例

```jsx
import { createMemoryHistory, createRouter } from 'vue-router'

import HomeView from './HomeView.vue'
import AboutView from './AboutView.vue'

const routes = [
	// { path: '/', component: import('./HomeView.vue')}
  { path: '/', component: HomeView },
  { path: '/about', component: AboutView },
]

const router = createRouter({
  history: createMemoryHistory(),
  routes,
})
```

- routes 定义了一组路由，将 URL 映射到视图（被 RouterView 渲染的组件）
    - path：定义了 URL
    - component：定义了视图组件
- history 控制了路由和 URL 路径如何双向映射
- 一般将路由实例写在 router 目录下的 `index.js` 文件中

### 注册路由插件

```jsx
createApp(App)
  .use(router)
  .mount('#app')
```

- 在 `main.js` 中引入路由实例，使用 `use()` 方法完成注册
- 插件职责包括：
    - 全局注册 RouterView 和 RouterLink 组件
    - 添加全局的 `$router` 和 `$route` 属性（选项式 API 中使用）
    - 启用 `useRouter()` 和 `useRoute()` 组合式函数（this 不再指向 Vue 实例）
    - 触发路由解析器解析初始路由

### 访问路由器和当前路由

- 通用方法：直接将路由实例导入到需要的地方
- 选项式 API：使用 `this.$router` 和 `this.$route` 属性
- 组合式 API：使用 `useRouter()` 和 `useRoute()` 函数拿到各自的实例

# 概念

## 动态路由匹配

### 动态路由的基本概念

- 动态路由允许在路由路经中使用动态参数，使同一视图匹配不同 url ，并动态加载内容
- 动态参数通过 `:paramName` 的形式定义，称为路径参数
- /user/johnny、/user/jolyne、/user/123都会映射到同一视图

```jsx
const routes = [
  {
    path: '/user/:id',
    component: UserDetail,
    name: 'user-detail'
  }
];
```

- 可以在同一路由中设置多个路径参数，它们会映射到 `$route.params` 上的相应字段

|**匹配模式**|**匹配路径**|**route.params**|
|---|---|---|
|/users/:username|/users/eduardo|`{ username: 'eduardo' }`|
|/users/:username/posts/:postId|/users/eduardo/posts/123|`{ username: 'eduardo', postId: '123' }`|

### 访问路径参数

- 当路由匹配时，它的 params 的值将在每个组件中以 `route.params` 的形式暴露出来
- 假如上面的路由被匹配，则通过以下方式获取路径参数

```jsx
<template>
  <div>
    <!-- 当前路由可以通过 $route 在模板中访问 -->
    User {{ $route.params.id }}
  </div>
</template>
```

### 路径参数的写入

- 动态路径参数可以通过编程式导航（`router.push()` 或 `router.replace()`）来写入 URL
- 会根据路径配置自动填充 URL 的动态部分

```jsx
// 通过编程式导航跳转到用户详情页，并传递 id 参数
this.$router.push({ name: 'user-detail', params: { id: 123 } });
```

### 响应路由参数的变化

## 路由的匹配语法

## 嵌套路由

## 命名路由

- 创建路由时，可以选择给路由一个 name 属性 :1:
    
- 这个 name 属性可以代替 path 属性传递给路由方法或 RouterLink :2:
    
- 所有命名必须是唯一的，如果相同，则只会保留最后一条
    
- 给路由命名具有以下好处：
    
    - 没有硬编码的 URL。
    - `params` 的自动编码/解码。
    - 防止你在 URL 中出现打字错误。
    - 绕过路径排序，例如展示一个匹配相同路径但排序较低的路由。
- :1:
    
    ```jsx
    const routes = [
      {
        path: '/user/:username',
        name: 'profile', 
        component: User
      }
    ]
    ```
    
- :2:
    
    ```jsx
    <router-link :to="{ name: 'profile', params: { username: 'erina' } }">
      User profile
    </router-link>
    ```
    

## 编程式导航

- 编程式导航是不依赖于用户点击，通过 JS 代码控制路由跳转的方式
    
- 所有的导航方法都会返回一个 _Promise_，表示跳转是否成功
    
- **`router.push()`**
    
    - 点击`<router-link :to="...">` 时，内部会调用 `push()` 方法
    - 会向 History 栈中添加一个新纪录
    - 方法参数可以是一个字符串路径或是一个描述地址的对象
        - 代码示例
            
            ```jsx
            // 字符串路径
            router.push('/users/eduardo')
            
            // 带有路径的对象
            router.push({ path: '/users/eduardo' })
            
            // 命名的路由，并加上参数，让路由建立 url
            router.push({ name: 'user', params: { username: 'eduardo' } })
            
            // 带查询参数，结果是 /register?plan=private
            router.push({ path: '/register', query: { plan: 'private' } })
            
            // 带 hash，结果是 /about#team
            router.push({ path: '/about', hash: '#team' })
            ```
            
            如果提供了 `path`，`params` 会被忽略
            
            ```jsx
            const username = 'eduardo'
            // 我们可以手动建立 url，但我们必须自己处理编码
            router.push(`/user/${username}`) // -> /user/eduardo
            // 同样
            router.push({ path: `/user/${username}` }) // -> /user/eduardo
            // 如果可能的话，使用 `name` 和 `params` 从自动 URL 编码中获益
            router.push({ name: 'user', params: { username } }) // -> /user/eduardo
            // `params` 不能与 `path` 一起使用
            router.push({ path: '/user', params: { username } }) // -> /user
            ```
            
- **`router.replace()`**
    
    - 点击`<router-link :to="..." replace>` 时，内部会调用 `replace()` 方法
    - 作用与 `push()` 方法类似，但不会向 History 添加新纪录
    - 可以直接在`router.push` 的 `to` 参数中增加一个属性 `replace: true`
        - 代码示例
            
            ```jsx
            router.push({ path: '/home', replace: true })
            // 相当于
            router.replace({ path: '/home' })
            ```
            
- **`router.go()`**
    
    - 类似于 `window.history.go(n)`
    - 以整数作为参数，表示在 History 栈中前进或后退多少步
        - 代码示例
            
            ```jsx
            // 向前移动一条记录，与 router.forward() 相同
            router.go(1)
            
            // 返回一条记录，与 router.back() 相同
            router.go(-1)
            
            // 前进 3 条记录
            router.go(3)
            
            // 如果没有那么多记录，静默失败
            router.go(-100)
            router.go(100)
            ```
            

## 命名视图

## 重定向和别名

## 路由组件传参

## Route

## Router

## RouterLink

## RouterView

# 配置

## History

## 导航守卫

## 动态路由

# 附录

<aside> 🌐

**官网地址：** [https://router.vuejs.org/zh/](https://router.vuejs.org/zh/)

</aside>

<aside> 💾

**代码地址：** [https://github.com/vuejs/router](https://github.com/vuejs/router)

</aside>

<aside> 📓

**其它文档：**

</aside>

在前端开发中，**Hash 模式**（Hash Mode）是用于管理和控制浏览器历史记录及 URL 的一种路由模式，通常用于单页应用程序（SPA，Single Page Application）中。

### Hash 模式的工作原理

Hash 模式使用 URL 中的 `#` 符号（即 hash 标志），来表示路由的变化。例如：

```bash
bash
Copy code
<http://example.com/#/home>

```

在这个 URL 中，`#` 后面的部分（`/home`）就是 hash 部分。浏览器不会将 `#` 后面的内容发送到服务器，它只会在客户端处理。通过监听 hash 值的变化，前端应用可以动态地渲染不同的页面内容，而不需要真正的页面刷新。

### 核心特点

1. **浏览器不发送 hash 到服务器**：`#` 后的部分只在客户端解析，因此请求只发送到根路径。服务器不需要做任何处理，所有路由操作都在前端进行。
2. **基于 `hashchange` 事件**：当用户点击浏览器的前进、后退按钮或手动修改 URL 中的 `#` 后的内容时，浏览器会触发 `hashchange` 事件。通过监听这个事件，应用可以根据新的 hash 值动态更新页面内容。
3. **兼容性好**：Hash 模式可以在不支持 HTML5 History API 的旧版本浏览器中使用，因此其兼容性非常好。

### Hash 模式的优缺点

### 优点

- **易于实现**：不需要后端服务器的特殊配置，因为所有内容都在客户端处理，后端只需要处理一个入口文件（如 `index.html`）。
- **兼容性好**：与几乎所有浏览器兼容，包括一些不支持 HTML5 History API 的旧版浏览器。

### 缺点

- **URL 不美观**：由于 `#` 的存在，URL 看起来不如使用 HTML5 History API 那么干净，例如 `http://example.com/#/home` 比 `http://example.com/home` 更显冗余。
- **SEO 不友好**：虽然现代 SPA 应用程序可以通过服务端渲染和预渲染来改善 SEO，但 Hash 模式天然对 SEO 不友好，因为搜索引擎往往不会抓取 `#` 后的部分内容。

### Hash 模式 vs History 模式

Hash 模式与基于 HTML5 History API 的 **History 模式** 是两种常见的前端路由实现方式。两者的主要区别是：

- **Hash 模式**：使用 `#` 后的内容来表示路由状态，完全在客户端处理，不涉及服务器。
- **History 模式**：利用 HTML5 提供的 `pushState` 和 `replaceState` API 操作 URL，直接在不带 `#` 的 URL 中进行路由操作，如 `http://example.com/home`。

在现代前端开发中，**Hash 模式** 更适合兼容性要求较高、后端配置较少的项目，而 **History 模式** 则适用于更干净、直观的 URL 结构，并且需要服务器端的支持和配置。

### 适用场景

- **单页应用程序**：Hash 模式常用于 SPA 项目，在页面不刷新和请求服务器的情况下，通过前端 JavaScript 路由来切换页面。
- **需要兼容旧版浏览器的应用**：由于 Hash 模式依赖的是早期浏览器普遍支持的机制（`hashchange` 事件），它在一些不支持 HTML5 History API 的旧版浏览器中仍然可以使用。

### 如何使用

在常见的 JavaScript 前端框架中，如 Vue.js、React.js 等，Hash 模式通常是通过内置的路由库实现的。例如，在 Vue Router 中，可以通过以下方式启用 Hash 模式：

```jsx
javascript
Copy code
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  mode: 'hash', // 使用 hash 模式
  routes: [
    {
      path: '/home',
      component: Home
    },
    {
      path: '/about',
      component: About
    }
  ]
})

```

通过配置 `mode: 'hash'`，即可启用 Hash 模式，URL 将以 `#` 的形式表现。

### 总结

**Hash 模式** 是一种简单的客户端路由方式，适合单页应用中不需要后端服务器配置的场景。它提供了较好的兼容性，但在 URL 美观性和 SEO 友好性方面有所欠缺。

**History 模式** 是一种现代的前端路由模式，基于 HTML5 的 `History API`，通常用于单页应用程序（SPA，Single Page Application）中。与传统的 Hash 模式不同，History 模式通过使用干净的 URL（不带 `#` 符号）来管理浏览器的历史记录，并允许前端在不刷新页面的情况下更新 URL。

### History 模式的工作原理

HTML5 的 `History API` 提供了 `pushState()` 和 `replaceState()` 方法，允许前端开发者在不重新加载页面的情况下改变浏览器的 URL，保持用户体验的一致性。以下是一些关键点：

1. **干净的 URL**：History 模式不需要 `#` 符号，直接使用标准的路径，如 `http://example.com/home`。与 Hash 模式的 `http://example.com/#/home` 不同，History 模式下的 URL 看起来更简洁、优雅。
2. **操作浏览器的历史记录**：通过 `pushState()` 和 `replaceState()`，可以在不刷新页面的前提下，修改地址栏的 URL，并且这些操作会被记录到浏览器的历史记录中，因此用户可以使用浏览器的“前进”、“后退”按钮来导航。
3. **与服务器交互**：由于 History 模式直接操作 URL，当用户手动输入 URL 或刷新页面时，浏览器会向服务器发送 HTTP 请求。为了避免 404 错误，服务器需要配置将所有的请求都重定向到应用的入口文件（通常是 `index.html`），让前端路由进行处理。

### History 模式的优缺点

### 优点

1. **干净的 URL**：与 Hash 模式相比，History 模式使用标准的路径，URL 更简洁且直观。对于用户和搜索引擎来说，这种 URL 更友好。
2. **SEO 友好**：因为 URL 是标准的路径格式，搜索引擎可以更好地抓取和索引页面内容，因此使用 History 模式的应用可以更好地进行 SEO 优化。
3. **浏览体验一致**：用户可以通过浏览器的前进、后退按钮自由导航，不会出现页面刷新，保持了单页应用的流畅体验。

### 缺点

1. **需要服务器端支持**：当用户直接输入 URL 或刷新页面时，浏览器会向服务器发送请求，因此服务器需要正确配置路由规则，将所有请求都重定向到应用的入口文件。如果没有正确配置服务器，用户可能会遇到 404 错误。
2. **不兼容旧版浏览器**：History 模式依赖于 HTML5 的 `pushState` 和 `replaceState` API，旧版本浏览器（如 IE9 以下版本）不支持这些特性，需要提供降级方案或者使用 polyfill。

### 与 Hash 模式的比较

|特性|Hash 模式|History 模式|
|---|---|---|
|**URL 结构**|`http://example.com/#/home`|`http://example.com/home`|
|**SEO**|不太友好，搜索引擎通常不会抓取 `#` 后的内容|SEO 友好，标准路径，易于索引|
|**页面刷新**|页面刷新不会发送请求到服务器|页面刷新会向服务器发送请求，需要配置|
|**兼容性**|兼容旧版浏览器|依赖 HTML5 API，不兼容较旧的浏览器|
|**实现难度**|不需要服务器支持，简单易用|需要服务器配置，复杂度较高|

### 如何在前端框架中使用 History 模式

### Vue.js 中的 History 模式

在 Vue.js 中，可以通过 Vue Router 使用 History 模式。只需在路由器配置中将 `mode` 设置为 `'history'` 即可启用 History 模式：

```jsx
javascript
Copy code
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  mode: 'history', // 启用 history 模式
  routes: [
    {
      path: '/home',
      component: Home
    },
    {
      path: '/about',
      component: About
    }
  ]
})

```

### React.js 中的 History 模式

在 React 中，如果使用 React Router，可以使用 `BrowserRouter` 来实现 History 模式。

```jsx
javascript
Copy code
import React from 'react'
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom'

function App() {
  return (
    <Router>
      <Switch>
        <Route path="/home" component={Home} />
        <Route path="/about" component={About} />
      </Switch>
    </Router>
  )
}

```

### 服务器端配置

因为 History 模式会直接操作 URL，如果用户刷新页面或直接访问某个子页面，服务器会尝试处理该 URL 路径。因此，服务器需要重定向所有请求到单页应用的入口文件（如 `index.html`）。

**Nginx 配置示例：**

```
nginx
Copy code
server {
  listen 80;
  server_name example.com;

  location / {
    try_files $uri $uri/ /index.html;
  }
}

```

**Apache 配置示例（`.htaccess` 文件）：**

```
apache
Copy code
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>

```

### 总结

**History 模式** 是一种基于 HTML5 `History API` 的路由模式，提供了干净的 URL 和更好的 SEO 支持，适合现代单页应用。但它需要后端服务器的支持来处理页面刷新和直接访问。相比 Hash 模式，它能提供更直观的用户体验和 URL 结构，但需要对服务器端进行额外配置。




# Vueuse