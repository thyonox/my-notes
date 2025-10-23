![[jQuery.png]]
# 概述
---
## 简介
---
- jQuery 是一个快速、轻量级的 JavaScript 库，旨在简化 HTML 文档操作、事件处理、动画和 Ajax 交互。
- 它通过简洁的 API 提供跨浏览器兼容性，允许开发者使用 CSS 选择器轻松操作 DOM，支持链式调用、事件绑定和动态效果。
- jQuery 广泛应用于快速开发和维护老项目，尽管现代框架如 React 或 Vue 在复杂应用中更受欢迎。
- 核心特点包括强大的选择器、简化的 Ajax、丰富的插件生态和易用的动画方法。
## 需求
---
- **简化 DOM 操作**
	- 问题：原生 JavaScript 的 DOM 操作繁琐，代码冗长（如 `document.getElementById`）。
	- jQuery 优势：提供简洁的 API，如 `$('#id')` 选择元素，`text()`、`html()` 修改内容，链式调用更高效。
		```js
		// 原生 JS
		document.getElementById('myId').innerHTML = 'Hello';
		// jQuery
		$('#myId').html('Hello');
		```
- **跨浏览器兼容性**
	- 问题：早期浏览器（如 IE6-8、Firefox）对 JavaScript 和 CSS 支持不一致，开发需处理大量兼容性代码。
	- jQuery 优势：封装了浏览器差异，统一 API（如事件处理、Ajax），开发者无需单独适配。
		```js
		// jQuery 事件绑定，跨浏览器一致
		$('#btn').on('click', function() { /* 逻辑 */ });
		```
- **简化事件处理**
	- 问题：原生事件绑定（如 `addEventListener` 或 IE 的 `attachEvent`）复杂，且需处理浏览器差异。
	- jQuery 优势：提供统一的事件绑定方法（如 `click()`、`on()`）和事件委托，简化动态元素处理。
		```js
		$('#list').on('click', 'li', function() { /* 动态元素处理 */ });
		```
- **简化的 Ajax 请求**
	- 问题：原生 `XMLHttpRequest` 使用复杂，需手动处理请求、响应和错误。
	- jQuery 优势：提供 `$.ajax()`、`$.get()`、`$.post()` 等简洁方法，支持 JSON、XML 等格式。
		```js
		$.get('data.json', function(data) { console.log(data); });
		```
- **动画与效果**
	- 问题：原生 JavaScript 实现动画需手动操作 CSS 和定时器，代码复杂。
	- jQuery 优势：内置动画方法（如 `fadeIn()`、`animate()`），快速实现视觉效果。
		```js
		$('#box').fadeIn(500).animate({ width: '200px' }, 1000);
		```
- **丰富的插件生态**
	- 问题：开发复杂功能（如日历、轮播图）需从头编写。
	- jQuery 优势：庞大插件生态（如 jQuery UI、DataTables），快速扩展功能，节省开发时间。
- **代码简洁与开发效率**
	- 问题：原生 JavaScript 代码量大，开发和维护成本高。
	- jQuery 优势：API 直观，链式调用减少代码量，适合快速开发和原型设计。
		```js
		$('p').addClass('highlight').fadeIn(500).text('Updated');
		```
- **历史背景与普及性**
	- 背景：jQuery 诞生于 2006 年，当时 JavaScript 生态不成熟，框架（如 React、Vue）尚未出现，jQuery 填补了空白。
	- 普及性：广泛用于老项目（如 WordPress 插件），维护和兼容需求使其仍有价值。
## 对比
---

| 维度         | jQuery                          | 原生 JavaScript                   | React                         | Vue.js              | Angular                   |
| ---------- | ------------------------------- | ------------------------------- | ----------------------------- | ------------------- | ------------------------- |
| **定位**     | DOM 操作和交互简化库                    | 浏览器原生语言                         | 组件化 UI 库                      | 渐进式框架               | 全面框架                      |
| **学习曲线**   | 低，API 直观，适合初学者                  | 中高，需掌握 DOM 和浏览器 API             | 中高，需理解 JSX、状态管理               | 中，语法简单，易上手          | 高，需掌握 TypeScript 和框架概念    |
| **DOM 操作** | 强大，链式调用，简洁（如 `$('#id').hide()`） | 繁琐（如 `document.getElementById`） | 虚拟 DOM，声明式更新                  | 虚拟 DOM，响应式数据绑定      | 虚拟 DOM，指令式绑定              |
| **性能**     | 中等，复杂操作可能慢于原生                   | 最高，无额外开销                        | 高，虚拟 DOM 优化渲染                 | 高，虚拟 DOM 高效         | 高，但框架较重                   |
| **浏览器兼容性** | 优秀，内置兼容性处理                      | 需手动处理（如事件监听）                    | 依赖 polyfill，现代浏览器为主           | 现代浏览器为主             | 现代浏览器为主                   |
| **事件处理**   | 简洁，支持委托（如 `.on()`）              | 复杂，需处理浏览器差异                     | 合成事件，统一管理                     | 简单，响应式事件绑定          | 复杂，指令式事件绑定                |
| **Ajax**   | 简单（如 `$.ajax`、`$.get`）          | 需用 `fetch` 或 `XMLHttpRequest`   | 依赖 `fetch` 或库（如 Axios）        | 依赖 `fetch` 或 Axios  | 内置 HttpClient 模块          |
| **动画效果**   | 内置丰富（如 fadeIn、animate）          | 需 CSS 或手动 JS 实现                 | 依赖 CSS 或库（如 React Transition） | 内置过渡组件，依赖 CSS       | 内置动画模块                    |
| **组件化**    | 无，基于 DOM 操作                     | 无，需手动组织代码                       | 强大，组件复用                       | 强大，单文件组件            | 强大，模块化组件                  |
| **状态管理**   | 无，需手动管理                         | 无，需手动管理                         | 内置（如 useState），或 Redux        | 内置（如 Vuex/Pinia）    | 内置（如 RxJS、服务）             |
| **生态系统**   | 插件丰富（如 jQuery UI）               | 无统一生态，依赖社区                      | 丰富（React Router、Redux 等）      | 丰富（Vue Router、Vuex） | 全面（NgRx、Angular Material） |
| **文件大小**   | 小（~80KB 压缩后）                    | 无额外大小                           | 中（~100KB + 依赖）                | 中（~20KB 核心）         | 大（~500KB+）                |
| **适用场景**   | 快速开发、老项目维护、简单交互                 | 性能敏感项目、小型脚本                     | 复杂 SPA、动态 UI                  | 中小型 SPA、渐进式开发       | 大型企业级应用                   |
| **维护现状**   | 活跃但逐渐减少，现代项目用得少                 | 永久支持                            | 活跃，广泛使用                       | 活跃，社区增长             | 活跃，企业支持                   |

# 开始
---
## 安装
---
- **通过 CDN 引入**
	- 方式：直接在 HTML 文件中使用 `<script>` 标签引入 jQuery 的 CDN 链接。
	- 优点：无需下载，加载快，CDN 缓存可能提高性能。
	- 常用 CDN：
		- jQuery 官方：`https://code.jquery.com`
		- Google CDN：`https://ajax.googleapis.com/ajax/libs/jquery/`
		- jsDelivr：`https://cdn.jsdelivr.net`
	- 示例：
		```html
		<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
		```
		- 选择版本：推荐最新稳定版（如 3.6.0），`min.js` 为压缩版，适合生产环境。
- **下载本地文件引入**
	- 方式：从 jQuery 官网（`https://jquery.com/download/`）下载 jQuery 文件，保存到项目目录，再通过 `<script>` 引入。
	- 优点：离线可用，适合无网络环境或需控制版本。
	- 在 HTML 中引入。
- **通过包管理工具安装**
	- 方式：使用 npm、Yarn 或其他包管理工具安装 jQuery，适合模块化开发。
	- 步骤：
		- 安装：
			```bash
			npm install jquery
			yarn add jquery
			```
		- 在 JavaScript 文件中引入：
			```js
			import $ from 'jquery'; // ES 模块
			// 或
			const $ = require('jquery'); // CommonJS
			```
		- 或者在 HTML 中引入（需构建工具打包）：
			```html
			<script src="node_modules/jquery/dist/jquery.min.js"></script>
			```
	- 优点：适合现代前端项目，配合 Webpack、Vite 等工具使用。
	- 注意：确保项目有模块解析支持（如通过 `import` 或构建工具）。
- **验证安装**
	- 在 HTML 文件中添加测试代码：
		```html
		<script>
		  $(document).ready(function() {
		    console.log('jQuery is ready! Version:', $.fn.jquery);
		  });
		</script>
		```
	- 打开浏览器开发者工具（F12），检查控制台输出 jQuery 版本号。
## 使用
---
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jQuery Demo</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; }
        .highlight { background: yellow; padding: 5px; }
        #result { margin-top: 20px; border: 1px solid #ccc; padding: 10px; }
    </style>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
    <h1>jQuery Demo</h1>
    <p>This is a sample paragraph.</p>
    <button id="toggleColor">Toggle Color</button>
    <button id="addItem">Add List Item</button>
    <button id="loadData">Load Data</button>
    <ul id="itemList">
        <li>Item 1</li>
        <li>Item 2</li>
    </ul>
    <div id="result">Data will appear here...</div>

    <script>
        $(document).ready(function() {
            // 切换文本颜色
            $('#toggleColor').on('click', function() {
                $('p').toggleClass('highlight').css('color', $('p').hasClass('highlight') ? 'red' : 'black');
            });

            // 动态添加列表项
            $('#addItem').on('click', function() {
                $('#itemList').append('<li>New Item ' + ($('#itemList li').length + 1) + '</li>').children().last().fadeIn(500);
            });

            // Ajax 加载数据
            $('#loadData').on('click', function() {
                $.get('https://jsonplaceholder.typicode.com/posts/1', function(data) {
                    $('#result').html('<strong>Title:</strong> ' + data.title + '<br><strong>Body:</strong> ' + data.body).fadeIn(500);
                }).fail(function() {
                    $('#result').html('Failed to load data.').css('color', 'red');
                });
            });
        });
    </script>
</body>
</html>
```
# 概念
---
## 选择器
---
- **作用**：基于 CSS 选择器语法查找 DOM 元素，返回 jQuery 对象。
- **语法**：`$(selector)`，如 `$('p')` 选择所有 `<p>` 元素。
- **特点**：
	- 支持 CSS1-CSS3 选择器（部分 CSS4）。
	- 跨浏览器兼容，处理浏览器差异（如 IE）。
	- 可结合 jQuery 方法进行链式操作。
```js
$('p') // 所有 <p> 元素
$('#myId') // ID 为 myId 的元素
$('.myClass') // 所有 class="myClass" 的元素
$('*') // 所有 DOM 元素（慎用，性能低）
$('p, div, .myClass') // 选择 <p>、<div> 和 class="myClass" 的元素
$('div p') // <div> 内的所有 <p>
$('div > p') // <div> 的直接 <p> 子元素
$('h2 + p') // 紧跟 <h2> 后的 <p>
$('h2 ~ p') // <h2> 后的所有同级 <p>
...
```
## DOM操作
---
- **获取与设置内容**
	- `text()`：
		- 获取：返回元素文本内容。
		- 设置：`$('selector').text('新文本')`。
		- 示例：
			```js
			let text = $('p').text(); // 获取 p 元素文本
			$('p').text('新内容'); // 设置文本
			```
	- `html()`：
		- 获取：返回元素 HTML 内容。
		- 设置：`$('div').html('<b>新内容</b>')`。
		- 示例：
			```js
			let html = $('#myDiv').html(); // 获取 HTML
			$('#myDiv').html('<span>更新</span>'); // 设置 HTML
			```
	- `val()`：
		- 获取：返回表单元素的值。
		- 设置：`$('input').val('新值')`。
		- 示例：
			```js
			let value = $('input').val(); // 获取输入框值
			$('input').val('新输入'); // 设置值
			```
- **属性操作**
	- `attr()`：
		- 获取：`$('img').attr('src')`。
		- 设置：`$('img').attr('src', 'new.jpg')` 或 `$('img').attr({ src: 'new.jpg', alt: '新图片' })`。
	- `removeAttr()`：
		- 移除属性：`$('img').removeAttr('alt')`。
	- `prop()`：
		- 用于处理布尔属性（如 `checked`、`disabled`）。
		- 示例：
			```js
			$('input').prop('checked', true); // 设置复选框选中
			let isChecked = $('input').prop('checked'); // 获取选中状态
			```
	- 注意：`attr()` 操作 DOM 属性，`prop()` 操作 JavaScript 属性，布尔属性推荐用 `prop()`。
- **样式操作**
	- `css()`：
		- 获取：`$('div').css('color')`。
		- 设置：`$('div').css('color', 'blue')` 或 `$('div').css({ color: 'blue', fontSize: '16px' })`。
	- `addClass()`：添加类，如 `$('p').addClass('highlight')`。
	- `removeClass()`：移除类，如 `$('p').removeClass('highlight')`。
	- `toggleClass()`：切换类，有则移除，无则添加。
		```js
		$('p').toggleClass('active'); // 切换 active 类
		```
	- `hasClass()`：检查是否包含类，返回布尔值。
		```js
		let hasClass = $('p').hasClass('highlight'); // true/false
		```
- **DOM 增删改**
	- 添加元素：
		- `append()`：在元素内部末尾添加内容。
		- `prepend()`：在元素内部开头添加内容。
		- `after()`：在元素外部后面添加内容。
		- `before()`：在元素外部前面添加内容。
		```js
		$('#list').append('<li>新项</li>'); // 末尾添加
		$('#list').prepend('<li>首项</li>'); // 开头添加
		$('p').after('<div>后插入</div>'); // 外部后插入
		```
	- 删除元素：
		- `remove()`：删除选中元素及其子元素。
		- `empty()`：清空元素内容，但保留元素本身。
		```js
		$('#myDiv').remove(); // 删除 div 及其内容
		$('#myDiv').empty(); // 清空 div 内容
		```
	- 替换元素：
		- `replaceWith()`：用新内容替换选中元素。
		- `replaceAll()`：替换目标元素。
		```js
		$('p').replaceWith('<span>新内容</span>'); // 替换 p 为 span
		```
- **DOM 遍历**
	- 查找：
		- `find()`：查找后代元素，如 `$('div').find('p')`。
		- `children()`：查找直接子元素，如 `$('ul').children('li')`。
	- 过滤：
		- `filter()`：筛选符合条件的元素，如 `$('li').filter('.active')`。
		- `not()`：排除符合条件的元素，如 `$('li').not('.active')`。
		- `has()`：筛选包含指定后代元素的元素，如 `$('ul').has('li.active')`。
		- `is()`：判断元素是否匹配选择器或条件，返回布尔值，如 `$('#myId').is(':visible')`。
	- 定位：
		- `first()`、`last()`：获取第一个/最后一个元素。
		- `eq(index)`：获取指定索引的元素，如 `$('li').eq(1)`。
	- 父子兄弟：
		- `parent()`：父元素。
		- `parents()`：所有祖先元素。
		- `siblings()`：兄弟元素。
		```js
		$('#item').parent().css('border', '1px solid'); // 父元素加边框
		$('#item').siblings().hide(); // 隐藏兄弟元素
		```
- **尺寸与位置**
	- 尺寸：
		- `width()`、`height()`：获取/设置元素宽高。
		- `innerWidth()`、`innerHeight()`：包含内边距。
		- `outerWidth()`、`outerHeight()`：包含边框和外边距（加 `true` 包含 margin）。
		```js
		let w = $('div').width(); // 获取宽度
		$('div').height(100); // 设置高度
		```
	- 位置：
		- `offset()`：获取/设置相对于文档的坐标 `{ top: x, left: y }`。
		- `position()`：获取相对于父元素的坐标（只读）。
		```js
		let pos = $('#box').offset(); // 获取坐标
		$('#box').offset({ top: 100, left: 200 }); // 设置坐标
		```
- **性能优化建议**
	- 缓存选择器：避免重复选择 DOM，保存到变量。
		```js
		let $elem = $('#myDiv'); // 缓存
		$elem.css('color', 'red').text('更新');
		```
	- 批量操作：使用单一选择器或链式操作，减少 DOM 访问。
	- 避免复杂选择器：如 `$('div p span')`，尽量用简单选择器。
## 事件处理
---
- **事件绑定方法**
	- `.on(event, [selector], handler)`：通用事件绑定方法，支持动态元素和事件委托。
		- 参数：
			- `event`：事件类型（如 `click`, `mouseover`）。
			- `selector`（可选）：用于事件委托，匹配子元素。
			- `handler`：事件处理函数。
		- 示例：
			```js
			$('#list').on('click', 'li', function() {
			  console.log($(this).text()); // 点击动态添加的 li 触发
			});
			```
	- 快捷方法：为常见事件提供简写，如：
		- `click()`、`dblclick()`、`mouseover()`、`mouseout()`、`change()`、`submit()` 等。
		- 示例：
			```js
			$('#btn').click(function() {
			  console.log('Clicked!');
			});
			```
	- 多事件绑定：用 `.on()` 绑定多个事件。
		```js
		$('#elem').on('click mouseover', function() {
		  console.log('Event triggered!');
		});
		```
- **事件委托**
	- 作用：将事件绑定到父元素，监听动态添加的子元素，节省内存且支持动态 DOM。
	- 示例：
		```js
		$('#container').on('click', '.dynamic', function() {
		  console.log('Dynamic element clicked!');
		});
		```
	- 优势：无需为每个新添加的子元素重新绑定事件。
- **事件对象**
	- jQuery 规范化事件对象，跨浏览器一致。
	- 常用属性/方法：
		- `event.target`：触发事件的元素。
		- `event.type`：事件类型（如 `click`）。
		- `event.preventDefault()`：阻止默认行为（如表单提交）。
		- `event.stopPropagation()`：阻止事件冒泡。
		- `event.pageX/event.pageY`：鼠标位置。
	- 示例：
		```js
		$('a').click(function(event) {
		  event.preventDefault(); // 阻止链接跳转
		  console.log(event.type); // 输出: click
		});
		```
- **解绑事件**
	- `.off(event, [selector], [handler])`：移除事件监听。
		```js
		$('#btn').off('click'); // 移除所有 click 事件
		$('#list').off('click', 'li'); // 移除委托事件
		```
	- 移除特定处理函数：
		```js
		function myHandler() { console.log('Handler'); }
		$('#btn').on('click', myHandler);
		$('#btn').off('click', myHandler); // 只移除 myHandler
		```
- **触发事件**
	- `.trigger(event)`：手动触发事件。
		```js
		$('#btn').trigger('click'); // 模拟点击
		```
	- 快捷触发：如 `$('#btn').click()`。
	- 自定义事件：
		```js
		$('#elem').on('myEvent', function() { console.log('Custom event'); });
		$('#elem').trigger('myEvent');
		```
- **事件命名空间**
	- 作用：为事件添加命名空间，便于管理。
		```js
		$('#btn').on('click.myNamespace', function() {
		  console.log('Namespaced event');
		});
		$('#btn').off('click.myNamespace'); // 只移除该命名空间的事件
		```
- **常用事件类型**
	- 鼠标事件：`click`, `dblclick`, `mouseover`, `mouseout`, `mouseenter`, `mouseleave`, `mousemove`.
	- 键盘事件：`keydown`, `keypress`, `keyup`.
	- 表单事件：`submit`, `change`, `focus`, `blur`.
	- 窗口/文档事件：`load`, `resize`, `scroll`, `ready`.
	```js
	$(document).ready(function() { // DOM 加载完成
	  console.log('DOM ready!');
	});
	```
- **特殊事件方法**
	- `.hover(handlerIn, handlerOut)`：快捷处理鼠标悬停。
		```js
		$('#elem').hover(
		  function() { $(this).addClass('hover'); },
		  function() { $(this).removeClass('hover'); }
		);
		```
	- `.one(event, handler)`：事件只触发一次。
		```js
		$('#btn').one('click', function() {
		  console.log('Only triggers once!');
		});
		```
- **事件性能优化**
	- 事件委托：减少绑定，适合动态元素。
	- 缓存选择器：避免重复查询 DOM。
	- 避免过多绑定：用命名空间或 `.off()` 清理无用事件。
## 动画与效果
---
- **基本效果方法**
	- `show(speed, easing, callback)`：显示元素。
	    - `speed`：动画持续时间（毫秒，如 500，或 `"slow"`、`"fast"`）。
	    - `easing`：动画缓动效果（默认 `"swing"`，可选 `"linear"`）。
	    - `callback`：动画完成后的回调函数。
	- `hide(speed, easing, callback)`：隐藏元素。
	- `toggle(speed, easing, callback)`：切换显示/隐藏状态。
	- 示例：
		```js
		$('#box').show(500); // 500ms 内显示
		$('#box').toggle('fast', function() { console.log('Toggled'); });
		```
- **淡入淡出效果**
	- 用于平滑改变元素透明度。
	- `fadeIn(speed, easing, callback)`：淡入显示。
	- `fadeOut(speed, easing, callback)`：淡出隐藏。
	- `fadeToggle(speed, easing, callback)`：切换淡入/淡出。
	- `fadeTo(speed, opacity, easing, callback)`：调整到指定透明度（0 到 1）。
	- 示例：
		```js
		$('#box').fadeOut(1000); // 1秒淡出
		$('#box').fadeTo('slow', 0.5); // 透明度变为 0.5
		```
- **滑动效果**
	- 通过调整元素高度实现滑动效果。
	- `slideDown(speed, easing, callback)`：向下滑动显示。
	- `slideUp(speed, easing, callback)`：向上滑动隐藏。
	- `slideToggle(speed, easing, callback)`：切换滑动效果。
	- 示例：
		```js
		$('#panel').slideToggle(500); // 500ms 切换滑动
		```
- **自定义动画**
	- `animate(properties, options)`：自定义 CSS 属性动画。
		- `properties`：目标 CSS 属性（只支持数值属性，如 `width`、`opacity`）。
		- `options`：对象，包含 `duration`（时长）、`easing`（缓动）、`complete`（回调）、`step`（每帧回调）。
	- 示例：
		```js
		$('#box').animate({
		  width: '200px',
		  opacity: 0.5,
		  marginLeft: '+=50px'
		}, {
		  duration: 1000,
		  easing: 'swing',
		  complete: function() { console.log('Animation done'); }
		});
		```
	- 注意：
		- 仅支持数值型 CSS 属性（如 `width`、`left`，不支持 color）。
		- 使用 `+=` 或 `-=` 表示相对值。
- **动画控制**
	- `stop(clearQueue, jumpToEnd)`：停止当前动画。
		- `clearQueue`：是否清除动画队列（默认 `false`）。
		- `jumpToEnd`：是否跳到动画结束状态（默认 `false`）。
		```js
		$('#box').stop(true, true); // 停止所有动画并跳到结束状态
		```
	- `delay(duration)`：延迟执行后续动画。
		```js
		$('#box').fadeOut(500).delay(1000).fadeIn(500); // 淡出后延迟 1秒再淡入
		```
	- `finish(queue)`：完成指定队列中的所有动画。
		```js
		$('#box').finish(); // 立即完成所有动画
		```
- **动画队列**
	- jQuery 默认将动画加入队列，顺序执行。
	- 查看队列：`queue()` 获取动画队列。
	- 添加自定义队列：
		```js
		$('#box').queue(function(next) {
		  console.log('Custom step');
		  next(); // 继续执行队列
		});
		```
	- 清空队列：`clearQueue()`。
- **全局动画设置**
	- `jQuery.fx.off`：禁用所有动画（直接跳到结束状态）。
		```js
		jQuery.fx.off = true; // 禁用动画
		```
	- `jQuery.fx.interval`：设置动画帧率（默认 13ms）。
		```js
		jQuery.fx.interval = 10; // 提高帧率
		```
- **性能优化**
	- 减少重绘：尽量合并动画，避免多次操作 DOM。
	- 缓存选择器：`let $box = $('#box');` 提高性能。
	- 避免复杂动画：多重嵌套动画可能导致卡顿。
	- 使用 CSS 动画替代：现代开发中，CSS 动画（如 `transition` 或 `@keyframes`）性能更优。
## Ajax
---
- **核心方法**
	- `$.ajax()`
	- 描述：最灵活的 AJAX 方法，支持详细配置。
	- 语法：
		```js
		$.ajax({
		  url: 'server.php', // 请求地址
		  method: 'GET', // 或 POST、PUT 等
		  data: { key: 'value' }, // 发送的数据
		  dataType: 'json', // 预期返回数据类型（json、html、xml 等）
		  success: function(response) { console.log('Success:', response); }, // 成功回调
		  error: function(xhr, status, error) { console.log('Error:', error); }, // 失败回调
		  complete: function() { console.log('Request completed'); } // 请求完成回调
		});
		```
	- 常用配置：
		- `url`：请求的 URL。
		- `method`：HTTP 方法（GET、POST 等）。
		- `data`：发送的数据（对象、字符串等）。
		- `dataType`：服务器返回的数据类型（自动解析）。
		- `async`：是否异步（默认 true）。
		- `timeout`：请求超时（毫秒）。
		- `headers`：自定义 HTTP 头。
		- `beforeSend`：请求发送前的回调。
	- 快捷方法
		- `$.get(url, data, success, dataType)`：GET 请求的简化版。
		- `$.post(url, data, success, dataType)`：POST 请求的简化版。
		- `$.getJSON(url, data, success)`：专门用于获取 JSON 数据。
		- `$.getScript(url, success)`：加载并执行 JavaScript 文件。
- **数据处理**
	- 发送数据：
		- 数据可以是对象（自动序列化为查询字符串）或字符串。
		```js
		$.ajax({
		  url: 'server.php',
		  data: { id: 1, name: 'Alice' }, // 序列化为 id=1&name=Alice
		  method: 'POST'
		});
		```
	- 接收数据：
		- `dataType` 指定返回格式（如 `json`、`html`、`xml`）。
		- jQuery 自动解析 JSON 数据为 JavaScript 对象。
- **全局 AJAX 事件**
	- 方法：
		- `ajaxStart()`：请求开始时触发。
		- `ajaxSend()`：发送请求前触发。
		- `ajaxSuccess()`：请求成功时触发。
		- `ajaxError()`：请求失败时触发。
		- `ajaxComplete()`：请求完成时触发（无论成功或失败）。
		- `ajaxStop()`：所有请求结束时触发。
	- 示例：
		```js
		$(document).ajaxStart(function() {
		  $('#loading').show(); // 显示加载动画
		}).ajaxStop(function() {
		  $('#loading').hide(); // 隐藏加载动画
		});
		```
- **错误处理**
	- 错误回调：`error(xhr, status, error)` 提供错误信息。
		- `xhr`：XMLHttpRequest 对象，包含状态码等。
		- `status`：错误类型（如 "timeout"、"error"）。
		- `error`：错误描述。
	- 示例：
		```js
		$.ajax({
		  url: 'invalid-url',
		  error: function(xhr, status, error) {
		    console.log(`Status: ${xhr.status}, Error: ${error}`);
		  }
		});
		```
- **Promise 支持**
	- `$.ajax()` 返回 Promise 对象，支持 `.then()` 和 `.catch()`。
		```js
		$.ajax({ url: 'data.json' })
		  .then(function(data) { console.log('Success:', data); })
		  .catch(function(err) { console.log('Error:', err); });
		```
- **跨域请求**
	- JSONP：通过 `<script>` 标签实现跨域（仅支持 GET）。
		```js
		$.ajax({
		  url: 'http://api.example.com/data',
		  dataType: 'jsonp',
		  success: function(data) { console.log(data); }
		});
		```
	- CORS：依赖服务器支持跨域头，配置 `$.ajax` 的 `xhrFields`。
		```js
		$.ajax({
		  url: 'http://api.example.com/data',
		  xhrFields: { withCredentials: true } // 发送凭据
		});
		```
- **实用工具**
	- 序列化表单：
		- `$('#form').serialize()`：将表单数据序列化为查询字符串。
		- `$('#form').serializeArray()`：返回表单数据的数组。
		```js
		$.post('submit.php', $('#form').serialize(), function(response) {
		  console.log(response);
		});
		```
	- 全局配置：
		- `$.ajaxSetup()`：设置默认 AJAX 参数。
		```js
		$.ajaxSetup({
		  timeout: 5000,
		  dataType: 'json'
		});
		```
- **注意事项**
	- 性能：
	    - 避免频繁 AJAX 请求，考虑合并请求或缓存。
	    - 使用快捷方法（如 `$.get`）简化代码。
	- 安全性：
	    - 防止 XSS：验证服务器返回数据。
	    - JSONP 不安全，仅用于可信 API。
	- 现代替代：
	    - 原生 `fetch()` API 更轻量，现代浏览器支持良好。
	    - 考虑使用 Axios 等库，API 更现代化。
## 遍历
---
- `.each()`
	- 遍历 jQuery 对象集合，执行回调函数。
	- 语法：`$(selector).each(function(index, element) {})`。
	- 参数：index（索引）、element（DOM 元素）。
	- 示例：
		```js
		$('li').each(function(index, element) {
		  console.log(index, $(element).text()); // 打印索引和文本
		});
		```
- `$.each()`
	- 遍历数组或对象（非 jQuery 对象）。
	- 语法：`$.each(array/object, function(index/key, value) {})`。
	- 示例：
		```js
		let arr = ['a', 'b', 'c'];
		$.each(arr, function(index, value) {
		  console.log(index, value); // 打印 0:a, 1:b, 2:c
		});
		```
- `.map()`
	- 遍历并生成新数组，基于回调返回值。
	- 语法：`$(selector).map(function(index, element) { return value; })`。
	- 示例：
		```js
		let texts = $('li').map(function(index, element) {
		  return $(element).text();
		}).get(); // 返回 ['text1', 'text2', ...]
		```
		- 使用 `.get()` 获取普通数组。
## 插件
---
- **插件定义**
	- 定义：jQuery 插件是扩展 jQuery 原型对象（`$.fn`）或 jQuery 全局对象（`$`）的方法，用于封装可复用的功能。
	- 作用：提供特定功能，如轮播图（Slider）、表单验证、模态框等，减少重复编码。
	- 常见插件：jQuery UI、DataTables、Slick Slider 等。
- **插件分类**
	- UI 插件：增强用户界面，如 jQuery UI（拖拽、对话框）。
	- 功能插件：特定功能，如表单验证（jQuery Validation）。
	- 工具插件：全局工具方法，如 `$.ajax` 扩展。
- **使用插件**
	- 引入插件：
		- 引入 jQuery 核心库。
		- 引入插件文件（通常是 `.js` 文件，部分需要 `.css`）。
		```html
		<script src="jquery.min.js"></script>
		<script src="jquery.plugin-name.min.js"></script>
		```
	- 调用插件：
		- 针对 jQuery 对象：`$('#element').pluginName(options)`;
		- 全局方法：`$.pluginName(options)`;
		```js
		$('#mySlider').slick({ slidesToShow: 3, autoplay: true }); // 调用 Slick 插件
		```
	- 配置选项：插件通常提供可配置参数，如速度、样式等。
		```js
		$('#myForm').validate({
		  rules: { name: "required" },
		  messages: { name: "请输入姓名" }
		});
		```
- **开发插件**
	- 基本结构：
		- 扩展 `$.fn`（针对 jQuery 对象）或 `$`（全局方法）。
		- 使用闭包避免变量冲突。
		```js
		(function($) {
		  $.fn.myPlugin = function(options) {
		    return this.each(function() {
		      // 插件逻辑
		      $(this).css('color', 'green');
		    });
		  };
		})(jQuery);
		```
	- 调用示例：
		```js
		$('p').myPlugin(); // 所有 p 元素变绿色
		```
	- 添加选项：
		- 使用 `$.extend` 合并默认和用户选项。
			```js
			(function($) {
			  $.fn.myPlugin = function(options) {
			    var settings = $.extend({
			      color: 'blue',
			      fontSize: '16px'
			    }, options);
			    return this.each(function() {
			      $(this).css({
			        color: settings.color,
			        fontSize: settings.fontSize
			      });
			    });
			  };
			})(jQuery);
			```
		- 调用：
			```js
			$('p').myPlugin({ color: 'red', fontSize: '20px' });
			```
	- 添加方法：
		- 插件支持多种方法（如 `init`, `destroy`）。
			```js
			(function($) {
			  $.fn.myPlugin = function(options) {
			    if (typeof options === 'string') {
			      // 处理方法调用
			      if (options === 'destroy') {
			        return this.each(function() {
			          $(this).removeClass('my-plugin');
			        });
			      }
			    } else {
			      // 初始化
			      var settings = $.extend({ color: 'blue' }, options);
			      return this.each(function() {
			        $(this).addClass('my-plugin').css('color', settings.color);
			      });
			    }
			  };
			})(jQuery);
			```
		- 调用：
			```js
			$('p').myPlugin({ color: 'red' }); // 初始化
			$('p').myPlugin('destroy'); // 销毁
			```
	- 全局工具方法：
		```js
		(function($) {
		  $.myGlobalFunction = function() {
		    console.log('Global function called');
		  };
		})(jQuery);
		```
		- 调用：`$.myGlobalFunction();`
- **插件规范**
	- 命名：
	    - 文件名：`jquery.plugin-name.js`（如 `jquery.myplugin.js`）。
	    - 避免冲突：使用独特前缀或命名空间。
	- 闭包：用 `(function($) { ... })(jQuery);` 防止全局变量污染。
	- 链式调用：返回 `this` 支持链式操作。
	- 兼容性：测试跨浏览器兼容性。
	- 文档：提供使用说明、选项列表和示例。
- **事件与状态管理**
	- 绑定事件：使用 `on()` 绑定事件，支持动态元素。
		```js
		$.fn.myPlugin = function() {
		  return this.each(function() {
		    $(this).on('click', function() {
		      $(this).toggleClass('active');
		    });
		  });
		};
		```
	- 状态保存：使用 `data()` 存储插件状态。
		```js
		$.fn.myPlugin = function() {
		  return this.each(function() {
		    $(this).data('plugin-state', { initialized: true });
		  });
		};
		```
- **优化与注意事项**
	- 性能：
	    - 缓存选择器：`let $this = $(this);`。
	    - 避免复杂 DOM 操作。
	- 销毁机制：提供 `destroy` 方法清理事件和样式。
	- 冲突处理：
	    - 使用 `jQuery.noConflict()` 解决 `$` 冲突。
		```js
		var $jq = jQuery.noConflict();
		$jq.fn.myPlugin = function() { ... };
		```
	- 调试：用 `console.log` 或开发者工具检查插件行为。
## 实用工具方法
---
- **字符串操作**
	- `$.trim(str)`：移除字符串首尾的空白字符（空格、制表符、换行符等）。
- **数组和对象操作**
	- `$.each(obj, callback)`：遍历数组或对象，callback 接收索引/键和值参数。
		```js
		$.each([1, 2, 3], function(index, value) {
		  console.log(index, value); // 输出: 0 1, 1 2, 2 3
		});
		$.each({ a: 1, b: 2 }, function(key, value) {
		  console.log(key, value); // 输出: a 1, b 2
		});
		```
	- `$.map(arrayOrObject, callback)`：遍历数组或对象，返回新数组，callback 可修改元素。
		```js
		let arr = $.map([1, 2, 3], function(value, index) {
		  return value * 2;
		}); // 输出: [2, 4, 6]
		```
	- `$.inArray(value, array, [fromIndex])`：查找值在数组中的索引，未找到返回 -1。
		```js
		let arr = ['a', 'b', 'c'];
		console.log($.inArray('b', arr)); // 输出: 1
		console.log($.inArray('d', arr)); // 输出: -1
		```
	- `$.merge(array1, array2)`：合并两个数组，修改第一个数组。
		```js
		let arr1 = [1, 2];
		let arr2 = [3, 4];
		$.merge(arr1, arr2); // arr1 变为 [1, 2, 3, 4]
		```
	- `$.extend([deep], target, object1, [objectN])`：合并多个对象到目标对象，deep 为 true 时支持深拷贝。
		```js
		let obj1 = { a: 1 };
		let obj2 = { b: 2 };
		$.extend(obj1, obj2); // obj1 变为 { a: 1, b: 2 }
		// 深拷贝
		let obj3 = { c: { d: 1 } };
		let target = $.extend(true, {}, obj3); // 深拷贝 obj3
		```
- **类型判断**
	- `$.isArray(obj)`：判断是否为数组。
		```js
		console.log($.isArray([1, 2])); // true
		console.log($.isArray({ a: 1 })); // false
		```
	- `$.isFunction(obj)`：判断是否为函数。
		```js
		console.log($.isFunction(function() {})); // true
		console.log($.isFunction({})); // false
		```
	- `$.isEmptyObject(obj)`：判断对象是否为空（无自有属性）。
		```js
		console.log($.isEmptyObject({})); // true
		console.log($.isEmptyObject({ a: 1 })); // false
		```
	- `$.isPlainObject(obj)`：判断是否为普通对象（通过 `{}` 或 `new Object()` 创建）。
		```js
		console.log($.isPlainObject({ a: 1 })); // true
		console.log($.isPlainObject(new Date())); // false
		```
	- `$.isNumeric(value)`：判断是否为数字（包括字符串形式的数字）。
		```js
		console.log($.isNumeric(123)); // true
		console.log($.isNumeric("123")); // true
		console.log($.isNumeric("abc")); // false
		```
- **数据解析与序列化**
	- `$.parseJSON(string)`：将 JSON 字符串解析为 JavaScript 对象（jQuery 3.0+ 推荐用 `JSON.parse`）。
		```js
		let obj = $.parseJSON('{"a": 1}'); // 输出: { a: 1 }
		```
	- `$.param(obj)`：将对象或数组序列化为 URL 查询字符串。
		```js
		let obj = { name: "John", age: 30 };
		console.log($.param(obj)); // 输出: name=John&age=30
		```
- **其他实用方法**
	- `$.now()`：返回当前时间戳，等同于 `Date.now()`。
		```js
		console.log($.now()); // 输出: 当前时间戳
		```
	- `$.grep(array, callback)`：过滤数组，返回符合条件的元素。
		```js
		let arr = $.grep([1, 2, 3, 4], function(num) {
		  return num % 2 === 0;
		}); // 输出: [2, 4]
		```
	- `$.makeArray(obj)`：将类数组对象（如 jQuery 对象或 NodeList）转换为数组。
		```js
		let nodes = $('li');
		let arr = $.makeArray(nodes); // 转换为数组
		```
	- `$.proxy(fn, context)`：改变函数的 `this` 指向。
		```js
		let obj = { name: "John" };
		let fn = $.proxy(function() { console.log(this.name); }, obj);
		fn(); // 输出: John
		```

# 配置
---




# 思考
---



# 附录
---
- **官网**：[https://jquery.com/](https://jquery.com/)
- **API文档**：[https://api.jquery.com/](https://api.jquery.com/)
- **jQuery Learning Center**：[https://learn.jquery.com/](https://learn.jquery.com/)
- **jQuery Plugin Registry**：[https://plugins.jquery.com/](https://plugins.jquery.com/)
- **jQuery Script**：[https://www.jqueryscript.net/](https://www.jqueryscript.net/)

