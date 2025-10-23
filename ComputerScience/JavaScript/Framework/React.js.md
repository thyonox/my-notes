
![[React.js.png|1190x612]]
# 概述
## 简介
- React 是一个由 Facebook 开发的开源 JavaScript 库，用于构建高效、可复用的用户界面。
- 核心特点：
	- **组件化开发**：将 UI 拆分为独立、可复用的组件，便于开发和维护。
	- **声明式编程**：通过 JSX 描述 UI 结构，代码直观且易于理解。
	- **虚拟 DOM**：使用虚拟 DOM 优化渲染，减少直接操作真实 DOM 的开销。
	- **单向数据流**：数据从父组件流向子组件，确保状态可预测和管理。
	- **Hooks**：函数组件通过 Hooks（如 useState、useEffect）实现状态管理和副作用处理。
	- **强大的生态系统**：支持 React Router、Redux、Next.js 等工具，适用于复杂应用。
	- **跨平台支持**：通过 React Native 扩展到移动端开发。
	- **社区活跃**：拥有庞大的社区和丰富的学习资源，更新迭代迅速。
## 需求
- **React 之前**：
	- 纯 HTML + jQuery（或类似库）
		- 页面是由 HTML 静态文件写出来的。
		- 当用户操作时，用 jQuery 或原生 JS 操作 DOM（增删改查）。
		- 需要操作 DOM 来更新内容、绑定事件、处理交互。
		- 手动操作 DOM 繁琐且易错。
		- 当页面越来越复杂，状态越来越多，UI 和状态很难同步。
	- 模板渲染模式（如 AngularJS、Backbone）
		- 出现了**双向绑定**或模板渲染（MVVM、MVC）。
		- 比如 AngularJS 可以自动把数据绑定到 DOM 上。
		- 双向绑定虽然方便，但在大型项目里很容易引发复杂的依赖、难以跟踪数据流向。
		- 组件化思想不够成熟，复用和组合能力较弱。
- **React 之后**：
	- 高效更新 UI。
		- 问题：
			- 原生 DOM 操作开销大，手动维护 DOM 难度大，容易出错。
			- 页面状态变动时，哪个元素该更新、哪个不该更新，容易混乱。
		- 解决：
			- React 引入 **Virtual DOM**（虚拟 DOM），把真实 DOM 抽象成一个 JS 对象树。
			- 当状态改变时，先在内存里构建新的 Virtual DOM，再和旧的 Virtual DOM 做对比（Diff 算法）。
			- 最后只把需要更新的部分同步到真实 DOM，避免了不必要的重绘和回流，性能显著提升。
	- 组件化和单向数据流。
		- 问题：
			- 复杂页面里，HTML、JS、CSS 往往混在一起，难以复用和维护。
			- 状态和 UI 紧耦合，重复逻辑多，开发体验差。
		- 解决：
			- 提出 **组件化** 思想，一个页面就是由组件树组成的，每个组件是**自包含**的，拥有自己的状态和 UI。
			- 父组件通过 `props` 向下传递数据，子组件通过事件把行为传回去（单向数据流）。
## 对比

|                |                                         |                                          |                                               |                                         |
| -------------- | --------------------------------------- | ---------------------------------------- | --------------------------------------------- | --------------------------------------- |
| **特性/框架**      | **React**                               | **Vue.js**                               | **Angular**                                   | **Svelte**                              |
| **核心理念**       | 组件化，虚拟 DOM，JSX，单向数据流                    | 响应式数据绑定，虚拟 DOM，模板语法，渐进式框架                | 完整框架，TypeScript，双向数据绑定，依赖注入                   | 编译时框架，无虚拟 DOM，响应式语法                     |
| **学习曲线**       | 适中，需掌握 JSX 和 Hooks                      | 平缓，模板语法对初学者友好                            | 陡峭，需掌握 TypeScript 和 Angular 概念                | 简单，语法接近原生 JavaScript                    |
| **性能**         | 虚拟 DOM，性能良好，适合复杂 UI                     | 虚拟 DOM，性能接近 React                        | 性能稍逊（框架较重），适合大型应用                             | 编译时生成原生 JS，性能极高                         |
| **生态系统**       | 丰富（React Router、Redux、Next.js 等），社区最大   | 较丰富（Vue Router、Pinia），社区活跃               | 丰富（Angular CLI、RxJS），企业级支持强                   | 较新（SvelteKit），生态正在发展                    |
| **状态管理**       | Context、Redux、Zustand、React Query       | Pinia、Vuex，内置响应式系统                       | 内置 NgRx、RxJS，依赖注入                             | 内置响应式，无需额外库                             |
| **路由**         | React Router（需单独引入）                     | Vue Router（官方支持）                         | Angular Router（内置）                            | SvelteKit（官方支持）                         |
| **体积**         | 中等（核心库 ~30KB，需额外库）                      | 轻量（~20KB）                                | 较重（~100KB+）                                   | 极轻（~1-3KB，无运行时）                         |
| **SSR/SSG 支持** | 支持（Next.js）                             | 支持（Nuxt.js）                              | 支持（Angular Universal）                         | 支持（SvelteKit）                           |
| **跨平台**        | 支持（React Native for 移动端）                | 有限（Weex 或第三方）                            | 有限（Ionic 或 NativeScript）                      | 有限（SvelteKit 实验性支持）                     |
| **优点**         | - 灵活性高，生态丰富  <br>- 跨平台支持  <br>- 社区庞大    | - 学习简单，开箱即用  <br>- 轻量高效  <br>- 模板语法直wab观 | - 完整框架，规范性强  <br>- 强类型支持  <br>- 企业级开发友好       | - 性能极高，代码简洁  <br>- 无运行时开销  <br>- 学习成本低  |
| **缺点**         | - 需额外库（如路由、状态管理）  <br>- JSX 有学习成本       | - 生态规模小于 React  <br>- 大型项目灵活性稍逊          | - 学习曲线陡峭  <br>- 框架较重，开发速度慢                    | - 生态较新，工具支持有限  <br>- 企业采用率低             |
| **适用场景**       | - 单页应用（SPA）  <br>- 复杂交互 UI  <br>- 跨平台项目 | - 快速原型开发  <br>- 中小型项目  <br>- 初学者团队       | - 企业级大型应用  <br>- 需要强规范项目  <br>- TypeScript 团队 | - 高性能需求项目  <br>- 轻量化应用  <br>- 移动端或嵌入式设备 |
| **社区与支持**      | 最大社区，文档丰富，活跃度高                          | 社区活跃，多语言文档支持                             | 企业支持强，文档规范                                    | 社区较小但增长迅速，文档完善                          |

# 开始
## 安装
- **使用 Create React App (CRA)搭建**
	- 全局安装 Create React App
		- 运行命令：
			```bash
			npm install -g create-react-app
			```
		- 或者使用 npx（推荐，避免全局安装）：
			```bash
			npx create-react-app my-app
			```
	- 创建项目
		- 运行以下命令生成项目（`my-app` 为项目名称）：
			```bash
			npx create-react-app my-app
			```
		- 项目结构包括 `src/`（源代码）、`public/`（静态资源）和默认配置文件。
	- 启动开发服务器
		- 进入项目目录并启动：
			```bash
			cd my-app
			npm start
			```
		- 默认在 `http://localhost:3000` 打开浏览器，显示 React 默认页面。
	- 构建生产版本
		- 运行以下命令生成优化后的静态文件：
			```bash
			npm run build
			```
		- 输出在 `build/` 目录，可部署到服务器（如 Vercel、Netlify）。
- **使用 Vite 搭建**
	- 创建 Vite 项目
		- 运行以下命令，选择 React 模板：
		- 或使用 yarn：
	- 安装依赖并启动
		- 进入项目目录，安装依赖并启动开发服务器：
			```bash
			cd my-app
			npm install
			npm run dev
			```
		- 默认在 `http://localhost:5173` 打开浏览器。
	- 构建生产版本
		- 运行以下命令：
			```bash
			npm run build
			```
		- 输出在 `dist/` 目录，适合部署。
- **使用 Next.js 搭建**
	- 创建 Next.js 项目
		- 运行以下命令：
			```bash
			npx create-next-app@latest my-app
			```
		- 按提示选择配置（如是否使用 TypeScript、ESLint、Tailwind CSS）。
	- 启动开发服务器
		- 进入项目目录并启动：
			```bash
			cd my-app
			npm run dev
			```
		- 默认在 `http://localhost:3000` 打开。
	- 构建与部署
		- 构建生产版本：
			```bash
			npm run build
			```
		- 启动生产服务器：
			```bash
			npm run start
			```
- **手动搭建 React 项目**
	- 初始化项目
		- 创建目录并初始化 npm 项目：
			```bash
			mkdir my-app
			cd my-app
			npm init -y
			```
	- 安装 React 和核心依赖
		- 安装 React 和 ReactDOM：
			```bash
			npm install react react-dom
			```
	- 配置 Webpack 和 Babel
		- 安装 Webpack、Babel 和相关插件：
			```bash
			npm install --save-dev webpack webpack-cli webpack-dev-server @babel/core @babel/preset-env @babel/preset-react babel-loader
			```
		- 创建 `babel.config.json`：
			```json
			{
			  "presets": ["@babel/preset-env", "@babel/preset-react"]
			}
			```
		- 创建 `webpack.config.js`：
			```js
			const path = require("path");
			module.exports = {
			  entry: "./src/index.js",
			  output: {
			    path: path.resolve(__dirname, "dist"),
			    filename: "bundle.js",
			  },
			  module: {
			    rules: [
			      {
			        test: /\.jsx?$/,
			        exclude: /node_modules/,
			        use: "babel-loader",
			      },
			    ],
			  },
			  devServer: {
			    contentBase: path.join(__dirname, "dist"),
			    port: 3000,
			  },
			};
			```
	- 创建入口文件
		- 创建 `src/index.js`：
			```jsx
			import React from "react";
			import ReactDOM from "react-dom";
			ReactDOM.render(<h1>Hello, React!</h1>, document.getElementById("root"));
			```
		- 创建 `public/index.html`：
			```html
			<!DOCTYPE html>
			<html>
			  <body>
				<div id="root"></div>
				<script src="../dist/bundle.js"></script>
			  </body>
			</html>
			```
	- 启动开发服务器
		- 添加 npm 脚本到 package.json：
			```bash
			"scripts": {
			  "start": "webpack-dev-server --mode development",
			  "build": "webpack --mode production"
			}
			```
		- 运行：
			```bash
			npm start
			```
## 使用
```text
my-vite-app/
├── node_modules/              # 存放项目依赖，自动生成
├── public/                    # 静态资源，直接复制到构建输出根路径
│   ├── favicon.ico            # 网站图标
│   └── images/                # 静态图片资源，例：/images/logo.png 可直接访问
├── src/                       # 源代码目录，核心开发区域
│   ├── assets/                # 静态资源（图片、字体等），构建时优化处理
│   │   ├── logo.png           # 示例图片，供组件引用
│   │   └── fonts/             # 自定义字体文件
│   ├── components/            # 可复用 UI 组件，封装展示逻辑
│   │   ├── common/            # 通用的基础组件
│   │   │   ├── Button.jsx     # 按钮组件，封装样式和交互
│   │   │   └── Button.css     # 按钮组件样式（或使用 CSS 模块：Button.module.css）
│   │   ├── layout/            # 布局组件，如导航栏、页脚
│   │   │   ├── Navbar.jsx     # 导航栏组件
│   │   │   └── Footer.jsx     # 页脚组件
│   ├── pages/                 # 页面级组件，通常对应路由
│   │   ├── Home.jsx           # 首页组件
│   │   ├── About.jsx          # 关于页面组件
│   │   └── Blog.jsx           # 博客页面组件
│   ├── hooks/                 # 自定义 Hooks，封装可复用逻辑
│   │   ├── useFetch.js        # 数据获取 Hook，处理 API 请求
│   │   └── useAuth.js         # 认证相关 Hook
│   ├── api/                   # API 请求逻辑，封装网络调用
│   │   ├── fetchData.js       # 数据获取函数，例：调用 REST API
│   │   └── auth.js            # 认证相关 API 调用
│   ├── utils/                 # 工具函数，通用逻辑
│   │   ├── formatDate.js      # 格式化日期函数
│   │   └── constants.js       # 常量定义，如 API 地址
│   ├── styles/                # 全局样式或 CSS 模块
│   │   ├── global.css         # 全局样式，影响整个应用
│   │   └── Home.module.css    # 页面专属 CSS 模块，避免样式冲突
│   ├── context/               # 全局状态管理，基于 Context API
│   │   └── ThemeContext.js    # 主题上下文，管理全局主题状态
│   ├── tests/                 # 测试文件，单元测试或集成测试
│   │   ├── Button.test.js     # 按钮组件测试
│   │   └── useFetch.test.js   # 自定义 Hook 测试
│   ├── App.jsx                # 根组件，定义应用结构
│   ├── App.css                # 根组件样式
│   ├── main.jsx               # 应用入口，渲染根组件
│   └── index.css              # 全局样式
├── .gitignore                 # Git 忽略文件，排除 node_modules、dist 等
├── index.html                 # HTML 入口，定义挂载点
├── package.json               # 项目元信息，定义依赖和脚本
├── vite.config.js             # Vite 配置，定制插件、代理等
└── README.md                  # 项目说明文档
```
# 概念
## JSX
### JSX基础
- **定义与本质**
	- JSX 是 JavaScript 的语法糖，允许在 JS 代码中编写类似 HTML 的标记。
	- JSX 最终被编译为 `React.createElement` 调用，生成虚拟 DOM 对象。
		```jsx
		// JSX
		const element = <h1>Hello, world!</h1>;
		// 编译后
		const element = React.createElement("h1", null, "Hello, world!");
		```
- **使用前提**
	- 需要引入 React（即使不直接使用 React 变量，JSX 编译依赖 React 作用域）。
	- 在 React 17+ 中，新的 JSX Transform 自动注入依赖，无需手动导入 React。
		```jsx
		// React 17+，无需 import React
		const element = <div>Hello</div>;
		```
- **基本规则**
	- 单一根元素：每个 JSX 表达式必须有一个根节点。
		```jsx
		// 正确
		<div><h1>Title</h1><p>Text</p></div>
		// 错误：多个根节点
		<h1>Title</h1><p>Text</p>
		// 解决：使用 Fragment
		<><h1>Title</h1><p>Text</p></>
		```
	- 闭合标签：所有标签必须闭合，包括自闭合标签（如 `<img />` )。
	- 属性命名：使用 camelCase（如 `className`、`onClick`），而非 HTML 的 `class` 或 `onclick`。
	- 嵌入表达式：在 `{}` 中嵌入 JavaScript 表达式。
		```jsx
		const name = "Alice";
		const element = <h1>Hello, {name}!</h1>;
		```
### JSX中的JavaScript表达式
- **动态值**
	- 在 JSX 中，`{}` 可包含任何 JavaScript 表达式（返回值的代码）。
		```jsx
		const count = 5;
		const element = <p>Count: {count * 2}</p>; // 输出 Count: 10
		```
- **支持的表达式**
	- 变量、运算、函数调用、条件表达式等。
		```jsx
		const user = { name: "Bob" };
		const greeting = () => "Hi!";
		<div>
		  {user.name} {greeting()} {1 + 2} {true ? "Yes" : "No"}
		</div>
		```
- **不支持的语句**
	- `if`、`for`、`switch` 等语句不能直接用在 `{}` 中，需用三元运算符或逻辑运算符代替。
		```jsx
		// 错误
		<div>{if (true) { return "Hi"; }}</div>
		// 正确
		<div>{true ? "Hi" : "Bye"}</div>
		```
### JSX属性
- **静态属性**
	- 使用 camelCase 命名属性。
		```jsx
		<input type="text" className="input" disabled />
		```
- **动态属性**
	- 属性值可以用 `{}` 动态绑定。
		```jsx
		const id = "my-input";
		<input id={id} value={user.name} />
		```
- **拓展属性**
	- 使用展开运算符（`...`）传递多个属性。
		```jsx
		const props = { type: "text", className: "input" };
		<input {...props} />
		```
- **特殊属性**
	- `className`：替代 HTML 的 `class`。
	- `htmlFor`：替代 HTML 的 for（用于 `<label>`）。
	- `style`：接受对象，而非字符串。
		```jsx
		const styles = { color: "blue", fontSize: "16px" };
		<div style={styles}>Styled text</div>
		```
### JSX组件
- **组件作为标签**
	- JSX 区分 HTML 标签和 React 组件：首字母小写为 HTML 标签，首字母大写为组件。
		```jsx
		// HTML 标签
		<div>Native</div>
		// React 组件
		function MyComponent() {
		  return <span>Custom</span>;
		}
		<MyComponent />
		```
- **Props 传递**
	- 组件通过 Props 接收数据。
		```jsx
		function Welcome({ name }) {
		  return <h1>Hello, {name}!</h1>;
		}
		<Welcome name="Alice" />
		```
- **Children**
	- 组件可以通过 `props.children` 接收嵌套内容。
		```jsx
		function Card({ children }) {
		  return <div className="card">{children}</div>;
		}
		<Card><p>Content</p></Card>
		```
### 条件渲染
- **三元运算符**
	- 用于简单的条件渲染。
		```jsx
		const isLoggedIn = true;
		<div>{isLoggedIn ? <User /> : <Guest />}</div>
		```
- **逻辑运算符**
	- 使用 `&&` 或 `||` 控制渲染。
		```jsx
		{items.length > 0 && <p>{items.length} items</p>}
		```
- **提前返回**
	- 在函数组件中直接返回不同 JSX。
		```jsx
		function Component({ isLoading }) {
		  if (isLoading) return <p>Loading...</p>;
		  return <p>Data loaded!</p>;
		}
		```
### 列表渲染
- 使用 map
	- 遍历数组生成 JSX 元素，需为每个元素指定唯一 `key`。
		```jsx
		const items = [{ id: 1, name: "Apple" }, { id: 2, name: "Banana" }];
		<ul>
		  {items.map(item => <li key={item.id}>{item.name}</li>)}
		</ul>
		```
- 注意事项
	- 避免使用数组索引作为 `key`，除非数据稳定且无排序变化。
	- 动态列表需确保 `key` 唯一且稳定。
### Fragment
- 作用
	- 避免多余的 DOM 节点，包裹多个元素。
		```jsx
		import { Fragment } from "react";
		function List() {
		  return (
		    <Fragment>
		      <h1>Title</h1>
		      <p>Text</p>
		    </Fragment>
		  );
		}
		// 简写
		<> <h1>Title</h1> <p>Text</p> </>
		```
- 场景
	- 返回多个元素但不想增加额外 `<div>`。
	- 列表渲染中避免多余包裹节点。
### JSX安全性


## 组件化开发
### 组件类型
#### 函数组件
- **定义**：使用 JavaScript 函数定义组件，返回 JSX。
- **特点**：
	- 无 this 绑定问题。
	- 配合 Hooks（如 useState、useEffect）实现状态管理和副作用。
	- 代码更简洁，易于测试。
- **示例**：
	```js
	function Welcome(props) {
	  return <h1>Hello, {props.name}!</h1>;
	}
	```
- **使用场景**：展示型组件、简单逻辑组件、需要 Hooks 的场景。
#### 类组件
- **定义**：使用 ES6 类语法，继承 `React.Component`。
- **特点**：
	- 提供 `state` 和 `setState` 管理状态。
	- 包含生命周期方法（如 `componentDidMount`、`componentDidUpdate`）。
	- 需要处理 `this` 绑定。
- **示例**：
	```js
	class Welcome extends React.Component {
	  constructor(props) {
	    super(props);
	    this.state = { name: props.name };
	  }
	
	  render() {
	    return <h1>Hello, {this.state.name}!</h1>;
	  }
	}
	```
- **使用场景**：复杂状态管理、需要生命周期方法、与旧代码兼容。
### Props 与组件通信
#### Props 传递与接收
- **传递**：父组件通过属性向子组件传递数据。
- **接收**：子组件通过 props 参数访问数据。
- **示例**：
	```js
	function Parent() {
	  return <Child name="Alice" age={25} />;
	}
	
	function Child(props) {
	  return <p>{props.name} is {props.age} years old.</p>;
	}
	```
#### 默认 Props
- 为组件设置默认属性，防止未传值时的错误。
	```js
	function Child({ name = "Guest" }) {
	  return <p>Hello, {name}!</p>;
	}
	```
#### Props 类型检查
- 使用 PropTypes 或 TypeScript 确保 Props 类型正确。
- **PropTypes 示例**：
	```js
	import PropTypes from 'prop-types';
	
	function Child(props) {
	  return <p>{props.name}</p>;
	}
	
	Child.propTypes = {
	  name: PropTypes.string.isRequired,
	  age: PropTypes.number,
	};
	```
- **TypeScript 示例**：
	```js
	interface ChildProps {
	  name: string;
	  age?: number;
	}
	
	function Child({ name, age }: ChildProps) {
	  return <p>{name} {age && `is ${age}`}</p>;
	}
	```
#### 单向数据流
- 数据从父组件流向子组件，子组件通过回调函数通知父组件更新。
- **示例**：
	```js
	function Parent() {
	  const [count, setCount] = useState(0);
	
	  return <Child count={count} onIncrement={() => setCount(count + 1)} />;
	}
	
	function Child({ count, onIncrement }) {
	  return <button onClick={onIncrement}>Count: {count}</button>;
	}
	```
### State 管理
#### useState（函数组件）
- **定义与更新**：
	```js
	function Counter() {
	  const [count, setCount] = useState(0);
	
	  return (
	    <div>
	      <p>Count: {count}</p>
	      <button onClick={() => setCount(count + 1)}>Increment</button>
	    </div>
	  );
	}
	```
- **注意事项**：
	- 状态更新是异步的，可能需要使用函数式更新：`setCount(prev => prev + 1)`。
	- 不要直接修改状态，始终使用 `setState`。
#### setState（类组件）
- **定义与更新**：
	```js
	class Counter extends React.Component {
	  state = { count: 0 };
	
	  increment = () => {
	    this.setState(prevState => ({ count: prevState.count + 1 }));
	  };
	
	  render() {
	    return (
	      <div>
	        <p>Count: {this.state.count}</p>
	        <button onClick={this.increment}>Increment</button>
	      </div>
	    );
	  }
	}
	```
- **注意事项**：
	- setState 是异步的，可能批量处理更新。
	- 合并更新特性：多次调用 setState 会合并为一次更新。
#### 状态提升
- 当多个组件需要共享状态时，将状态提升到最近的共同父组件。
- **示例**：
	```js
	function Parent() {
	  const [value, setValue] = useState("");
	
	  return (
	    <div>
	      <ChildA value={value} onChange={setValue} />
	      <ChildB value={value} />
	    </div>
	  );
	}
	
	function ChildA({ value, onChange }) {
	  return <input value={value} onChange={e => onChange(e.target.value)} />;
	}
	
	function ChildB({ value }) {
	  return <p>Current value: {value}</p>;
	}
	```
### 组件复用与组合
#### 组件组合
- 通过组合子组件构建复杂 UI。
- **示例**：
	```js
	function Card({ header, children, footer }) {
	  return (
	    <div className="card">
	      {header && <div className="card-header">{header}</div>}
	      <div className="card-body">{children}</div>
	      {footer && <div className="card-footer">{footer}</div>}
	    </div>
	  );
	}
	
	function App() {
	  return (
	    <Card
	      header={<h1>Title</h1>}
	      footer={<button>Action</button>}
    >	
	      <p>Card content</p>
	    </Card>
	  );
	}
	```

## React Hooks
### Hooks简介
- **Hooks 的背景与优势**
	- **背景**：
		- 在类组件中，状态管理、生命周期逻辑复杂且难以复用。
		- 类组件涉及 this 绑定问题，增加了代码复杂性。
		- Hooks 使函数组件能够完全替代类组件。
	- **优势**：
		- 代码更简洁，减少样板代码。
		- 逻辑复用更简单，通过自定义 Hooks 实现。
		- 无 `this` 问题，函数式编程更直观。
		- 便于测试和维护。
- **Hooks 的规则**
	- **仅在顶层调用**：不要在循环、条件或嵌套函数中调用 Hooks，确保每次渲染调用顺序一致。
	- **仅在函数组件或自定义 Hooks 中使用**：Hooks 不可在普通 JavaScript 函数中使用。
	- **ESLint 插件**：使用 `eslint-plugin-react-hooks` 确保遵循规则。
- **Hooks 解决的问题**
	- 复用状态逻辑（如数据获取、表单处理）。
	- 简化复杂组件的逻辑拆分。
	- 替代类组件的生命周期方法。


## 状态管理



## 路由管理
### 路由管理概述
- 路由管理是 React 单页应用（SPA）的核心功能，用于根据 URL 渲染不同组件，实现页面导航而无需刷新整个页面。
- React 本身不提供内置路由功能，通常依赖第三方库如 **React Router** 来实现。
- **定义**：路由管理通过映射 URL 路径到特定组件，控制应用的导航和视图切换。
- **优势**：
	- **单页应用体验**：无页面刷新，提供流畅的用户体验。
	- **动态导航**：根据用户操作或状态动态切换页面。
	- **模块化**：将页面逻辑拆分为独立组件，便于维护。
	- **灵活性**：支持嵌套路由、动态路由和权限控制。
### React Router
#### 简介
- React Router 是 React 生态中最流行的路由库，支持浏览器端和服务端渲染，提供强大的路由功能。
- **版本**：React Router v6 是当前主流版本（截至 2025），与 v5 相比有重大改进。
- **核心特性**：
	- 声明式路由配置。
	- 支持嵌套路由、动态路由、路由参数。
	- 提供 Hooks（如 useNavigate、useParams）简化开发。
	- 支持代码分割和懒加载。
#### 安装与配置
- **安装**：
	```bash
	npm install react-router-dom
	```
- **基本配置**： 使用 BrowserRouter 包裹应用，定义路由规则。
	```js
	import { BrowserRouter, Routes, Route } from 'react-router-dom';
	
	function App() {
	  return (
	    <BrowserRouter>
	      <Routes>
	        <Route path="/" element={<Home />} />
	        <Route path="/about" element={<About />} />
	      </Routes>
	    </BrowserRouter>
	  );
	}
	```
- **核心组件**：
	- BrowserRouter：基于 HTML5 History API 的路由容器。
	- Routes：定义路由规则的容器。
	- Route：映射路径到组件。
	- Link：用于导航，生成 `<a>` 标签。
### 核心路由功能
#### 基本路由
- 定义简单的路径到组件映射。
- 示例：
	```js
	import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';
	
	function Home() {
	  return <h1>Home Page</h1>;
	}
	
	function About() {
	  return <h1>About Page</h1>;
	}
	
	function App() {
	  return (
	    <BrowserRouter>
	      <nav>
	        <Link to="/">Home</Link> | <Link to="/about">About</Link>
	      </nav>
	      <Routes>
	        <Route path="/" element={<Home />} />
	        <Route path="/about" element={<About />} />
	      </Routes>
	    </BrowserRouter>
	  );
	}
	```
- **说明**：
	- Link 提供声明式导航，避免页面刷新。
	- Routes 和 Route 定义路由规则，element 指定渲染组件。
#### 动态路由
- 通过 URL 参数动态渲染内容。
- **用法**：
	- 使用 :param 定义动态路径。
	- 通过 useParams Hook 获取参数。
- 示例：
	```js
	import { useParams } from 'react-router-dom';
	
	function User() {
	  const { userId } = useParams();
	  return <h1>User ID: {userId}</h1>;
	}
	
	function App() {
	  return (
	    <BrowserRouter>
	      <Routes>
	        <Route path="/user/:userId" element={<User />} />
	      </Routes>
	    </BrowserRouter>
	  );
	}
	```
- **访问**：/user/123 将渲染 User ID: 123。
#### 嵌套路由
- 支持在父路由内定义子路由，适合复杂页面布局。
- **用法**：
	- 使用 Outlet 组件渲染子路由内容。
- **示例**：
	```js
	import { Routes, Route, Link, Outlet } from 'react-router-dom';
	
	function Dashboard() {
	  return (
	    <div>
	      <h1>Dashboard</h1>
	      <nav>
	        <Link to="profile">Profile</Link> | <Link to="settings">Settings</Link>
	      </nav>
	      <Outlet />
	    </div>
	  );
	}
	
	function Profile() {
	  return <h2>Profile Page</h2>;
	}
	
	function Settings() {
	  return <h2>Settings Page</h2>;
	}
	
	function App() {
	  return (
	    <BrowserRouter>
	      <Routes>
	        <Route path="/dashboard" element={<Dashboard />}>
	          <Route path="profile" element={<Profile />} />
	          <Route path="settings" element={<Settings />} />
	        </Route>
	      </Routes>
	    </BrowserRouter>
	  );
	}
	```
- **说明**：
	- Outlet 渲染子路由的 element。
	- 访问 /dashboard/profile 显示 Profile 组件。
#### 路由导航
- **声明式导航**：使用 Link 或 NavLink。
	- NavLink：支持高亮当前路由（通过 className 或 style）。
- **编程式导航**：使用 useNavigate Hook。
- 示例：
	```js
	import { useNavigate } from 'react-router-dom';
	
	function LoginButton() {
	  const navigate = useNavigate();
	
	  const handleLogin = () => {
	    // 模拟登录逻辑
	    navigate('/dashboard');
	  };
	
	  return <button onClick={handleLogin}>Login</button>;
	}
	```
#### 路由守卫
- 通过自定义逻辑控制路由访问（如权限验证）。
- **示例**（受保护路由）：
	```js
	import { Navigate, Outlet } from 'react-router-dom';
	
	function ProtectedRoute({ isAuthenticated }) {
	  if (!isAuthenticated) {
	    return <Navigate to="/login" replace />;
	  }
	  return <Outlet />;
	}
	
	function App() {
	  const isAuthenticated = false; // 模拟认证状态
	  return (
	    <BrowserRouter>
	      <Routes>
	        <Route path="/login" element={<Login />} />
	        <Route element={<ProtectedRoute isAuthenticated={isAuthenticated} />}>
	          <Route path="/dashboard" element={<Dashboard />} />
	        </Route>
	      </Routes>
	    </BrowserRouter>
	  );
	}
	```
- **说明**：
	- Navigate 重定向到指定路径。
	- replace 替换历史记录，避免返回受保护页面。
#### 404 页面
- 处理未匹配的路由。
- 示例：
	```js
	function NotFound() {
	  return <h1>404 - Page Not Found</h1>;
	}
	
	function App() {
	  return (
	    <BrowserRouter>
	      <Routes>
	        <Route path="/" element={<Home />} />
	        <Route path="*" element={<NotFound />} />
	      </Routes>
	    </BrowserRouter>
	  );
	}
	```
- **说明**：`path="*"` 匹配所有未定义路径。
### 路由优化
#### 路由懒加载
- 通过 React.lazy 和 Suspense 实现按需加载，减少初始加载时间。
- **示例**：
	```js
	import { lazy, Suspense } from 'react';
	import { BrowserRouter, Routes, Route } from 'react-router-dom';
	
	const Home = lazy(() => import('./Home'));
	const About = lazy(() => import('./About'));
	
	function App() {
	  return (
	    <BrowserRouter>
	      <Suspense fallback={<div>Loading...</div>}>
	        <Routes>
	          <Route path="/" element={<Home />} />
	          <Route path="/about" element={<About />} />
	        </Routes>
	      </Suspense>
	    </BrowserRouter>
	  );
	}
	```
- **优势**：
	- 按需加载组件，减少 bundle 大小。
	- Suspense 提供加载中的 UI。
#### 代码分割
- 结合 Webpack 或 Vite 实现路由级别的代码分割。
- **Webpack 配置**（示例）：
	```js
	const Home = lazy(() => import(/* webpackChunkName: "home" */ './Home'));
	```
- **说明**：为每个路由生成单独的 chunk 文件。
#### 路由预加载
- 通过预加载关键路由提升用户体验。
- **示例**（手动预加载）：
	```js
	function preloadComponent(path) {
	  import(path).then(module => console.log(`Preloaded ${path}`));
	}
	
	function App() {
	  useEffect(() => {
	    preloadComponent('./About');
	  }, []);
	  return <BrowserRouter>{/* 路由配置 */}</BrowserRouter>;
	}
	```
### React Router Hooks
#### useNavigate
- 编程式导航。
- 示例：
	```js
	function Button() {
	  const navigate = useNavigate();
	  return <button onClick={() => navigate('/home')}>Go Home</button>;
	}
	```
#### useParams
- 获取动态路由参数。
- 示例：
	```js
	function UserProfile() {
	  const { userId } = useParams();
	  return <h1>User: {userId}</h1>;
	}
	```
#### useLocation
- 访问当前 URL 信息。
- 示例：
	```js
	import { useLocation } from 'react-router-dom';
	
	function CurrentPath() {
	  const location = useLocation();
	  return <p>Current path: {location.pathname}</p>;
	}
	```
#### useSearchParams
- 管理查询参数。
- 示例：
	```js
	import { useSearchParams } from 'react-router-dom';
	
	function Search() {
	  const [searchParams, setSearchParams] = useSearchParams();
	  const query = searchParams.get('q');
	
	  return (
	    <div>
	      <input
	        value={query || ''}
	        onChange={e => setSearchParams({ q: e.target.value })}
	      />
	      <p>Search: {query}</p>
	    </div>
	  );
	}
	```
#### useRouteError
- 捕获路由错误（v6 特性）。
- 示例：
	```js
	import { useRouteError } from 'react-router-dom';
	
	function ErrorBoundary() {
	  const error = useRouteError();
	  return <p>Error: {error.message}</p>;
	}
	```
### 服务端渲染（SSR）与路由
- React Router 支持服务端渲染，常用框架如 **Next.js** 简化实现。
- **Next.js 路由**：
	- 文件系统路由：pages/about.js 自动映射为 /about。
	- 动态路由：pages/user/[id].js 对应 /user/:id。
	- API 路由：pages/api/data.js 处理后端请求。
- **React Router SSR**：
	- 使用 StaticRouter 替代 BrowserRouter。
	- 示例：
		```js
		import { StaticRouter } from 'react-router-dom/server';
		
		function ServerApp({ url }) {
		  return (
		    <StaticRouter location={url}>
		      <Routes>
		        <Route path="/" element={<Home />} />
		      </Routes>
		    </StaticRouter>
		  );
		}
		```







# 配置




# 思考



# 附录
> [!website]
    > 

> [!code]
    > 

> [!video]
    > 

> [!doc]
    > 

