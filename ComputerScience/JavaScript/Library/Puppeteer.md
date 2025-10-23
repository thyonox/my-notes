![[Puppeteer.png]]
# 概述
---
## 简介
---
- Puppeteer 是一个由 Google Chrome 团队开发的 Node.js 库，用于通过 DevTools 协议控制 headless Chrome 或 Chromium 浏览器。
- 它提供高级 API，适用于网页自动化、爬取、截图、PDF 生成及自动化测试等场景。
## 需求
---
1. **动态网页爬取问题**：
    - **问题**：现代网页大量使用 JavaScript（如 React、Vue、Angular）动态渲染内容，传统爬虫（如 requests 或 BeautifulSoup）无法获取完整页面数据。
    - **解决**：Puppeteer 运行完整浏览器环境，等待页面渲染完成，抓取动态内容，甚至能执行页面内的 JavaScript 逻辑。
2. **复杂的用户交互自动化**：
    - **问题**：需要模拟用户行为（如填写表单、点击按钮、滚动页面），传统工具配置复杂或无法精确模拟。
    - **解决**：Puppeteer 提供简单 API（如 page.click()、page.type()），精确模拟用户操作，支持复杂交互场景，如登录、文件上传、处理弹窗等。
3. **生成高质量网页快照**：
    - **问题**：需要生成网页截图或 PDF（如生成报告、存档页面），传统工具难以保证渲染一致性。
    - **解决**：Puppeteer 能生成高保真截图（page.screenshot()）和 PDF（page.pdf()），支持自定义分辨率、页面大小等。
4. **自动化测试的效率问题**：
    - **问题**：传统 UI 测试工具（如 Selenium）配置复杂，运行速度慢，维护成本高。
    - **解决**：Puppeteer 轻量、运行快速，与 Node.js 生态无缝集成，适合端到端测试和功能验证。
5. **性能分析与优化**：
    - **问题**：开发者需要分析网页加载时间、网络请求等性能指标，缺乏简单工具。
    - **解决**：Puppeteer 支持捕获性能指标（page.metrics()）、拦截请求（page.setRequestInterception()），帮助优化网页性能。
6. **反爬机制的绕过**：
    - **问题**：许多网站通过 JavaScript 检测、浏览器指纹等方式阻止爬虫。
    - **解决**：Puppeteer 使用真实浏览器环境，模拟真实用户行为（如设置 User Agent、视口），结合插件（如 puppeteer-extra-plugin-stealth）可有效绕过反爬机制。
7. **服务器环境兼容性**：
    - **问题**：在无图形界面的服务器上运行浏览器自动化任务困难。
    - **解决**：Puppeteer 的 headless 模式专为服务器环境设计，资源占用低，支持 Linux 容器（如 Docker）。
## 对比
---

| **工具**         | **语言**                      | **浏览器支持**                 | **Headless ** | **主要用途**         | **性能** | **难度** |
| -------------- | --------------------------- | ------------------------- | ------------- | ---------------- | ------ | ------ |
| **Puppeteer**  | Node.js                     | Chrome/Chromium           | 支持            | 爬虫、自动化、测试、截图/PDF | 高      | 中      |
| **Playwright** | Node.js, Python, .NET, Java | Chrome, Firefox, WebKit   | 支持            | 爬虫、跨浏览器测试、自动化    | 高      | 中      |
| **Selenium**   | 多语言 (Java, Python, JS 等)    | Chrome, Firefox, Safari 等 | 支持            | 跨浏览器测试、自动化       | 中      | 高      |
| **Cypress**    | JavaScript                  | Chrome, Firefox, Edge     | 不支持           | 端到端测试            | 高      | 中      |
| **Cheerio**    | Node.js                     | 无 (静态 HTML 解析)            | 不适用           | 静态网页爬虫           | 极高     | 低      |
- **Axios**：
    - axios 是一个基于 HTTP 的客户端库，直接发送 HTTP 请求（GET、POST 等）。
    - 请求头默认简单，缺少真实浏览器环境中的复杂特征（如完整的 User-Agent、Cookies、TLS 指纹等）。
    - Cloudflare 的 WAF 会检测请求是否来自真实浏览器，通过分析请求头、JavaScript 执行能力、浏览器指纹等来判断是否为爬虫。
    - 如果 axios 的请求缺少这些特征或行为模式（如未执行 JavaScript 挑战），Cloudflare 会返回 403 Forbidden 或要求完成 CAPTCHA 验证。
- **Puppeteer**：
    - Puppeteer 控制的是真实的 Chrome/Chromium 浏览器，运行完整的浏览器环境，包括 JavaScript 引擎、DOM 渲染和网络栈。
    - 它模拟了真实用户的浏览器行为，发送的请求包含完整的浏览器指纹（如 User-Agent、Accept-Language、WebGL 特性等）。
    - Cloudflare 的 JavaScript 挑战（如执行特定的脚本或检测浏览器环境）会被 Puppeteer 自动处理，因此能通过 WAF 检测。
# 开始
---
## 安装
---
- 安装 Puppeteer
	```bash
	npm install puppeteer
	```
	- 安装过程会自动下载 Chromium（约 150-300 MB，视平台而定）。
	- 如果只想安装库而不下载 Chromium，可用 `puppeteer-core`。
- 安装 puppeteer-core
	```bash
	npm install puppeteer-core
	```
	- 然后需手动提供 Chrome/Chromium 可执行路径。
## 使用
---
```js
const puppeteer = require('puppeteer');

(async () => {
  // 启动浏览器
  const browser = await puppeteer.launch({
    headless: true, // 无头模式
    args: ['--no-sandbox', '--disable-setuid-sandbox'], // Linux 服务器常用
  });

  // 打开新页面
  const page = await browser.newPage();

  // 设置视口大小
  await page.setViewport({ width: 1280, height: 720 });

  // 访问网页
  await page.goto('https://example.com', { waitUntil: 'networkidle2' });

  // 截图
  await page.screenshot({ path: 'example.png', fullPage: true });

  // 获取页面标题
  const title = await page.title();
  console.log('Page title:', title);

  // 关闭浏览器
  await browser.close();
})();
```
# 概念
---
## 核心功能
---
- **自动化操作**
    - **功能**：模拟用户行为，如点击、输入、滚动、导航等。
    - **解释**：通过 API（如 page.click()、page.type()）执行用户交互，适合自动化表单提交、页面测试等。
- **Headless 模式**
    - **功能**：运行无界面浏览器（headless: true），也可开启有界面模式（headless: false）。
    - **解释**：无头模式适合服务器环境，节省资源；有头模式便于调试。
- **网页截图与 PDF 生成**
    - **功能**：生成页面截图（page.screenshot()）或 PDF 文件（page.pdf()）。
    - **解释**：用于存档网页、生成报告或视觉测试，支持自定义分辨率、页面格式。
- **网页爬取**
    - **功能**：抓取动态网页内容，处理 JavaScript 渲染。
    - **解释**：通过 page.evaluate() 执行页面内 JavaScript，提取数据，适合爬取 SPA 应用。
- **性能分析**
    - **功能**：监控页面加载时间、网络请求等（如 page.metrics()）。
    - **解释**：帮助优化网页性能，分析资源加载瓶颈。
- **自动化测试**
    - **功能**：支持 UI 和端到端测试，验证网页功能。
    - **解释**：结合测试框架（如 Jest），自动化检查页面交互和渲染结果。
## 核心API
---
1. **Browser（浏览器实例）**
	- 管理浏览器及其页面、上下文。
	- `puppeteer.launch(options)`
		- **功能**：启动 Chromium 浏览器。
		- **参数**：
			- `headless: true/false`：是否无头模式（默认 true）。
			- `args: ['--no-sandbox']`：启动参数，服务器环境常用。
			- `defaultViewport: { width, height }`：默认视口大小。
		- **示例**：
			```js
			const browser = await puppeteer.launch({ headless: false });
			```
	- `browser.newPage()`
		- **功能**：创建新页面（标签页）。
		- **返回**：Page 实例。
		- **示例**：
			```js
			await browser.close();
			```
	- `browser.close()`
		- **功能**：关闭浏览器，释放资源。
		- **示例**：
			```js
			await browser.close();
			```
	- `browser.pages()`
		- **功能**：获取所有打开的页面。
		- **返回**：`Page[]` 数组。
		- **示例**：
			```js
			const pages = await browser.pages();
			```
2. **Page（页面实例）**
	- 控制具体页面的操作，如导航、DOM 交互、截图等。
	- `page.goto(url, options)`
		- **功能**：导航到指定 URL。
		- **参数**：
		    - `waitUntil`：等待条件（如 '`domcontentloaded`'、'`networkidle0`'）。
		    - `timeout`：超时时间（毫秒）。
		- **示例**：
			```js
			await page.goto('https://example.com', { waitUntil: 'networkidle2' });
			```
	- `page.$ / page.$$(selector)`
		- **功能**：查询 DOM 元素，类似 `querySelector` / `querySelectorAll`。
		- **返回**：`ElementHandle`（单个）或 `ElementHandle[]`（多个）。
		- **示例**：
			```js
			const link = await page.$('a');
			const links = await page.$$('a');
			```
	- `page.evaluate(fn, ...args)`
		- **功能**：在页面上下文中执行 JavaScript 函数，获取结果。
		- **参数**：
		    - `fn`：要执行的函数。
		    - `...args`：传递给函数的参数。
		- **示例**：
			```js
			const title = await page.evaluate(() => document.title);
			```
	- `page.waitForSelector(selector, options)`
		- **功能**：等待元素出现在 DOM 中。
		- **参数**：
			- `visible: true`：等待元素可见。
		    - `timeout`：超时时间。
		- **示例**：
			```js
			await page.waitForSelector('#submit', { visible: true });
			```
	- `page.click(selector, options)`
		- **功能**：点击指定元素。
		- **参数**：
		    - `delay`：点击延迟。
		    - `button`：鼠标按键（'`left`'、'`right`'）。
		- **示例**：
			```js
			await page.click('#submit');
			```
	- `page.type(selector, text, options)`
		- **功能**：在输入框中输入文本，模拟键盘。
		- **参数**：
		    - `delay`：按键间隔（毫秒）。
		- **示例**：
			```js
			await page.type('#username', 'user123');
			```
	- `page.screenshot(options)`
		- **功能**：生成页面截图。
		- **参数**：
		    - `path`：保存路径。
		    - `fullPage: true`：截取整个页面。
		    - `clip`：裁剪区域 `{ x, y, width, height }`。
		- **示例**：
			```js
			await page.screenshot({ path: 'screenshot.png', fullPage: true });
			```
	- `page.pdf(options)`
		- **功能**：生成页面 PDF。
		- **参数**：
		    - `path`：保存路径。
		    - `format`：纸张格式（如 'A4'）。
		    - `margin`：边距设置。
		- **示例**：
			```js
			await page.pdf({ path: 'output.pdf', format: 'A4' });
			```
	- `page.setRequestInterception(value)`
		- **功能**：启用/禁用网络请求拦截。
		- **示例**：
			```js
			await page.setRequestInterception(true);
			page.on('request', req => req.continue());
			```
	- `page.setUserAgent(userAgent)`
		- **功能**：设置浏览器的 `User Agent`。
		- **示例**：
			```js
			await page.setUserAgent('Mozilla/5.0 ...');
			```
	- `page.setViewport({ width, height })`
		- **功能**：设置页面视口大小。
		- **示例**：
			```js
			await page.setViewport({ width: 1280, height: 720 });
			```
3. **ElementHandle（DOM 元素操作）**
	- 表示页面中的 DOM 元素，用于交互。
	- `elementHandle.click(options)`
		- **功能**：点击元素。
		- **示例**：
			```js
			const button = await page.$('#submit');
			await button.click();
			```
	- `elementHandle.getProperty(propertyName)`
		- **功能**：获取元素属性值（如 value、href）。
		- **示例**：
			```js
			const href = await (await link.getProperty('href')).jsonValue();
			```
	- `elementHandle.type(text, options)`
		- **功能**：在元素中输入文本。
		- **示例**：
			```js
			const input = await page.$('#username');
			await input.type('user123');
			```
4. **事件监听**
	- `page.on(event, handler)`
		- **功能**：监听页面事件，如网络请求、弹窗、控制台输出等。
		- **常见事件**：
			- `request`：网络请求发起。
		    - `response`：网络响应返回。
		    - `dialog`：处理 `alert/confirm/prompt` 弹窗。
		    - `console`：捕获 `console.log` 输出。
		- **示例**：
			```js
			page.on('console', msg => console.log(msg.text()));
			page.on('dialog', async dialog => await dialog.accept());
			```
5. **其他重要 API**
	- `page.waitForTimeout(ms)`
		- **功能**：等待指定时间（毫秒）。
		- **示例**：
			```js
			await page.waitForTimeout(1000);
			```
	- `page.exposeFunction(name, fn)`
		- **功能**：将 Node.js 函数暴露到页面上下文中。
		- **示例**：
			```js
			await page.exposeFunction('myFunc', () => 'Hello');
			await page.evaluate(() => myFunc()); // 返回 'Hello'
			```
	- `browser.newBrowserContext()`
		- **功能**：创建新的浏览器上下文（类似隐身模式）。
		- **示例**：
			```js
			const context = await browser.newBrowserContext();
			const page = await context.newPage();
			```
### 注意事项
---
- **异步性**：Puppeteer API 基于 Promise，所有操作需用 await 或 .then()。
- **上下文隔离**：
    - page.evaluate 运行在浏览器上下文中，无法直接访问 Node.js 变量。
    - 使用 page.exposeFunction 或参数传递解决。
- **资源管理**：及时关闭页面（page.close()）和浏览器（browser.close()）以释放内存。
- **错误处理**：建议用 try-catch 捕获超时或导航错误。
## 常见用例
---
1. **网页爬取**：
    - 抓取动态渲染的网页内容（如 SPA 应用）。
    - 示例：提取页面标题或列表数据。
		```js
		const data = await page.evaluate(() => {
		  return Array.from(document.querySelectorAll('h1')).map(h => h.textContent);
		});
		```
2. **自动化测试**：
    - 进行 UI 或端到端测试，验证网页功能。
    - 示例：检查按钮点击后是否跳转正确页面。
		```js
		await page.click('#button');
		await page.waitForSelector('#result');
		```
3. **生成截图与 PDF**：
    - 捕获网页截图或生成 PDF，用于报告或存档。
    - 示例：生成页面 PDF。
		```js
		await page.pdf({ path: 'output.pdf', format: 'A4' });
		```
4. **模拟用户操作**：
    - 自动化表单填写、登录、点击等操作。
    - 示例：模拟登录。
		```js
		await page.type('#username', 'user');
		await page.type('#password', 'pass');
		await page.click('#login-button');
		```
5. **性能监控**：
    - 分析页面加载时间、网络请求等性能指标。
    - 示例：获取性能数据。
		```js
		const metrics = await page.metrics();
		console.log(metrics);
		```
6. **批量操作**：
    - 自动执行重复任务，如批量下载文件或提交表单。
    - 示例：上传文件。
		```js
		await page.setInputFiles('input[type="file"]', 'file.txt');
		```
7. **网络请求拦截**：
    - 阻止特定资源加载（如图片、广告），优化爬取效率。
    - 示例：拦截图片请求。
		```js
		await page.setRequestInterception(true);
		page.on('request', request => {
		  if (request.url().endsWith('.png')) request.abort();
		  else request.continue();
		});
		```





# 配置
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

