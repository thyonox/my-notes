![[Express.png|980x500]]
# 概述
---
## 简介
---
- Express.js 是一个基于 Node.js 的轻量、灵活的 Web 应用框架，用于快速构建 Web 应用程序和 RESTful API。
- 它以简洁和无强制约束（unopinionated）为核心，提供强大的 HTTP 工具、中间件支持和路由机制，简化了服务器端开发。
- Express 支持模块化路由、多种模板引擎（如 Pug、EJS）以及静态文件服务，适合从简单应用到复杂项目的开发。
- 其丰富的生态系统和第三方中间件（如 body-parser、morgan）使其易于扩展，常用于 MEAN 技术栈。
- Express 易于上手，性能优异，是 Node.js 生态中最流行的 Web 框架之一。
## 需求
---
- **原生 HTTP 模块的复杂性**
	- 问题：Node.js 的 http 模块需要手动处理 HTTP 请求、解析 URL、处理头信息和请求体等，代码量大且容易出错。例如，处理一个简单的 GET 请求需要大量代码：
		```js
		const http = require('http');
		const server = http.createServer((req, res) => {
		  if (req.method === 'GET' && req.url === '/hello') {
		    res.writeHead(200, { 'Content-Type': 'text/plain' });
		    res.end('Hello World');
		  } else {
		    res.writeHead(404);
		    res.end();
		  }
		});
		server.listen(3000);
		```
	- Express 解决方案：Express 提供简洁的路由和响应方法，简化请求处理：
		```js
		const express = require('express');
		const app = express();
		app.get('/hello', (req, res) => res.send('Hello World'));
		app.listen(3000);
		```
- **路由管理的复杂性**
	- 问题：在原生 Node.js 中，开发者需要手动解析 URL 并根据路径和 HTTP 方法编写条件逻辑，难以维护复杂路由。例如，处理多个路径（如 `/users`、`/products`）需要大量 `if-else` 或 `switch` 语句。
	- Express 解决方案：Express 提供强大的路由机制，支持基于 HTTP 方法（GET、POST 等）和路径的路由定义，还支持模块化路由：
		```js
		const express = require('express');
		const app = express();
		const userRouter = express.Router();
		userRouter.get('/', (req, res) => res.send('User list'));
		app.use('/users', userRouter);
		```
- **中间件功能的缺失**
	- 问题：Node.js 原生 http 模块没有内置中间件机制，开发者需要手动实现请求预处理逻辑（如解析请求体、身份验证、日志记录等），导致代码重复且难以复用。
	- Express 解决方案：Express 的核心是中间件机制，允许开发者插入可复用的函数处理请求。例如：
		```js
		app.use((req, res, next) => {
		  console.log(`Request at ${Date.now()}`);
		  next();
		});
		```
	- Express 还支持第三方中间件（如 body-parser 解析 JSON、morgan 记录日志），极大减少了重复开发。
- **静态文件和模板引擎支持不足**
	- 问题：Node.js 原生模块需要手动读取和返回静态文件（如 CSS、图片），且没有内置的模板引擎支持，动态生成 HTML 页面非常麻烦。
	- Express 解决方案：
		- 静态文件：通过 `express.static` 轻松提供静态资源：
			```js
			app.use(express.static('public'));
			```
		- 模板引擎：支持 Pug、EJS 等模板引擎，动态渲染页面：
			```js
			app.set('view engine', 'pug');
			app.get('/', (req, res) => res.render('index', { title: 'Home' }));
			```
- **错误处理和扩展性不足**
	- 问题：原生 Node.js 的错误处理需要手动实现，难以统一管理，且扩展功能（如集成数据库或身份验证）需要从头开发。
	- Express 解决方案：
		- 提供专门的错误处理中间件：
			```js
			app.use((err, req, res, next) => {
			  res.status(500).send('Server Error');
			});
			```
		- 通过中间件生态和模块化设计，Express 易于集成数据库（如 MongoDB）、身份验证（如 Passport.js）等。
- **开发效率和社区支持**
	- 问题：Node.js 原生开发需要开发者手动处理许多通用功能（如 CORS、文件上传），缺乏统一的生态支持，开发效率低。
	- Express 解决方案：Express 拥有庞大的社区和丰富的中间件生态（如 `cors`、`multer`），开发者可以快速集成现有解决方案，专注于业务逻辑。此外，Express 生成器（`express-generator`）能快速生成项目结构，进一步提升开发效率。
## 对比
---

|**维度**|**Express.js**|**Koa.js**|**Fastify**|**NestJS**|**Hapi.js**|
|---|---|---|---|---|---|
|**核心理念**|轻量、灵活、无强制约束（unopinionated）|现代、轻量、基于 async/await|高性能、低开销、插件化|模块化、结构化、基于 TypeScript|配置驱动、适合复杂 API|
|**性能**|中等（基于回调，性能良好但非最优）|中等（async/await 优化事件循环）|高（专注于低开销，接近原生 Node.js）|中等（依赖 Express 或 Fastify，略有开销）|中等（配置较多，性能稍逊于 Fastify）|
|**学习曲线**|低（API 简单，易上手）|中等（需熟悉 async/await）|中等（需了解插件系统和 JSON Schema）|高（需熟悉 TypeScript 和 Angular 风格）|中等（配置复杂，需学习特定 API）|
|**生态系统**|丰富（最成熟，中间件和插件最多）|中等（生态较小，但社区活跃）|中等（快速增长，插件生态完善）|丰富（支持 Express/Fastify，社区活跃）|中等（有专用插件，生态不如 Express）|
|**中间件/插件机制**|中间件（req, res, next）|中间件（洋葱模型，异步优先）|插件系统（支持异步，灵活封装）|模块化中间件（基于装饰器和 DI）|配置式插件（高度可定制）|
|**路由**|简单路由，模块化（express.Router）|简单路由，支持 async/await|高效路由，支持 JSON Schema 验证|控制器+装饰器，结构化路由|配置式路由，细粒度控制|
|**模板引擎支持**|支持（Pug、EJS 等）|支持（需手动配置）|支持（需插件，如 point-of-view）|支持（需配置，推荐 REST API）|支持（Handlebars 等）|
|**类型支持**|弱（需 @types/express）|弱（需 @types/koa）|强（内置 JSON Schema，TypeScript 支持好）|强（原生 TypeScript，装饰器驱动）|中等（TypeScript 支持需额外配置）|
|**适用场景**|快速开发、REST API、Web 应用、中小型项目|现代 Web 应用、需要 async/await 的项目|高性能 API、微服务、大流量场景|企业级应用、复杂后端、需要强类型支持|复杂 API、企业级应用、需要细粒度配置|
|**社区和文档**|非常成熟，文档全面，教程丰富|社区活跃，文档清晰|社区快速增长，文档较完善|社区活跃，文档详细，偏企业级|社区较小，文档完善但学习成本较高|
|**维护状态**|活跃（Express 5.0 Beta 持续更新）|活跃（Koa 2.x 稳定）|活跃（快速迭代，性能优化持续进行）|活跃（TypeScript 生态热门）|活跃（但社区规模较小）|

# 开始
---
## 安装
---
- **基本安装方式**
	1. 创建目录并初始化 Node.JS 项目
		- 运行以下命令生成 package.json 文件：
			```bash
			npm init -y
			```
			- `-y` 表示使用默认配置，跳过交互式设置。
	2. 安装 Express
		- 在项目目录中运行以下命令安装 Express：
			```js
			npm install express
			```
		- 这会在 `package.json` 中添加 Express 作为依赖，并将 Express 安装到 `node_modules` 目录。
	3. 验证安装
		- 创建一个简单的 Express 应用（如 app.js）来验证安装：
			```js
			const express = require('express');
			const app = express();
			
			app.get('/', (req, res) => {
			  res.send('Hello World!');
			});
			
			app.listen(3000, () => {
			  console.log('Server running at http://localhost:3000');
			});
			```
		- 保存后运行：
			```bash
			node app.js
			```
		- 访问 `http://localhost:3000`，浏览器应显示 “Hello World!”。
- **使用 Express 生成器**
	1. 全局安装 Express 生成器
		```bash
		npm install -g express-generator
		```
	2. 生成项目
		```bash
		express my-express-app
		```
		- 这会生成包含以下结构的目录：
			- `app.js`：主应用文件。
			- `routes/`：路由文件目录。
			- `views/`：模板文件目录（默认使用 Pug）。
			- `public/`：静态文件目录。
			- `package.json`：项目依赖和脚本。
	3. 安装依赖并启动
		```bash
		cd my-express-app
		npm install
		npm start
		```
		- 默认访问 `http://localhost:3000` 查看应用。
	4. 自定义生成器选项
		- 生成器支持选项，例如指定模板引擎：
			```bash
			express my-express-app --view=ejs
			```
		- 支持的模板引擎包括 `pug`（默认）、`ejs`、`hbs`（Handlebars）等。
- **安装常用中间件**
	- body-parser（解析 JSON/URL 编码数据，Express 4.16+ 已内置 `express.json()`）：
		```bash
		npm install body-parser
		```
	- cookie-parser（解析 Cookie）：
		```bash
		npm install cookie-parser
		```
	- morgan（HTTP 请求日志）：
		```bash
		npm install morgan
		```
	- cors（跨域资源共享）：
		```bash
		npm install cors
		```
- **开发环境优化**
	- 安装 nodemon（自动重启服务器，适合开发）：
		```bash
		npm install --save-dev nodemon
		```
		- 修改 `package.json` 的 scripts：
			```json
			"scripts": {
			  "start": "node app.js",
			  "dev": "nodemon app.js"
			}
			```
		- 运行开发模式：
			```bash
			npm run dev
			```
	- 环境变量管理（使用 dotenv）：
		```bash
		npm install dotenv
		```
		- 创建 .env 文件（如 PORT=3000），然后在代码中使用：
			```js
			require('dotenv').config();
			app.listen(process.env.PORT || 3000);
			```
## 使用
---
```text
my-express-app/
├── app.js                # 主应用文件，项目入口
├── package.json          # 项目依赖和脚本配置
├── bin/
│   └── www               # 启动脚本，配置服务器监听
├── public/               # 静态文件目录
│   ├── images/           # 图片资源
│   ├── javascripts/      # 客户端 JavaScript 文件
│   └── stylesheets/      # CSS 文件
│       └── style.css
├── routes/               # 路由文件目录
│   ├── index.js          # 默认主页路由
│   └── users.js          # 示例用户路由
├── views/                # 模板文件目录（默认使用 Pug）
│   ├── error.pug         # 错误页面模板
│   ├── index.pug         # 主页模板
│   └── layout.pug        # 布局模板
└── node_modules/         # 依赖包（运行 npm install 后生成）
```
# 概念
---
## 中间件
---
### 中间件概述
---
- **定义**：中间件是一个函数，接收 `req`、`res` 和 `next` 参数，用于在请求到达最终路由处理程序之前执行逻辑（如解析请求体、身份验证、日志记录等）。
- **执行顺序**：中间件按注册顺序依次执行，调用 `next()` 将控制权传递给下一个中间件或路由处理器。如果不调用 `next()`，请求会挂起。
- **功能**：
    - 修改 `req` 或 `res` 对象。
    - 执行预处理任务（如数据解析、认证）。
    - 终止请求（直接发送响应）。
    - 调用下一个中间件或路由。
### 中间件结构
---
```js
function myMiddleware(req, res, next) {
  // 执行逻辑
  console.log('Request URL:', req.url);
  next(); // 继续执行下一个中间件
}
```
- 参数：
	- `req`：请求对象，包含请求信息（如 URL、头部、查询参数）。
	- `res`：响应对象，用于发送响应。
	- `next`：函数，调用以进入下一个中间件或路由。
- 错误处理中间件：特殊形式，接收四个参数：
	```js
	function errorMiddleware(err, req, res, next) {
	  console.error(err.stack);
	  res.status(500).send('Something went wrong!');
	}
	```
### 中间件类型
---
- **应用级中间件**
	- 定义：绑定到 Express 应用实例（`app`），作用于所有或特定路径的请求。
	- 使用方式：
	    - 使用 `app.use()` 或 `app.METHOD()`（如 `app.get()`）注册。
	    - 可指定路径前缀（如 `/api`）。
	- 示例：
		```js
		const express = require('express');
		const app = express();
		
		// 全局中间件，所有请求都会经过
		app.use((req, res, next) => {
		  console.log('Time:', Date.now());
		  next();
		});
		
		// 特定路径中间件，仅作用于 /api 开头的请求
		app.use('/api', (req, res, next) => {
		  console.log('API request');
		  next();
		});
		
		app.listen(3000);
		```
- **路由级中间件**
	- 定义：绑定到 `express.Router()` 实例，作用于特定路由模块。
	- 使用方式：通过 `router.use()` 或路由方法（如 `router.get()`）注册。
	- 示例：
		```js
		const express = require('express');
		const router = express.Router();
		
		router.use((req, res, next) => {
		  console.log('Router middleware');
		  next();
		});
		
		router.get('/users', (req, res) => res.send('User list'));
		
		app.use('/api', router);
		```
- **错误处理中间件**
	- 定义：专门处理错误，接收 (`err`, `req`, `res`, `next`) 四个参数。
	- 使用方式：通常放在所有中间件和路由之后，用于捕获异常。
	- 示例：
		```js
		app.use((err, req, res, next) => {
		  console.error(err.stack);
		  res.status(500).send('Server Error');
		});
		```
	- 注意：只有在中间件或路由中调用 `next(err)` 时，错误处理中间件才会被触发：
		```js
		app.get('/error', (req, res, next) => {
		  next(new Error('Something broke!'));
		});
		```
- **内置中间件**
	- Express 提供内置中间件，4.16+ 版本后部分功能直接集成。
	- 常见内置中间件：
		- `express.json()`：解析 JSON 请求体。
		- `express.urlencoded({ extended: true })`：解析 URL 编码的表单数据。
		- `express.static('public')`：提供静态文件服务。
	- 示例：
		```js
		app.use(express.json());
		app.use(express.static('public'));
		```
- **第三方中间件**
	- 社区提供的中间件，扩展 Express 功能。
	- 常见第三方中间件：
		- `body-parser`：解析请求体（4.16+ 后部分功能内置）。
		- `cookie-parser`：解析 Cookie。
		- `morgan`：记录 HTTP 请求日志。
		- `cors`：启用跨域资源共享。
		- `multer`：处理文件上传。
	- 安装与使用：
		```bash
		npm install morgan cors
		```
		```js
		const morgan = require('morgan');
		const cors = require('cors');
		
		app.use(morgan('dev')); // 日志记录
		app.use(cors()); // 允许跨域
		```
### 中间件使用
---
- 全局中间件：通过 `app.use()` 注册，作用于所有请求。
	```js
	app.use((req, res, next) => {
	  req.requestTime = Date.now();
	  next();
	});
	```
- 路径特定中间件：指定路径前缀。
	```js
	app.use('/api/users', (req, res, next) => {
	  console.log('User API');
	  next();
	});
	```
- 多个中间件：为同一路由指定多个中间件，按顺序执行。
	```js
	app.get('/users', [middleware1, middleware2], (req, res) => {
	  res.send('Users');
	});
	```
- 跳过中间件：通过条件逻辑或直接调用 `res.send()` 终止请求。
	```js
	app.use((req, res, next) => {
	  if (req.query.skip) {
	    res.send('Skipped middleware');
	  } else {
	    next();
	  }
	});
	```
### 中间件应用
---
- 日志记录：记录请求信息（如 URL、时间）。
	```js
	app.use((req, res, next) => {
	  console.log(`${req.method} ${req.url} - ${Date.now()}`);
	  next();
	});
	```
- 身份验证：检查用户是否登录。
	```js
	function authMiddleware(req, res, next) {
	  if (req.headers.authorization) {
	    next();
	  } else {
	    res.status(401).send('Unauthorized');
	  }
	}
	app.use('/protected', authMiddleware);
	```
- 请求体解析：处理 POST 请求的 JSON 数据。
	```js
	app.use(express.json());
	app.post('/data', (req, res) => {
	  console.log(req.body); // 访问 JSON 数据
	  res.send('Data received');
	});
	```
- 错误处理：统一处理异常。
	```js
	app.use((err, req, res, next) => {
	  res.status(500).json({ error: err.message });
	});
	```
- CORS 配置：允许跨域请求。
	```js
	const cors = require('cors');
	app.use(cors({ origin: 'http://example.com' }));
	```
### 工作原理
---
- 洋葱模型：Express 中间件的执行类似洋葱模型，请求从外层进入，经过所有中间件处理后再返回（响应不会经过中间件）。
- 执行流程：
	- 请求到达 Express 应用。
	- 中间件按注册顺序依次执行，调用 `next()` 继续。
	- 如果中间件调用 `res.send()` 、 `res.end()` 或 `res.json()`，请求终止。
	- 错误发生时，调用 `next(err)` 跳转到错误处理中间件。
- 示例：
	```js
	app.use((req, res, next) => {
	  console.log('Middleware 1 start');
	  next();
	  console.log('Middleware 1 end');
	});
	app.use((req, res, next) => {
	  console.log('Middleware 2 start');
	  res.send('Done');
	  // next() 未调用，请求终止
	});
	```
- 输出：
	```js
	Middleware 1 start
	Middleware 2 start
	```
### 注意事项
---
- **调用 next()**：不调用 `next()` 会导致请求挂起，客户端无响应。
- **错误处理中间件位置**：必须放在所有中间件和路由之后，否则无法捕获错误。
- **性能优化**：避免在中间件中执行耗时操作（如复杂计算），以免阻塞请求。
- **中间件顺序**：顺序影响执行逻辑，需合理安排（如解析中间件放在认证中间件之前）。
- **异步中间件**：处理异步操作时，使用 `async/await` 并确保错误传递给 `next(err)`：
```js
app.use(async (req, res, next) => {
  try {
    const data = await someAsyncFunction();
    next();
  } catch (err) {
    next(err);
  }
});
```
### 高级用法
---
- 条件中间件：根据请求条件动态应用中间件。
	```js
	app.use((req, res, next) => {
	  if (req.method === 'GET') {
	    next();
	  } else {
	    res.status(403).send('Only GET allowed');
	  }
	});
	```
- 模块化中间件：将中间件封装为模块，复用性更强。
	```js
	// middleware/logger.js
	module.exports = (req, res, next) => {
	  console.log(`${req.method} ${req.url}`);
	  next();
	};
	
	// app.js
	const logger = require('./middleware/logger');
	app.use(logger);
	```
- 第三方中间件生态：利用社区中间件快速实现功能，如 `helmet`（安全头）、`compression`（响应压缩）。
## 路由
---
### 路由概述
---
- **定义**：路由是指根据 HTTP 方法（如 GET、POST）和请求路径（如 `/users`）将客户端请求映射到对应的处理程序（Handler）。
- **核心组件**：
    - **HTTP 方法**：如 GET、POST、PUT、DELETE 等，对应 `app.get()`、`app.post()` 等。
    - **路径**：URL 路径，如 `/`、`/users/:id`。
    - **处理程序**：处理请求的回调函数，接收 `req`（请求对象）、`res`（响应对象）和 `next`（下一个中间件）。
- **基本示例**：
	```js
	const express = require('express');
	const app = express();
	
	app.get('/', (req, res) => {
	  res.send('Hello World!');
	});
	
	app.listen(3000);
	```
### 路由结构
---
- 语法：
	```js
	app.METHOD(PATH, HANDLER);
	```
	- `METHOD`：HTTP 方法（如 `get`、`post`、`put`、`delete`）。
	- `PATH`：请求的 URL 路径（字符串或正则表达式）。
	- `HANDLER`：处理函数，形如 `(req, res, next) => {}`。
- 示例：
	```js
	app.post('/users', (req, res) => {
	  res.send('User created');
	});
	```
- 多处理程序：一个路由可以绑定多个处理程序（中间件+最终处理）。
	```js
	app.get('/users', 
	  (req, res, next) => {
	    console.log('Middleware 1');
	    next();
	  },
	  (req, res) => {
	    res.send('User list');
	  }
	);
	```
### 路由方法
---
- `app.get()`：处理 GET 请求。
- `app.post()`：处理 POST 请求。
- `app.put()`：处理 PUT 请求。
- `app.delete()`：处理 DELETE 请求。
- `app.all()`：处理所有 HTTP 方法。
- 特殊方法：Express 还支持 `patch`、`options` 等其他 HTTP 方法。
- 示例（app.all）：
	```js
	app.all('/test', (req, res) => {
	  res.send(`Received ${req.method} request`);
	});
	```
### 路由路径
---
- 字符串路径：
	```js
	app.get('/about', (req, res) => res.send('About page'));
	```
- 路径模式：
	- 使用 `?`（可选字符）、`+`（一个或多个）、`*`（任意字符）。
	- 示例：
		```js
		app.get('/ab?out', (req, res) => res.send('Matches /about or /aout')); // ? 表示 b 可选
		app.get('/ab+cd', (req, res) => res.send('Matches /abcd, /abbcd, etc.')); // + 表示 b 重复
		app.get('/ab*cd', (req, res) => res.send('Matches /abcd, /abxyzcd, etc.')); // * 表示任意
		```
- 正则表达式路径：
	```js
	app.get(/.*fly$/, (req, res) => res.send('Matches butterfly, dragonfly, etc.'));
	```
### 路径参数
---
- 定义：使用 `:` 定义动态路径参数，参数值存储在 `req.params` 中。
- 示例：
	```js
	app.get('/users/:id', (req, res) => {
	  res.send(`User ID: ${req.params.id}`);
	});
	```
	- 访问 `/users/123`，返回 “User ID: 123”。
- 多参数：
	```js
	app.get('/users/:userId/posts/:postId', (req, res) => {
	  res.send(`User: ${req.params.userId}, Post: ${req.params.postId}`);
	});
	```
- 可选参数：使用 `?` 使参数可选。
	```js
	app.get('/users/:id?', (req, res) => {
	  res.send(req.params.id ? `User: ${req.params.id}` : 'All users');
	});
	```
### 查询参数
---
- 查询参数存储在 `req.query` 中，URL 格式为 `/path?key=value`。
- 示例：
	```js
	app.get('/search', (req, res) => {
	  const query = req.query.q; // 获取 ?q=xxx
	  res.send(`Search query: ${query}`);
	});
	```
	- 访问 `/search?q=express`，返回 “Search query: express”。
### 模块化路由
---
- 定义：使用 `express.Router()` 创建模块化路由，便于代码组织和复用。
- 示例：
	```js
	// routes/users.js
	const express = require('express');
	const router = express.Router();
	
	router.get('/', (req, res) => res.send('User list'));
	router.get('/:id', (req, res) => res.send(`User: ${req.params.id}`));
	
	module.exports = router;
	
	// app.js
	const express = require('express');
	const app = express();
	const userRouter = require('./routes/users');
	
	app.use('/users', userRouter); // 挂载到 /users 路径
	app.listen(3000);
	```
	- 访问 `/users` 返回 “User list”，`/users/123` 返回 “User: 123”。
- 优点：
	- 代码分离，适合大型项目。
	- 可为不同路由模块添加特定中间件。
### 路由中间件
---
- 路由可以与中间件结合，用于预处理（如认证、日志）。
- 示例：
	```js
	const authMiddleware = (req, res, next) => {
	  if (req.headers.authorization) next();
	  else res.status(401).send('Unauthorized');
	};
	
	app.get('/protected', authMiddleware, (req, res) => {
	  res.send('Protected resource');
	});
	```
- 多个中间件：
	```js
	app.get('/admin', [authMiddleware, logMiddleware], (req, res) => {
	  res.send('Admin page');
	});
	```
### 链式路由
---
- 使用 `router.route()` 定义同一路径的多种 HTTP 方法，提高代码简洁性。
- 示例：
	```js
	router.route('/users/:id')
	  .get((req, res) => res.send(`Get user: ${req.params.id}`))
	  .put((req, res) => res.send(`Update user: ${req.params.id}`))
	  .delete((req, res) => res.send(`Delete user: ${req.params.id}`));
	```
### 错误处理与路由
---
- 路由中发生的错误可以通过 `next(err)` 传递到错误处理中间件。
- 示例：
	```js
	app.get('/error', (req, res, next) => {
	  next(new Error('Invalid request'));
	});
	
	app.use((err, req, res, next) => {
	  res.status(500).json({ error: err.message });
	});
	```
### 最佳实践
---
- **模块化**：使用 express.Router 分离路由逻辑，保持 app.js 简洁。
- **RESTful 风格**：遵循 REST 规范设计 API（如 /users 获取列表，/users/:id 获取单个资源）。
- **参数验证**：结合中间件（如 express-validator）验证路由参数和查询参数。
- **版本化**：为 API 添加版本前缀（如 /api/v1/users）。
- **错误处理**：统一错误处理，确保路由中的异常被捕获。
### 注意事项
---
- **路径顺序**：Express 按路由定义顺序匹配，动态路由（如 `/:id`）应放在静态路由（如 `/about`）之后。
	```js
	app.get('/about', (req, res) => res.send('About')); // 先匹配
	app.get('/:id', (req, res) => res.send(`ID: ${req.params.id}`)); // 后匹配
	```
- **性能**：避免在路由处理程序中执行复杂逻辑，尽量使用中间件预处理。
- **大小写敏感**：默认路径大小写敏感，可通过 `app.set('case sensitive routing', true)` 配置。
- **斜杠处理**：默认忽略末尾斜杠，可通过 `app.set('strict routing', true)` 强制严格匹配。
## 模板引擎
---
### 模板引擎概述
---
- **定义**：模板引擎是一种工具，允许开发者在 HTML 模板中嵌入动态数据（如变量、循环、条件），Express 通过模板引擎渲染动态页面并返回给客户端。
- **作用**：
    - 将数据与 HTML 结构分离，提高代码可维护性。
    - 支持动态内容生成，如用户列表、博客页面等。
    - 适合服务端渲染（SSR），与前端框架（如 React）不同，模板引擎直接在服务器生成完整 HTML。
- **Express 中的支持**：Express 不内置模板引擎，但通过配置支持多种第三方模板引擎，如 Pug、EJS、Handlebars 等。
### 常用模板引擎
---

| **模板引擎**       | **特点**                                      | **语法风格**            | **适用场景**            |
| -------------- | ------------------------------------------- | ------------------- | ------------------- |
| **Pug**        | 简洁、缩进敏感（原名 Jade），基于空格和缩进，生成简洁的 HTML。        | 类似 Haml，缩进驱动        | 快速开发、简洁模板、静态页面      |
| **EJS**        | 嵌入 JavaScript 代码，语法接近原生 HTML，易于上手。          | 嵌入 `<% %>` 标签       | 熟悉 HTML 的开发者、灵活性要求高 |
| **Handlebars** | 逻辑分离，基于 Mustache，语法简单，支持部分模板和帮助函数（helpers）。 | `{{}}` 语法           | 结构化模板、复杂逻辑、团队协作     |
| **Nunjucks**   | 功能丰富，支持继承、过滤器、宏，类似 Jinja2，适合复杂项目。           | `{% %}` 和 `{{}}` 语法 | 大型项目、需要高级模板功能       |
### 配置模板引擎
---
1. 安装模板引擎
	```bash
	npm install pug  # 安装 Pug
	npm install ejs  # 安装 EJS
	```
2. 设置模板引擎
	```js
	const express = require('express');
	const app = express();
	
	// 设置模板引擎为 Pug
	app.set('view engine', 'pug');
	// 设置模板文件目录（默认 ./views）
	app.set('views', './views');
	
	app.listen(3000);
	```
3. 渲染模板
	- 使用 `res.render()` 渲染模板并传递数据
		```js
		app.get('/', (req, res) => {
		  res.render('index', { title: 'Home', message: 'Welcome to Express' });
		});
		```
4. 模板文件
	- 模板文件存放在 `views/` 目录，文件名与 `res.render()` 的第一个参数对应。
	- Pug 示例（`views/index.pug`）：
		```pug
		html
		  head
		    title= title
		  body
		    h1= message
		```
	- EJS 示例（`views/index.ejs`）：
		```html
		<!DOCTYPE html>
		<html>
		  <head>
		    <title><%= title %></title>
		  </head>
		  <body>
		    <h1><%= message %></h1>
		  </body>
		</html>
		```
	- 访问 `http://localhost:3000`，页面显示 `<h1>Welcome to Express</h1>`
### 模板引擎使用
---
#### Pug
---
#### EJS
---
#### Handlebars
---
#### Nunjucks
---
### 常见场景
---
- 动态页面渲染：显示用户列表、文章详情等。
	```js
	app.get('/users', (req, res) => {
	  res.render('users', { users: [{ name: 'Alice' }, { name: 'Bob' }] });
	});
	```
- 布局复用：使用模板继承（如 Pug 的 `extends` 或 Handlebars 的 `partials`）避免重复代码。
- 条件和循环：在模板中处理条件渲染和列表循环。
	```js
	if user.isAdmin
	  p Admin User
	else
	  p Regular User
	```
- 表单处理：结合 POST 请求动态渲染页面。
	```js
	app.post('/submit', express.urlencoded({ extended: true }), (req, res) => {
	  res.render('result', { name: req.body.name });
	});
	```
### 模板引擎与 Express 的集成
---
- **视图目录**：默认 `views/`，可通过 `app.set('views', './custom_views')` 修改。
- **文件扩展名**：默认根据模板引擎（如 `.pug`、`.ejs`），可在 `res.render()` 中省略扩展名。
- **缓存**：生产环境中，模板默认缓存以提高性能，可通过 `app.set('view cache', true)` 控制。
- **错误处理**：确保模板文件存在，否则渲染会抛出错误，需捕获：
	```js
	app.use((err, req, res, next) => {
	  res.status(500).send('Template error: ' + err.message);
	});
	```
## 静态文件服务
---
### 静态文件服务概述
---
- **定义**：Express 的静态文件服务允许将服务器上的文件（如 CSS、JavaScript、图片、HTML）直接提供给客户端，无需额外的路由处理。通过 `express.static` 中间件，Express 将指定目录中的文件映射到特定的 URL 路径。
- **用途**：
    - 提供前端资源（如 CSS、JS 文件）。
    - 托管静态 HTML 页面。
    - 服务图片、字体或其他静态资产。
- **核心功能**：`express.static` 是 Express 内置的中间件，基于 `serve-static` 模块，用于处理静态文件请求。
### 基本使用
---
- 配置：使用 `app.use()` 挂载 `express.static` 中间件，指定静态文件目录。
- 示例：
	```js
	const express = require('express');
	const app = express();
	
	// 设置静态文件目录为 public
	app.use(express.static('public'));
	
	app.listen(3000, () => {
	  console.log('Server running at http://localhost:3000');
	});
	```
	- 假设 public 目录结构如下：
		```text
		public/
		├── css/
		│   └── style.css
		├── js/
		│   └── script.js
		└── images/
		    └── logo.png
		```
	- 访问路径：
		- `http://localhost:3000/css/style.css` → 获取 `public/css/style.css`
		- `http://localhost:3000/images/logo.png` → 获取 `public/images/logo.png`
### 配置静态文件路径
---
- 指定 URL 前缀：可以通过 `app.use()` 指定虚拟路径前缀。
	```js
	app.use('/static', express.static('public'));
	```
	- 访问：`http://localhost:3000/static/css/style.css` 获取 `public/css/style.css`。
	- 作用：将实际目录与 URL 路径解耦，增加灵活性。
- 多个静态目录：可以为多个目录配置静态文件服务。
	```js
	app.use('/static', express.static('public'));
	app.use('/assets', express.static('assets'));
	```
- 绝对路径：为避免路径问题，建议使用 `path` 模块指定绝对路径。
	```js
	const path = require('path');
	app.use('/static', express.static(path.join(__dirname, 'public')));
	```
### 静态文件服务选项
---
- `express.static(root, [options])` 支持可选配置参数，常见选项包括：
	- `dotfiles`：控制是否提供以`.`开头的文件。
		- 默认：`'ignore'`（忽略点文件，如 `.gitignore`）。
		- 可选：`'allow'`（允许访问）、`'deny'`（拒绝访问）。
		- 示例：
			```js
			app.use(express.static('public', { dotfiles: 'deny' }));
			```
	- `etag`：是否生成 ETag 用于缓存控制。
		- 默认：`true`。
		- 示例：
			```js
			app.use(express.static('public', { etag: false }));
			```
	- `extensions`：自动添加文件扩展名。
		- 示例：为 `/index` 自动匹配 `/index.html`。
			```js
			app.use(express.static('public', { extensions: ['html', 'htm'] }));
			```
	- `index`：指定默认文件（如访问目录时返回的文件）。
		- 默认：`'index.html'`。
		- 示例：禁用默认文件或指定其他文件。
			```js
			app.use(express.static('public', { index: false })); // 禁用 index.html
			app.use(express.static('public', { index: 'home.html' })); // 指定 home.html
			```
	- `maxAge`：设置缓存控制的 `Cache-Control` 头（以毫秒为单位）。
		- 示例：
			```js
			app.use(express.static('public', { maxAge: '1d' })); // 缓存一天
			```
	- `redirect`：当路径为目录时是否重定向到末尾带斜杠的 URL。
		- 默认：`true`。
		- 示例：
			```js
			app.use(express.static('public', { redirect: false }));
			```
- 完整示例：
	```js
	app.use('/static', express.static('public', {
	  dotfiles: 'ignore',
	  etag: true,
	  extensions: ['html', 'htm'],
	  index: 'index.html',
	  maxAge: '1h',
	  redirect: true
	}));
	```
### 常见场景
---
- 托管前端资源：为 Web 应用提供 CSS、JavaScript 和图片。
	```js
	app.use(express.static('public'));
	app.get('/', (req, res) => {
	  res.sendFile(path.join(__dirname, 'public', 'index.html'));
	});
	```
- 服务单页应用（SPA）：将所有路由指向 `index.html`，让前端框架（如 React、Vue）处理路由。
	```js
	app.use(express.static('public'));
	app.get('*', (req, res) => {
	  res.sendFile(path.join(__dirname, 'public', 'index.html'));
	});
	```
- 多目录静态服务：为不同类型的资源配置不同路径。
	```js
	app.use('/css', express.static('public/css'));
	app.use('/img', express.static('public/images'));
	```
- 结合模板引擎：静态文件服务 CSS/JS，模板引擎渲染动态 HTML。
	```js
	app.set('view engine', 'pug');
	app.use(express.static('public'));
	app.get('/', (req, res) => {
	  res.render('index', { title: 'Home' });
	});
	```
	- 模板中引用静态文件：
		```js
		link(rel='stylesheet', href='/css/style.css')
		```
### 与中简介结合
---
- 认证保护：为静态文件添加访问控制。
	```js
	const authMiddleware = (req, res, next) => {
	  if (req.query.token) next();
	  else res.status(401).send('Unauthorized');
	};
	app.use('/protected', authMiddleware, express.static('protected'));
	```
- 日志记录：结合 morgan 记录静态文件请求。
	```js
	const morgan = require('morgan');
	app.use(morgan('dev'));
	app.use(express.static('public'));
	```
### 生产环境优化
---
- 缓存控制：设置 `maxAge` 启用浏览器缓存，减少服务器压力。
	```js
	app.use(express.static('public', { maxAge: '30d' }));
	```
- 反向代理：生产环境中使用 Nginx 或 CDN 提供静态文件，减轻 Node.js 负担。
	```js
	location /static/ {
	  root /path/to/public;
	  expires 30d;
	}
	```
- 压缩：结合 `compression` 中间件压缩静态文件。
	```bash
	npm install compression
	```
	```js
	const compression = require('compression');
	app.use(compression());
	app.use(express.static('public'));
	```
### 注意事项
---
- 路径安全性：确保静态文件目录不包含敏感文件（如 `.env`），`express.static` 不会提供点文件，但需检查目录权限。
- 404 处理：`express.static` 不处理不存在的文件，需添加自定义 404 路由。
	```js
	app.use(express.static('public'));
	app.use((req, res) => res.status(404).send('File not found'));
	```
- 性能：大量静态文件可能影响性能，建议使用 CDN 或反向代理。
- 目录索引：默认情况下，访问目录会返回 `index.html`，可通过 `index` 选项禁用。
- 大小写敏感：文件路径区分大小写，需确保客户端请求与文件系统一致。
# 配置
---
## 数据库集成
---



## 部署与生产
---




# 思考
---



# 附录
---
> [!website]
    > 

> [!code]
    > 

> [!video]
    > 

> [!doc]
    > 

