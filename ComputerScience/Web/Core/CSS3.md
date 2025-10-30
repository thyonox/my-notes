# CSS基础
---
## CSS简介与作用
---
- **简介**
	- CSS（Cascading Style Sheets，层叠样式表）是一种用于描述网页内容外观和格式的样式表语言。
	- 它与 HTML 结合使用，HTML 负责网页的结构和内容，CSS 则定义这些内容的视觉呈现方式，如颜色、字体、布局等。
	- CSS 由 W3C（万维网联盟）维护，最新版本为 CSS3，引入了许多新特性，如动画、响应式设计和高级布局。
- **作用**
	- 控制网页样式：CSS 用于设置网页元素的样式，包括字体、颜色、背景、边框、间距等，统一视觉效果。
	- 分离内容与表现：通过将样式（CSS）与结构（HTML）分离，提高代码可维护性和复用性。
	- 提升用户体验：通过动画、过渡和响应式设计，增强网页的交互性和视觉吸引力。
	- 适配不同设备：利用媒体查询和相对单位（如 rem、vw），实现响应式设计，确保网页在不同屏幕尺寸下良好显示。
	- 提高开发效率：CSS 框架（如 Bootstrap、Tailwind CSS）和预处理器（如 Sass）简化样式开发，加快项目进度。
	- 支持动态效果：通过伪类、伪元素和动画属性，创建动态交互效果，无需依赖 JavaScript。
	- 优化性能：合理使用 CSS（如减少重绘和回流）可提升网页加载速度和渲染效率。
## CSS引入方式
---
1. **内联样式（Inline CSS）**
	- 定义：将 CSS 样式直接写在 HTML 元素的 `style` 属性中。
	- 注意事项：
		- 直接嵌入 HTML 标签，优先级最高（权重 1000）。
		- 适合快速测试或单个元素的简单样式。
		- 代码维护困难，难以复用。
	- 示例：
		```css
		<p style="color: blue; font-size: 16px;">这是一个段落</p>
		```
2. **内部样式表（Internal CSS）**
	- 定义：将 CSS 写在 HTML 文件的 `<head>` 标签内的 `<style>` 标签中。
	- 注意事项：
		- 优先级低于内联样式（取决于选择器权重）。
		- 样式集中于页面，便于小规模管理。
		- 仅限于单个 HTML 文件，无法跨页面复用。
	- 示例：
		```css
		<head>
		  <style>
		    p {
		      color: green;
		      font-size: 14px;
		    }
		  </style>
		</head>
		<body>
		  <p>这是一个段落</p>
		</body>
		```
3. **外部样式表（External CSS）**
	- 定义：将 CSS 写在独立的 `.css` 文件中，通过 `<link>` 标签引入 HTML。
	- 注意事项：
		- 优先级取决于选择器权重。
		- 样式存储在单独文件，作用于多个页面。
		- 需要额外 HTTP 请求（可通过合并文件优化）。
	- 示例：
		```css
		<head>
		  <link rel="stylesheet" href="styles.css">
		</head>
		```
4. **导入样式（@import）**
	- 定义：在 CSS 文件或 `<style>` 标签中使用 `@import` 规则引入外部 CSS 文件。
	- 注意事项：
		- 允许在 CSS 中动态加载其他 CSS 文件。
		- 便于模块化管理 CSS 文件。
		- 加载顺序可能导致样式覆盖问题。
	- 示例：
		```css
		/* main.css */
		@import url("reset.css");
		p { color: purple; }
		```
## CSS语法结构
---
- **规则集**
	- CSS 由一个个规则集组成，每个规则集包含选择器和声明块。
	- 语法：
		```css
		selector {
		  property: value;
		  property: value;
		}
		```
		- `selector`：选择器，指定样式应用的 HTML 元素，多个选择器由逗号分隔。
		- `{ ... }`：声明块，包含一个或多个声明。
		- `property: value`：声明，指定样式属性和值。
- **注释**
	- 语法：使用 `/* */` 包裹注释内容。
	- 作用：添加代码说明，不影响样式渲染。
	- CSS 不支持单行 `//` 注释（与 JavaScript 不同）。
## 选择器类型
---
### 基础选择器
---
- **通用选择器**
	- 定义：匹配页面中的所有元素，包括 HTML 元素、文本节点等。
	- 用法：通常用于重置样式或应用全局样式。
	- 注意事项：
		- 性能较低，因为需要遍历所有元素。
		- 优先级最低（权重为 0）。
		- 谨慎使用，避免无意覆盖特定样式。
		- 常用于 CSS 重置样式表。
	- 示例：
		```css
		* {
		  margin: 0;
		  padding: 0;
		  box-sizing: border-box;
		}
		```
- **元素选择器**
	- 定义：根据 HTML 标签名选择所有匹配的元素。
	- 用法：直接使用标签名，适用于为特定类型的元素设置通用样式。
	- 注意事项：
		- 权重为 1，优先级较低，易被类或 ID 选择器覆盖。
		- 适合设置全局默认样式（如段落、标题等）。
		- 避免过于泛化，推荐结合类选择器提高特异性。
	- 示例：
		```css
		p {
		  color: #333;
		  font-size: 16px;
		}
		```
- **类选择器**
	- 定义：匹配具有指定 class 属性的元素，class 属性值前加点号（`.`）。
	- 用法：用于为多个元素应用相同样式，class 可重复使用。
	- 注意事项：
		- 权重为 10，高于元素选择器。
		- 元素可拥有多个 class（空格分隔），如 `class="btn btn-primary"`。
		- 推荐使用有意义的 class 名，遵循命名规范（如 BEM）。
		- 区分大小写，`.Btn` 和 `.btn` 是不同的选择器。
	- 示例：
		```css
		.btn {
		  background-color: blue;
		  color: white;
		  padding: 10px 20px;
		}
		```
- **ID 选择器**
	- 定义：匹配具有指定 ID 属性的唯一元素，ID 属性值前加井号（`#`）。
	- 用法：用于为页面中唯一元素设置样式。
	- 注意事项：
		- 权重为 100，高于类选择器。
		- ID 在页面中应唯一，重复 ID 会导致 HTML 验证错误。
		- 避免在 CSS 中过度使用 ID 选择器，因其高优先级可能导致样式难以覆盖。
		- 更推荐使用类选择器以提高代码可维护性。
	- 示例：
		```css
		#header {
		  font-size: 24px;
		  text-align: center;
		}
		```
- **属性选择器**
	- 定义：根据元素的属性或属性值选择元素，使用方括号（`[]`）定义。
	- 类型：
		- `[attr]`：匹配具有指定属性的元素。
		- `[attr=value]`：匹配属性值精确等于指定值的元素。
		- `[attr~=value]`：匹配属性值包含指定单词的元素（以空格分隔）。
		- `[attr|=value]`：匹配属性值等于指定值或以指定值开头（后接连字符）的元素。
		- `[attr^=value]`：匹配属性值以指定值开头的元素。
		- `[attr$=value]`：匹配属性值以指定值结尾的元素。
		- `[attr*=value]`：匹配属性值包含指定值的元素。
	- 注意事项：
		- 权重为 10，与类选择器相同。
		- 可结合其他选择器使用，如 `input[type="checkbox"]`。
		- 属性值区分大小写。
		- 在表单元素（如 `<input>`）或链接（如 `<a>`）中常用于精确样式控制。
		- 浏览器兼容性较好，但 IE6 对部分属性选择器支持有限。
	- 示例：
		```css
		[type="text"] {
		  border: 1px solid gray;
		}
		[href^="https"] {
		  color: green;
		}
		[class~="btn"] {
		  margin: 5px;
		}
		```
### 组合选择器
---
- **后代选择器( )**
	- 定义：匹配某个元素的所有后代元素（包括子元素、孙元素等）。
	- 语法：`选择器1 选择器2`
	- 注意事项：
		- 范围广，包含所有后代。
		- 性能较低，需谨慎使用深层嵌套。
		- 为容器内的所有特定元素设置统一样式。
	- 示例：
		```css
		div p { color: blue; }
		```
		- 匹配 `<div>` 内所有的 `<p>` 元素，无论嵌套多深。
- **子选择器(>)**
	- 定义：匹配某个元素的直接子元素（仅一级子元素）。
	- 语法：`选择器1 > 选择器2`
	- 注意事项：
		- 精确匹配，范围限于直接子级。
		- 性能优于后代选择器。
		- 需要区分直接子元素和深层后代的场景。
	- 示例：
		```css
		ul > li { list-style: none; }
		```
		- 仅匹配 `<ul>` 的直接 `<li>` 子元素，不包括嵌套更深的 `<li>` 。
- **相邻兄弟选择器(+)**
	- 定义：匹配紧跟在指定元素后的第一个兄弟元素（同级且紧邻）。
	- 语法：`选择器1 + 选择器2`
	- 注意事项：
		- 只影响紧邻的第一个兄弟元素。
		- 必须是同级元素。
		- 调整相邻元素之间的间距或样式。
	- 示例：
		```css
		h1 + p { margin-top: 0; }
		```
		- 匹配紧跟在 `<h1>` 后的第一个 `<p>` 元素。
- **通用兄弟选择器(~)**
	- 定义：匹配指定元素后的所有同级兄弟元素（无需紧邻）。
	- 语法：`选择器1 ~ 选择器2`
	- 注意事项：
		- 范围比相邻兄弟选择器广，影响所有后续兄弟。
		- 仍限于同级元素。
		- 为某元素后的所有同类型兄弟元素设置样式。
	- 示例：
		```css
		h2 ~ p { color: gray; }
		```
		- 匹配 `<h2>` 之后的所有同级 `<p>` 元素。
### 伪类选择器
---
- **伪类概述**
	- **定义**：伪类选择器用于选择元素的特定状态（如鼠标悬停、焦点）或基于文档结构的位置（如第一个子元素）。
	- **语法**：`selector:pseudo-class { property: value; }`
	- **特点**：
		- 动态响应用户交互或元素状态。
		- 可与基础选择器、组合选择器结合使用。
		- 优先级与类选择器相同（权重为 10）。
- **动态伪类**
	- **用于响应用户交互或元素状态变化**。
	- `:hover`：鼠标指针悬停在元素上时。
		- 示例：`a:hover { color: red; }`（链接悬停变红）
	- `:active`：元素被激活时（如鼠标点击按下瞬间）。
		- 示例：`button:active { background: darkgray; }`
	- `:focus`：元素获得焦点时（常用于表单元素）。
		- 示例：`input:focus { border-color: blue; }`
	- `:visited`：用户访问过的链接（仅限链接元素）。
		- 示例：`a:visited { color: purple; }`
		- 注意：出于隐私限制，仅能修改部分样式（如颜色）。
	- `:link`：未访问的链接。
		- 示例：`a:link { text-decoration: none; }`
	- `:target`：选择链接书签跳转过去的目标元素。
		- 当页面 URL 的片段标识符（如 `#section1`）匹配某个元素的 `id` 时，该元素成为“目标元素”，并应用 `:target` 样式。
		- 示例：`#section1:target {background: yellow; border: 2px solid red;}`
- **状态伪类**
	- **用于表单元素或特定状态**。
	- `:checked`：匹配选中的复选框或单选按钮。
		- 示例：`input[type="checkbox"]:checked { outline: 2px solid green; }`
	- `:enabled / :disabled`：匹配没有/有禁用状态的表单元素。
		- 示例：`button:enabled {background-color: #4CAF50; color: white;}`
	- `:required / :optional`：匹配带有/不带有 `required` 属性的表单元素。
		- 示例：`input:required { border-left: 2px solid red; }`
	- `:valid / :invalid`：匹配表单验证通过/未通过的元素。
		- 示例：`input:valid { border-color: green; }`
	- `:in-range / :out-of-range`：匹配选择值在/不在指定范围的元素。
		- 仅适用于具有 `min` 和/或 `max` 属性的输入元素。
		- 示例：`input:in-range {border: 2px solid yellow;}`
	- `:read-only / :read-write`：匹配只读/可读可写的表单元素。
		- 可读可写表示元素没有被设置 `readonly` 和 `disabled` 属性。
		- 示例：`input:read-write {background-color: yellow;}`
	- `:focus-within`：当元素或其后代获得焦点时。
		- 示例：`form:focus-within { background: #f0f0f0; }`
		- 注意：浏览器支持较新，需检查兼容性。
- **结构伪类**
	- 基于文档结构选择元素。
	- `:first-child`：匹配父元素的第一个子元素。
		- 示例：`li:first-child { font-weight: bold; }`
		- 匹配父元素下的第一个`<li>` 元素
	- `:last-child`：匹配父元素的最后一个子元素。
		- 示例：`li:last-child { border-bottom: none; }`
	- `:nth-child(n)`：匹配父元素的第 n 个子元素。
		- 参数：数字（如 2）、关键字（`odd/even`）、表达式（如 2n+1）。
		- 示例：`tr:nth-child(even) { background: #f2f2f2; }`（隔行变色）
	- `:nth-of-type(n)`：匹配同类型中的第 n 个元素。
		- 示例：`p:nth-of-type(2) { color: blue; }`
	- `:first-of-type / :last-of-type`：匹配同类型中的第一个/最后一个元素。
		- 示例：`div:first-of-type { margin-top: 0; }`
	- `:only-child`：匹配父元素中唯一的子元素。
		- 示例：`p:only-child { text-align: center; }`
	- `:only-of-type`：匹配父元素中唯一某种类型的元素。
		- 示例：`img:only-of-type { max-width: 100%; }`
	- `:nth-last-child(n) / :nth-last-of-type(n)`：从最后一个子元素开始计数。
		- 示例：`li:nth-last-child(2) { color: red; }`
- **否定伪类**
	- `:not(selector)`：匹配不符合指定选择器的元素。
		- 示例：`input:not([type="submit"]) { border: 1px solid gray; }`
		- 注意：不能嵌套 `:not()`，且不支持复杂选择器（如 `:not(div p)`）。
- **其它伪类**
	- `:root`：匹配文档的根元素（通常是 ）。
		- 示例：`:root { --main-color: blue; }`
	- `:empty`：匹配没有子节点（包括文本）的元素。
		- 示例：`div:empty { display: none; }`
	- `:lang(language)`：匹配特定语言的元素。
		- 示例：`p:lang(en) { font-family: Arial; }`
	- `:target`：匹配 URL 片段标识符指向的元素。
		- 示例：`:target { background: yellow; }`
	- `:has(selector)`：匹配包含指定子元素的父元素（实验性）。
		- 示例：`section:has(.highlight) { border: 2px solid blue; }`
		- 注意：浏览器支持有限，需检查兼容性。
- **现代伪类**
	- `:is(selector-list)`：简化选择器列表，等效于多个选择器。
		- 示例：`:is(h1, h2, h3) { font-weight: bold; }`
		- 优点：提高代码可读性，减少重复。
	- `:where(selector-list)`：类似 `:is()`，但支持更灵活的条件。
		- 示例：`div:where(.active, .selected) { color: red; }`
	- 区别：`:is()` 权重取决于列表中最高权重选择器；`:where()` 权重为 0。
### 伪元素选择器
---
- **伪元素概述**
	- **定义**：伪元素选择器用于样式化元素的特定部分（如首字母、首行）或插入虚拟内容（如装饰性元素）。
	- **语法**：`selector::pseudo-element { property: value; }`
	- **特点**：
	    - 针对元素特定部分或虚拟内容进行样式化。
	    - 可与基础选择器、组合选择器结合使用。
	    - 优先级与元素选择器相同（权重为 1）。
- **常用伪元素**
	- 伪元素选择器用于样式化元素的特定部分（如首字母、首行或插入内容）
	- `::before`：在元素内容前插入虚拟内容。
		- 需配合`content`属性使用，可插入文本、图片或空字符串。
		- 示例：`p::before {content: "★ "; color: red;}`
	- `::after`：在元素内容后插入虚拟内容。
		- 示例：`.box::after {content: ""; display: block; clear: both;}`
	- `::first-line`：样式化元素的第一行文本（受限于块级元素）。
		- 示例：`p::first-line {font-weight: bold; color: blue;}`
	- `::first-letter`：样式化元素的首字母（通常用于块级元素）。
		- 示例：`p::first-letter {font-size: 2em; float: left;}`
	- `::selection`：样式化用户选中的文本。
		- 支持属性：`color`、`background`、`text-shadow`等。
		- 示例：`::selection {background: yellow; color: black;}`
- **其它伪元素**
	- `::marker`（较新，部分浏览器支持）：样式化列表项的标记（如 `<li>` 的项目符号或编号）。
		- 示例：`li::marker {color: red; content: "➤ ";}`
	- `::backdrop`（全屏模式相关）：用于样式化全屏元素（如 `<dialog>` ）背后的背景。
		- 示例：`dialog::backdrop {background: rgba(0, 0, 0, 0.5);}`
	- 实验性伪元素（如`::part`、`::slotted`）：用于 Web Components，样式化 Shadow DOM 或插槽内容。
		- 示例：`::part(button) {background: blue;}`
- **浏览器专用伪元素**
	- **浏览器伪元素概述**
		- 定义：浏览器专有伪元素是特定浏览器引擎提供的伪元素，用于样式化浏览器原生 UI 组件或实现特定效果，不属于 W3C CSS 标准。
		- 作用：
			- 自定义浏览器 UI 组件（如滚动条、输入框提示）。
			- 提供标准伪元素（如 `::before`、`::after`）之外的额外功能。
			- 示例：`::-webkit-scrollbar` 用于样式化 WebKit 引擎浏览器的滚动条。
		- 前缀：
			- `-webkit-`：用于 Chrome、Safari、Edge、Opera（基于 WebKit/Blink 引擎）。
			- `-moz-`：用于 Firefox（Gecko 引擎）。
			- `-ms-`：用于旧版 Internet Explorer（Trident 引擎）。
	- **滚动条伪元素**
		- `::-webkit-scrollbar`：样式化滚动条整体。
			- 属性：`width`、`height`、`background` 等。
			- 示例：`::-webkit-scrollbar {width: 10px;}`
		- `::-webkit-scrollbar-track`：滚动条轨道。
			- 示例：`::-webkit-scrollbar-track {background: #f1f1f1;border-radius: 5px;}`
		- `::-webkit-scrollbar-thumb`：滚动条滑块。
			- 示例：`::-webkit-scrollbar-thumb {background: #888;border-radius: 5px;}`
		- `::-webkit-scrollbar-button`：滚动条两端的按钮（上下/左右箭头）。
			- 示例：`::-webkit-scrollbar-button {background: #ccc;}`
		- `::-webkit-scrollbar-corner`：水平和垂直滚动条交汇处。
			- 示例：`::-webkit-scrollbar-corner {background: #f1f1f1;}`
	- **表单控件伪元素**
		- `::-webkit-input-placeholder`：样式化输入框的占位符文本。
			- 示例：`::-webkit-input-placeholder {color: #999;font-style: italic;}`
		- `::-webkit-search-cancel-button`：搜索输入框的清除按钮。
			- 示例：`input[type="search"]::-webkit-search-cancel-button {display: none; /* 隐藏清除按钮 */}`
		- `::-webkit-clear-button`：输入框的清除按钮（特定于某些表单）。
			- 示例：`::-webkit-clear-button {background: red;}`
	- **其它浏览器伪元素**
		- `::-webkit-progress-bar`：自定义 `<progress>` 元素的进度条背景。
			- 示例：`progress::-webkit-progress-bar {background: #eee;}`
		- `::-webkit-progress-value`：自定义 `<progress>` 元素的进度值部分。
			- 示例：`progress::-webkit-progress-value {background: green;}`
		- `::-webkit-meter-bar`、`::-webkit-meter-optimum-value`：自定义 `<meter>` 元素的样式。
		- `::-moz-range-track`、`::-moz-range-thumb（Firefox）`：样式化范围输入（`<input type="range">`）的轨道和滑块。
			- 示例：`input[type="range"]::-moz-range-thumb {background: blue;border-radius: 50%;}`


## 优先级与权重
---
### 优先级基本概念
---
- CSS优先级是浏览器根据样式来源和选择器特性决定样式应用顺序的机制。
- 当多个规则冲突时，优先级高的样式会覆盖优先级低的样式。
- 优先级主要由以下因素决定：
	- 样式来源：内联样式 > 内部样式 > 外部样式 > 浏览器默认样式
	- 选择器权重：不同类型选择器的权重不同
	- `!important`：显式声明的最高优先级
	- 声明顺序：同等权重下，后声明的规则覆盖先声明的规则（层叠性）
### 选择器权重计算
---
- CSS选择器的权重通过一个四位数字的模型来计算（通常用(a, b, c, d)表示），每一位的值对应不同类型的选择器：
	- a：内联样式（style属性），权重为 1000
	- b：ID选择器（如#id），权重为 100
	- c：类选择器（如`.class`）、属性选择器（如`[type="text"]`）、伪类（如`:hover`），权重为 10
	- d：标签选择器（如`div`）、伪元素（如`::before`），权重为 1
- 计算方式：
	- 统计选择器中每个类别的数量，组成权重值（a, b, c, d）。
	- 比较权重时，从高位到低位逐位比较，高的优先级胜出。
	- 示例：
		- `#header .nav a`：权重为 (0, 1, 1, 1) = $0*1000 + 1*100 + 1*10 + 1*1 = 111$
		- `.nav a`：权重为 (0, 0, 1, 1) = $0*1000 + 0*100 + 1*10 + 1*1 = 11$
		- `a`：权重为 (0, 0, 0, 1) = $0*1000 + 0*100 + 0*10 + 1*1 = 1$
- `!important` 的作用
	- 使用 `!important` 的样式具有最高优先级，会覆盖任何其他规则（除非另一个规则也使用 `!important` 且权重更高）。
	- 示例：
		```css
		p { color: blue !important; }
		#id p { color: red; }
		```
- 注意事项：
	- 通配符选择器（`*`）：权重为 0，不影响权重计算。
	- 继承样式：继承的样式（如 `color` 从父元素继承）优先级低于任何直接指定的样式，哪怕是标签选择器。
	- 组合选择器：权重是所有选择器权重的累加。例如，`.class1 > .class2` 的权重为 (0, 0, 2, 0) = 20。
## 值与单位
---
### 值的类型
---
- **关键字值**：特定属性支持的预定义关键字，如 `auto`、`none`、`inherit`、`initial`、`unset`。
    - 示例：`display: block;, visibility: hidden;`
- **数字值**：整数或小数，用于表示尺寸、数量等。
    - 示例：`opacity: 0.5;, z-index: 10;`
- **字符串值**：文本值，通常用于内容或 URL。
    - 示例：`content: "Hello";, background-image: url("image.jpg");`
- **函数值**：通过函数定义的复杂值，如 `rgb()`、`calc()`、`linear-gradient()`。
	- `calc()`：执行数学运算，支持加减乘除和混合单位。
		- 示例：`width: calc(100% - 20px);`
	- `min()/max()/clamp()`：选择最小值、最大值或限制范围。
		- 示例：`font-size: clamp(16px, 2vw, 18px);`
	- `var()`：使用 CSS 自定义属性（变量）。
		```css
		:root { --main-size: 20px; }
		div { font-size: var(--main-size); }
		```
- **颜色值**：表示颜色的多种格式。
	- 关键字：如 `red`、`blue`、`transparent`。
	- HEX：十六进制表示，如 `#FF0000`（红色）或 `#FFF`（白色简写）。
	- RGB/RGBA：`rgb(255, 0, 0)`（红色、绿色、黄色） 或 `rgba(255, 0, 0, 0.5)`（带透明度）。
	- HSL/HSLA：`hsl(0, 100%, 50%)`（色相、饱和度、亮度）或 `hsla(0, 100%, 50%, 0.5)`。
	- 现代颜色函数：
		- `oklch()`：基于人类视觉的颜色空间，如 `oklch(0.69 0.14 29.23)`。
		- `color()`：支持特定颜色空间，如 `color(display-p3 1 0 0)`。
	- currentColor：表示当前元素的 `color` 属性值，动态复用。
		- 示例：`border-color: currentColor;`

### 单位类型
---
- **绝对单位**：绝对单位与物理尺寸相关，通常用于打印或固定尺寸场景。
	- **px**（像素）：CSS 中的基本单位，理论上 1px 对应设备的一个像素，但现代设备会根据像素密度（DPI）缩放。
	- **cm**（厘米）：1cm = 37.8px（在 96dpi 下）。
	- **mm**（毫米）：1mm = 0.1cm。
	- **in**（英寸）：1in = 2.54cm。
	- **pt**（点）：1pt = 1/72 英寸，常用于打印。
	- **pc**（派卡）：1pc = 12pt。
	- 注意：绝对单位在高分辨率屏幕上可能因设备像素比（DPR）而变化。
- **相对单位**：相对单位根据上下文（如父元素、视口、字体大小）动态计算，适合响应式设计。
	- **%**（百分比）：相对于父元素的对应属性值。
	- **rem**（根元素字体大小）：相对于根元素（`<html>`）的 `font-size`。
	- **em**：相对于当前元素的 `font-size` 或父元素的 `font-size`（对于非字体属性）。
	- **vw**（视口宽度）：1vw = 视口宽度的 1%。
	- **vh**（视口高度）：1vh = 视口高度的 1%。
	- **vmin**：视口宽度和高度中较小值的 1%。
	- **vmax**：视口宽度和高度中较大值的 1%。
	- **ex**：相对于当前字体的小写字母“x”的高度（较少使用）。
	- **ch**：相对于字符“0”的宽度，适合等宽字体场景。
	- **fr**（分数单位）：用于 CSS Grid，表示网格轨道可用空间的比例。
		- 示例：`grid-template-columns: 1fr 2fr;`（1:2 比例分配空间）
- **角度单位**：用于旋转、渐变等场景。
	- **deg**（度）：0° 到 360°。
		- 示例：`transform: rotate(45deg);`
	- **rad**（弧度）：1rad ≈ 57.3°。
	- **grad**（梯度）：1grad = 0.9°。
	- **turn**（圈）：1turn = 360°。
		- 示例：`transform: rotate(0.5turn);`（旋转半圈）
- **时间单位**：用于动画和过渡。
	- **s**（秒）：表示时间长度。
		- 示例：`transition-duration: 2s;`
	- **ms**（毫秒）：1ms = 0.001s。
		- 示例：`animation-duration: 500ms;`
- **分辨率单位**：用于媒体查询或高分辨率图像。
	- **dpi**（每英寸点数）：每英寸像素数。
		- 示例：`@media (min-resolution: 300dpi) {...}`
	- **dpcm**（每厘米点数）：1dpcm ≈ 2.54dpi。
	- **dppx**（每像素点数）：1dppx = 96dpi。
- **无单位值**：某些属性接受无单位的数字。
	- `line-height`：如 `line-height: 1.5;`（1.5 倍字体大小）。
	- `opacity`：如 `opacity: 0.8;`（0 到 1 之间的值）。
	- `z-index`：如 `z-index: 100;`（整数）。
# 盒模型
---
## 盒模型组成
---
![[CSS3.png]]

- **内容（Content）**：元素的实际内容（如文本、图片），由 `width` 和 `height` 定义。
- **内边距（Padding）**：内容与边框之间的空间，填充在内容周围。
- **边框（Border）**：围绕内边距的线条，增加元素厚度。
- **外边距（Margin）**：盒子外部与其他元素之间的空间，可能与其他元素的 `margin` 合并。
## 盒模型相关属性
---
- **内容（Content）**
	- 属性：
		- `width`：设置内容区域的宽度。
		- `height`：设置内容区域的高度。
		- `min-width` / `max-width`：限制内容的最小/最大宽度。
		- `min-height` / `max-height`：限制内容的最小/最大高度。
	- 语法：
		```css
		width: 200px | 50% | auto;
		height: 100px | 20vh | auto;
		min-width: 100px;
		max-height: 500px;
		```
	- 说明：
		- 单位：绝对单位（px、cm）、相对单位（%、vw、vh、rem、em）。
		- `auto`：由浏览器根据内容或父元素决定。
	- 示例：
		```css
		.box {
		  width: 300px;
		  height: 150px;
		  min-width: 200px; /* 最小宽度 */
		}
		```
- **内边距（Padding）**
	- 作用：控制内容与边框之间的空间。
	- 属性：
		- `padding-top`、`padding-right`、`padding-bottom`、`padding-left`：分别设置四个方向的内边距。
		- `padding`：简写属性。
	- 语法：
		```css
		padding: 10px; /* 四个方向统一 */
		padding: 10px 20px; /* 上下 10px，左右 20px */
		padding: 10px 20px 30px; /* 上 10px，左右 20px，下 30px */
		padding: 10px 20px 30px 40px; /* 上 右 下 左 */
		```
	- 说明：
		- 支持 px、rem、%、auto（通常为 0）。
		- 内边距增加元素尺寸（除非使用 `box-sizing: border-box`）。
	- 示例：
		```css
		.box {
		  padding: 15px 20px; /* 上下 15px，左右 20px */
		}
		```
- **边框（Border）**
	- 作用：围绕内边距的线条，增加元素厚度。
	- 属性：
		- `border-width`：边框宽度（如 `1px`、`thin`、`medium`、`thick`）。
		- `border-style`：边框样式（如 `solid`、`dashed`、`dotted`、`double`）。
		- `border-color`：边框颜色。
		- `border`：简写属性。
		- `border-top`、`border-right` 等：单独设置某一边。
	- 语法：
		```css
		border: 2px solid #000; /* 宽度 样式 颜色 */
		border-width: 1px 2px; /* 上下 1px，左右 2px */
		border-style: dashed;
		border-color: #333;
		```
	- 说明：
		- 边框增加元素尺寸（除非使用 `box-sizing: border-box`）。
		- 支持透明边框（`border-color: transparent`）。
	- 示例：
		```css
		.box {
		  border: 1px solid black;
		  border-top: 2px dashed red; /* 仅上边框为红色虚线 */
		}
		```
- **外边距（Margin）**
	- 作用：控制元素与其他元素之间的外部间距。
	- 属性：
		- `margin-top`、`margin-right`、`margin-bottom`、`margin-left`：分别设置四个方向的外边距。
		- `margin`：简写属性。
	- 语法：
		```css
		margin: 10px; /* 四个方向统一 */
		margin: 10px 20px; /* 上下 10px，左右 20px */
		margin: 10px 20px 30px; /* 上 10px，左右 20px，下 30px */
		margin: 10px 20px 30px 40px; /* 上 右 下 左 */
		```
	- 说明：
		- 支持负值（如 `margin: -10px`），可用于元素重叠。
		- `margin: auto`：常用于水平居中（需配合 `width` 和块级元素）。
	- 示例：
		```css
		.box {
		  margin: 20px auto; /* 上下 20px，左右自动（水平居中） */
		}
		```
- **box-sizing（盒模型计算方式）**
	- 作用：控制盒模型的尺寸计算方式。
	- 语法：
		```css
		box-sizing: content-box | border-box;
		```
	- 说明：
		- `content-box`（默认）：`width` 和 `height` 只包括内容区域，`padding` 和 `border` 会增加总尺寸。
		- `border-box`：`width` 和 `height` 包括内容、内边距和边框，总尺寸固定。
	- 计算示例：
		```css
		.box {
		  width: 200px;
		  height: 100px;
		  padding: 10px;
		  border: 5px solid black;
		}
		```
		- content-box：总宽度 = 200px + 10px + 10px + 5px + 5px = 230px
		- border-box：总宽度 = 200px（内容宽度自动调整为 170px）
## 外边距合并处理
---
- **合并机制**
	- 定义：当两个垂直相邻的块级元素的外边距（`margin-top` 和 `margin-bottom`）接触时，它们会合并为一个外边距，高度取两者中的较大值。
	- 合并规则：
		- 两个正外边距：合并后取较大值。
			- 例：`margin-bottom: 20px` 和 `margin-top: 30px` 合并为 30px。
		- 正负外边距：合并后为两者相加。
			- 例：`margin-bottom: 20px` 和 `margin-top: -10px` 合并为 10px。
		- 两个负外边距：合并后取绝对值较大的负值。
			- 例：`margin-bottom: -20px` 和 `margin-top: -10px` 合并为 -20px。
	- 不合并的情况：
		- 水平方向（`margin-left` 和 `margin-right`）不合并。
		- 非块级元素（如 `display: inline` 或 `inline-block`）不合并。
		- 浮动（`float`）、绝对定位（`position: absolute/fixed`）或 Flex/Grid 布局中的元素不参与合并。
- **合并发生的常见场景**
	- 相邻兄弟元素：
		- 两个垂直排列的块级元素（如 `<div>` ）之间的外边距合并。
		```css
		.box1 {
		  margin-bottom: 20px;
		}
		.box2 {
		  margin-top: 30px;
		}
		/* 两元素间距为 30px（合并后取较大值） */
		```
	- 父子元素：
		- 如果父元素与子元素之间没有 `padding`、`border` 或其他分隔，子元素的 `margin-top` 或 `margin-bottom` 会与父元素的对应外边距合并，或者“穿透”到父元素外部。
		```css
		.parent {
		  background: #f0f0f0;
		}
		.child {
		  margin-top: 20px; /* 父元素顶部出现 20px 间距 */
		}
		```
	- 空块级元素：
		- 如果一个块级元素没有内容、`padding` 或 `border`，其 `margin-top` 和 `margin-bottom` 会合并。
		```css
		.empty {
		  margin-top: 20px;
		  margin-bottom: 30px;
		}
		/* 总间距为 30px */
		```
- **防止外边距合并的方法**
	1. 添加 padding 或 border：
		- 在父元素或相邻元素间添加非零的 `padding` 或 `border`，阻断合并。
		```css
		.parent {
		  padding: 1px; /* 防止子元素 margin-top 穿透 */
		}
		.child {
		  margin-top: 20px; /* 正常应用 */
		}
		```
	2. 使用 overflow：
		- 设置父元素的 `overflow: auto`、`hidden` 或 `scroll`，创建新的块格式化上下文（BFC）。
		```css
		.parent {
		  overflow: auto; /* 创建 BFC，阻止 margin 合并 */
		}
		.child {
		  margin-top: 20px;
		}
		```
	3. 改变 display 属性：
		- 将元素设置为 `display: inline-block`、`flex` 或 `grid`，避免合并。
		```css
		.box1, .box2 {
		  display: inline-block; /* 不再合并 margin */
		  margin: 20px;
		}
		```
	4. 使用定位：
		- 设置 `position: absolute` 或 `float`，脱离标准流，不参与合并。
		```css
		.child {
		  float: left; /* 浮动元素不合并 margin */
		  margin: 20px;
		}
		```
	5. 添加伪元素或空内容：
		- 在父元素开头添加 `::before` 伪元素，阻断父子 `margin` 合并。
		```css
		.parent::before {
		  content: '';
		  display: table; /* 阻止 margin 穿透 */
		}
		```
## 边框相关高级属性
---
- **border-radius（圆角）**
	- 作用：为元素添加圆角效果。
	- 语法：
		```css
		border-radius: 10px; /* 统一圆角 */
		border-radius: 10px 20px; /* 左上/右下 10px，右上/左下 20px */
		border-radius: 10px 20px 30px 40px; /* 左上 右上 右下 左下 */
		```
	- 说明：
		- 支持百分比（如 `border-radius: 50%` 创建圆形）。
		- 可单独设置某个角（如 `border-top-left-radius`）。
	- 示例：
		```css
		.circle {
		  width: 100px;
		  height: 100px;
		  border-radius: 50%; /* 圆形 */
		}
		```
- **box-shadow（盒子阴影）**
	- 作用：为元素添加阴影效果。
	- 语法：
		```css
		box-shadow: [inset] x-offset y-offset blur-radius spread-radius color;
		```
	- 说明：
		- `inset`：内阴影（可选）。
		- `x-offset`、`y-offset`：水平和垂直偏移。
		- `blur-radius`：模糊半径。
		- `spread-radius`：扩展半径。
		- `color`：阴影颜色。
	- 示例：
		```css
		.shadow {
		  box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3); /* 外阴影 */
		  box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.5); /* 内阴影 */
		}
		```
- **outline（轮廓）**
	- 作用：在边框外绘制轮廓，不占用空间。
	- 语法：
		```css
		outline: 2px solid blue;
		outline-width: 2px;
		outline-style: solid;
		outline-color: blue;
		outline-offset: 5px; /* 轮廓与边框的距离 */
		```
	- 说明：
		- 不影响布局，常用于调试或焦点样式。
	- 示例：
		```css
		.focus {
		  outline: 2px solid blue;
		  outline-offset: 3px;
		}
		```
# 布局
---
## 传统布局
---
### 定位
---
- `position` 属性控制元素在页面中的定位方式，影响元素在文档流中的位置以及与其他元素的关系。
- **定位类型**
	- `static`（默认）：
		- 元素按正常文档流排列，不受 `top`、`right`、`bottom`、`left` 影响。
		- 示例：
			```css
			.static {
			  position: static; /* 默认，正常文档流 */
			}
			```
	- `relative`（相对定位）：
		- 元素相对于其原始位置偏移，仍在文档流中占位。
		- 用 `top`、`right`、`bottom`、`left` 设置偏移量。
		- 示例：
			```css
			.relative {
			  position: relative;
			  top: 10px; /* 向下偏移 10px */
			  left: 20px; /* 向右偏移 20px */
			}
			```
	- `absolute`（绝对定位）：
		- 元素脱离文档流，相对于最近的非 `static` 定位祖先元素定位。
		- 如果没有定位祖先，相对于文档的初始包含块（通常是 或视口）。
		- 示例：
			```css
			.parent {
			  position: relative; /* 提供定位上下文 */
			  width: 300px;
			  height: 200px;
			}
			.absolute {
			  position: absolute;
			  top: 20px;
			  right: 20px; /* 相对于父元素右上角 */
			}
			```
	- `fixed`（固定定位）：
		- 元素脱离文档流，相对于视口定位，随页面滚动保持固定。
		- 示例：
			```css
			.fixed {
			  position: fixed;
			  bottom: 20px;
			  right: 20px; /* 固定在视口右下角 */
			}
			```
	- `sticky`（粘性定位）：
		- 混合 `relative` 和 `fixed` 特性，元素在正常文档流中，直到滚动到指定阈值时固定。
		- 需设置 `top`、`right`、`bottom` 或 `left`。
		- 示例：
			```css
			.sticky {
			  position: sticky;
			  top: 0; /* 滚动到顶部时固定 */
			}
			```
- **偏移量**
	- `top`、`right`、`bottom` 和 `left` 属性用于定位元素。
	- 这些属性只有在元素的 `position` 属性设置为 `relative`、`absolute`、`fixed` 或 `sticky` 时才会生效。它们指定元素在定位容器中的偏移量。
- **z-index**
	- 作用：控制定位元素在 z 轴上的叠放顺序。
	- 语法：
		```css
		z-index: 10 | -1 | auto;
		```
	- 说明：
		- 数值越大，元素越靠前显示。
		- 仅对 `position: relative`、`absolute`、`fixed` 或 `sticky` 生效。
	- 示例：
		```css
		.front {
		  position: absolute;
		  z-index: 100; /* 靠前显示 */
		}
		```
- **注意事项**：
	- **absolute 和 fixed 脱离文档流**：不占原空间，可能覆盖其他元素。
	- **relative 占位**：偏移后仍保留原始空间。
	- **sticky 兼容性**：现代浏览器支持良好，但老版本 IE 不支持。
	- **定位上下文**：`absolute` 元素依赖最近的非 `static` 祖先，需确保父元素设置 `position`。
### 浮动
---
- float 属性用于让元素“浮动”到容器的一侧，常用于图文混排或简单布局。
- **属性值**：
	- `left`：元素浮动到容器左侧。
	- `right`：元素浮动到容器右侧。
	- `none`：默认，不浮动。
	- 语法：
		```css
		float: left | right | none;
		```
- **浮动特性**：
	- 脱离文档流：浮动元素不再占据正常流空间，但仍影响后续元素的布局。
	- 块级化：浮动元素自动变为块级元素（类似 `display: block`）。
	- 环绕效果：非浮动内容（如文本）会围绕浮动元素排列。
	- 示例：
		```css
		.float-left {
		  float: left;
		  width: 100px;
		  height: 100px;
		  margin-right: 10px;
		}
		p {
		  /* 文本围绕浮动元素 */
		}
		```
- **清除浮动（Clear）**
	- 问题：浮动元素可能导致父元素高度塌陷（父元素高度为 0）。
	- `clear` 属性：
		- 清除浮动对后续元素的影响。
		- 值：`left`、`right`、`both`。
	- 清除方法：
		1. 使用 `clear`：
			```css
			 .clear {
			  clear: both; /* 清除两侧浮动 */
			}
			```
		2. 父元素创建 BFC（块格式化上下文）：
			```css
			.parent {
			  overflow: hidden; /* 或 auto */
			}
			```
		3. 使用伪元素（推荐）：
			```css
			.clearfix::after {
			  content: '';
			  display: block;
			  clear: both;
			}
			```
	- 示例：
		```css
		.parent {
		  border: 1px solid black;
		}
		.float {
		  float: left;
		  width: 100px;
		}
		.parent::after {
		  content: '';
		  display: block;
		  clear: both; /* 防止高度塌陷 */
		}
		```
### 显示
---
- `display` 属性控制元素的显示类型和布局行为，是传统布局的重要工具。
- 常见值：
	- block：
		- 块级元素，独占一行，宽度默认占满父元素，支持 `width` 和 `height`。
		- 示例：
			```css
			.block {
			  display: block;
			  width: 200px;
			  height: 100px;
			}
			```
	- inline：
		- 行内元素，不换行，宽度和高度由内容决定，不支持 `width` 和 `height`。
		- 示例：
			```css
			.inline {
			  display: inline;
			  /* width 和 height 无效 */
			}
			```
	- inline-block：
		- 结合 `inline` 和 `block` 特性，不换行但支持 `width` 和 `height`。
		- 示例：
			```css
			.inline-block {
			  display: inline-block;
			  width: 100px;
			  height: 50px;
			}
			```
	- none：
		- 隐藏元素，不占空间（区别于 `visibility: hidden`，后者占空间但不可见）。
		- 示例：
			```css
			.hidden {
			  display: none;
			}
			```
- 注意事项：
	- 




### 溢出
---









## 现代布局
----
### Flex布局
---





### Grid布局
---




### 多列布局
---






# 样式属性
---
## 文本与字体
---
### 字体属性
---
- 字体属性主要用于定义文本的字体类型、大小、粗细等外观特性。
- **font-family（字体族）**
	- 作用：指定文本的字体，允许设置多个字体作为回退（fallback）。
	- 语法：
		```css
		font-family: [font-name | generic-family][, [font-name | generic-family]]*;
		```
	- 说明：
		- `font-name`：具体字体名称，如 "Arial", "Times New Roman", "Roboto"。
		- `generic-family`：通用字体族，包括：
			- `serif`（衬线字体）
			- `sans-serif`（无衬线字体）
			- `monospace`（等宽字体）
			- `cursive`（手写体）
			- `fantasy`（装饰字体）。
		- 字体名称按优先级排列，浏览器从左到右尝试使用。
		- 最后通常指定通用字体族（如 `sans-serif`、`serif`、`monospace`）作为最终回退。
		- 回退机制确保字体不可用时使用替代字体，建议最后指定通用字体族。
		- 如果字体名称包含空格（如 "Times New Roman"），需加引号。
	- 示例：
		```css
		body {
		  font-family: "Roboto", "Open Sans", sans-serif;
		}
		```
- **font-size（字体大小）**
	- 作用：设置文本的大小。
	- 语法：
		```css
		font-size: <absolute-size> | <relative-size> | <length> | <percentage>;
		```
	- 示例：
		```css
		h1 {
		  font-size: 2.5rem; /* 根元素字体大小的 2.5 倍 */
		}
		p {
		  font-size: 16px;
		}
		```
- **font-weight（字体粗细）**
	- 作用：控制字体粗细程度。
	- 语法：
		```css
		font-weight: normal | bold | bolder | lighter | 100 | 200 | 300 | ... | 900;
		```
	- 说明：
		- 常用值：`normal`（400）、`bold`（700）、`bolder`（比父元素粗一个级别）、`lighter`（父元素细一个级别）。
		- 数值范围 100-900，具体效果取决于字体是否支持多种粗细。
	- 示例：
		```css
		h2 {
		  font-weight: 700;
		}
		.light-text {
		  font-weight: 300;
		}
		```
- **font-style（字体样式）**
	- 作用：设置字体倾斜样式。
	- 语法：
		```css
		font-style: normal | italic | oblique;
		```
	- 说明：
		- `normal`：正常（默认）。
		- `italic`：使用字体的斜体版本。
		- `oblique`：强制倾斜，通常是正常字体的变形。
	- 示例：
		```css
		em {
		  font-style: italic;
		}
		```
- **font-variant（字体变体）**
	- 作用：控制字体的变体显示，如小型大写字母（small-caps）、连字、数字样式等。
	- 语法：
		```css
		font-variant: normal | none | [ <font-variant-caps> || <font-variant-numeric> || <font-variant-alternates> || <font-variant-ligatures> || <font-variant-east-asian> ];
		```
	- 子属性：
		- `font-variant-caps`：控制大小写变体。
		- `font-variant-numeric`：控制数字、分数和序数的显示。
		- `font-variant-alternates`：控制字体的替代样式（如历史形式或装饰字符）。
		- `font-variant-ligatures`：控制连字（ligatures）行为。
		- `font-variant-east-asian`：控制东亚文字（如中文、日文、韩文）的变体。
	- 说明：
		- 简写属性：值为 `normal` 时重置所有子属性为默认值，值为 `none` 时禁用所有连字。
		- `normal`：正常（默认）。
		- `small-caps`：将小写字母显示为小型大写字母。
	- 示例：
		```css
		/* 基本小型大写 */
		.small-caps {
		  font-variant: small-caps; /* 小写字母显示为小型大写 */
		}
		
		/* 综合设置 */
		.fancy-text {
		  font-variant: small-caps tabular-nums no-common-ligatures; /* 小型大写、等宽数字、禁用常见连字 */
		}
		
		/* 单独设置子属性 */
		.numeric {
		  font-variant-numeric: tabular-nums slashed-zero; /* 等宽数字和斜杠零 */
		}
		
		/* 东亚字体变体 */
		.japanese {
		  font-variant-east-asian: ruby; /* 适合日文注音 */
		}
		```
- **font（字体简写）**
	- 作用：将多个字体属性合并为一行。
	- 语法：
		```css
		font: [font-style] [font-variant] [font-weight] font-size[/line-height] font-family;
		```
	- 说明：
		- 必须包含 `font-size` 和 `font-family`，其他属性可选。
		- `line-height` 可与 `font-size` 一起写，用斜杠分隔（如 `16px/1.5`）。
	- 示例：
		```css
		p {
		  font: italic bold 16px/1.5 "Arial", sans-serif;
		}
		```
### 文本属性
---
- **direction（文本方向）**
	- 作用：设置文本的书写方向，适用于多语言排版。
	- 语法：
		```css
		direction: ltr | rtl | inherit;
		```
	- 说明：
		- `ltr`：从左到右（默认，适合英文等）。
		- `rtl`：从右到左（适合阿拉伯文、希伯来文等）。
		- 常与 `unicode-bidi` 配合使用，控制双向文本的显示。
	- 示例：
		```css
		.arabic {
		  direction: rtl; /* 右到左排版 */
		}
		```
- **unicode-bidi（双向文本控制）**
	- 作用：控制双向文本（混合不同书写方向的文本）的显示行为，通常与 `direction` 配合使用。
	- 语法：
		```css
		unicode-bidi: normal | embed | bidi-override | isolate | isolate-override | plaintext;
		```
	- 说明：
		- `normal`：默认，浏览器根据 Unicode 算法处理。
		- `embed`：创建新的双向嵌入层，用于在一段文本中添加具有不同书写方向的子标签文本内容。
		- `bidi-override`：使文本覆盖正常的双向文本算法，使文本严格按照指定方向显示。
		- 主要用于处理混合语言（如英文和阿拉伯文混排）。
	- 示例：
		```css
		.bidi-text {
		  direction: rtl;
		  unicode-bidi: bidi-override;
		}
		```
- **text-align（文本对齐）**
	- 作用：设置块级元素内文本的水平对齐方式。
	- 语法：
		```css
		text-align: left | right | center | justify;
		```
	- 说明：
		- `left`：左对齐（默认，取决于语言方向）。
		- `right`：右对齐。
		- `center`：居中对齐。
		- `justify`：两端对齐（可能导致单词间距不均匀）。
	- 示例：
		```css
		p {
		  text-align: justify;
		}
		h1 {
		  text-align: center;
		}
		```
- **text-align-last（最后一行对齐）**
	- 作用：控制块级元素最后一行文本的对齐方式。
	- 语法：
		```css
		text-align-last: auto | left | right | center | justify | start | end;
		```
	- 说明：
		- 常用于 `text-align: justify` 时，单独控制最后一行。
		- `start` 和 `end`：根据文本方向动态调整（`ltr` 时 `start` 为左，`rtl` 时为右）。
	- 示例：
		```css
		p {
		  text-align: justify;
		  text-align-last: center; /* 最后一行居中 */
		}
		```
- **text-decoration（文本装饰）**
	- 作用：作用：为文本添加装饰线（如下划线、上划线、删除线）或移除装饰，控制装饰线的类型、样式和颜色。
	- 语法：
		```css
		text-decoration: [text-decoration-line] [text-decoration-style] [text-decoration-color] [text-decoration-thickness] | none;
		```
	- 子属性：
		- `text-decoration-line`：`none`（默认） | `underline` | `overline` | `line-through`
			- 定义装饰线类型：移除装饰线、下划线、上划线或删除线。
		- `text-decoration-style`：`solid`（默认） | `double` | `dotted` | `dashed` | `wavy`
			- 设置装饰线的样式：实线、双线、点线、虚线或波浪线。
		- `text-decoration-color`：颜色值（如 `#ff0000`、 `red`、 `rgba(0,0,0,0.5)`）
			- 指定装饰线的颜色。
		- `text-decoration-thickness`：`auto` | `from-font` | `<length>` | `<percentage>`
			- 设置装饰线的粗细：自动、字体文件定义的粗细、手动指定或字体百分比。
	- 说明：
		- 简写属性，子属性顺序不固定，未指定的子属性使用默认值，`text-decoration-line` 为必须属性。
		- `none` 用于移除装饰，常用于取消链接默认下划线。
		- 浏览器兼容性：`text-decoration-style` 和 `text-decoration-color` 在老旧浏览器（如 IE）可能需要 `-webkit-` 前缀。
	- 示例：
		```css
		a {
		  text-decoration: none; /* 移除下划线 */
		}
		.strikethrough {
		  text-decoration: line-through;
		}
		```
- **line-height（行高）**
	- 作用：设置文本行间距，影响行与行之间的垂直间距。
	- 语法：
		```css
		line-height: normal | 1.5 | 20px | 150%;
		```
	- 说明：
		- 数值（无单位）：基于字体大小的倍数。
		- 单位值：px、rem、em、%。
		- normal：浏览器默认值，通常约为 1.2。
	- 示例：
		```css
		p {
		  line-height: 1.6; /* 行高为字体大小的 1.6 倍 */
		}
		```
- **letter-spacing（字符间距）**
	- 作用：控制字符之间的间距。
	- 语法：
		```css
		letter-spacing: normal | 2px | -1px;
		```
	- 说明：
		- 正值增加间距，负值减少间距。
		- `normal` 为默认间距。
	- 示例：
		```css
		h1 {
		  letter-spacing: 2px; /* 字符间距增加 2px */
		}
		```
- **word-spacing（单词间距）**
	- 作用：控制单词之间的间距（仅对空格分隔的语言有效）。
	- 语法：
		```css
		word-spacing: normal | 5px | -2px;
		```
	- 示例：
		```css
		p {
		  word-spacing: 4px;
		}
		```
- **text-transform（文本大小写）**
	- 作用：控制文本的大小写。
	- 语法：
		```css
		text-transform: none | uppercase | lowercase | capitalize | full-width;
		```
	- 说明：
		- `uppercase`：全大写。
		- `lowercase`：全小写。
		- `capitalize`：每个单词首字母大写。
		- `full-width`：将字符转换为全角字符（占用两个标准字符宽度的字符）。
	- 示例：
		```css
		h1 {
		  text-transform: uppercase; /* HELLO WORLD */
		}
		```
- **text-indent（文本缩进）**
	- 作用：设置首行缩进。
	- 语法：
		```css
		text-indent: 2em | 20px | 10%;
		```
	- 说明：
		- 正值向右缩进，负值向左缩进（可用于隐藏文本）。
		- 常用于段落首行缩进。
	- 示例：
		```css
		p {
		  text-indent: 2em; /* 首行缩进两个字符 */
		}
		```
- **white-space（空白处理）**
	- 作用：控制文本中空白字符的显示方式。
	- 语法：
		```css
		white-space: normal | nowrap | pre | pre-wrap | pre-line | break-spaces;
		```
	- 说明：
		- `normal`：默认，合并空白，自动换行。
		- `nowrap`：合并空白，不换行。
		- `pre`：保留所有空白和换行，类似 `<pre>` 标签。
		- `pre-wrap`：保留空白，允许换行。
		- `pre-line`：合并空白，保留换行。
		- `break-spaces`：类似于 `pre-wrap`，但保留的空白字符包括所有类型的空白字符（如 U+2028 行分隔符）。
	- 示例：
		```css
		.no-wrap {
		  white-space: nowrap; /* 文本不换行 */
		}
		```
- **text-overflow（文本溢出）**
	- 作用：处理文本溢出容器时的显示效果。
	- 语法：
		```css
		text-overflow: clip | ellipsis;
		```
	- 说明：
		- `clip`：直接截断。
		- `ellipsis`：显示省略号（`...`）。
		- 需配合 `overflow: hidden` 和 `white-space: nowrap` 使用。
	- 示例：
		```css
		.ellipsis {
		  white-space: nowrap;
		  overflow: hidden;
		  text-overflow: ellipsis;
		  width: 200px;
		}
		```
- **text-shadow（文本阴影）**
	- 作用：为文本添加阴影效果，增强视觉层次或装饰效果。
	- 语法：
		```css
		text-shadow: [offset-x] [offset-y] [blur-radius] [color] | none;
		```
	- 说明：
		- `offset-x`：阴影的水平偏移（正值向右，负值向左）。
		- `offset-y`：阴影的垂直偏移（正值向下，负值向上）。
		- `blur-radius`：模糊半径（可选，越大越模糊，默认为 0）。
		- `color`：阴影颜色（如 `#000`、`rgba(0,0,0,0.5)`），可省略（默认继承文本颜色）。
		- 支持多组阴影，用逗号分隔。
	- 示例：
		```css
		h1 {
		  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5); /* 基本阴影 */
		}
		.neon {
		  text-shadow: 0 0 5px #fff, 0 0 10px #ff0, 0 0 20px #f0f; /* 霓虹效果 */
		}
		.no-shadow {
		  text-shadow: none; /* 移除阴影 */
		}
		```
- **其它文本属性**
	- **text-emphasis（文本强调）**
		- 作用：为文本添加强调符号（如东亚语言的着重号）。
	- **hanging-punctuation（悬挂标点）**
		- 作用：控制标点是否悬挂在文本框外（常见于东亚排版）。
	- **text-justify（两端对齐优化）**
		- 作用：优化 `text-align: justify` 时的间距分配。
## 颜色与背景
---
### 颜色属性
---
- **color（文本颜色）**
	- 作用：设置元素的前景色，通常用于文本颜色，也影响某些装饰线（如下划线）。
	- 语法：
		```css
		color: #ff0000; /* HEX */
		color: rgb(255, 0, 0); /* RGB */
		color: rgba(255, 0, 0, 0.5); /* RGBA，带透明度 */
		color: hsl(0, 100%, 50%); /* HSL */
		color: hsla(0, 100%, 50%, 0.5); /* HSLA，带透明度 */
		color: red; /* 关键字 */
		```
	- 说明：
		- 支持多种颜色表示方式：
			- 关键字：如 red、blue、transparent。
			- HEX：`#RRGGBB` 或简写 `#RGB`。
			- RGB：`rgb(red, green, blue)`，值 0-255。
			- RGBA：RGB 基础上加透明度（0-1）。
			- HSL：`hsl(hue, saturation, lightness)`，色相（0-360）、饱和度（0%-100%）、亮度（0%-100%）。
			- HSLA：HSL 基础上加透明度。
		- 默认继承父元素的颜色。
	- 示例：
		```css
		p {
		  color: #333333; /* 深灰色 */
		}
		a {
		  color: rgba(0, 123, 255, 0.8); /* 带透明度的蓝色 */
		}
		```
- **opacity（透明度）**
	- 作用：设置整个元素的透明度，影响元素及其子元素。
	- 语法：
		```css
		opacity: 0 | 0.5 | 1; /* 0（完全透明）到 1（完全不透明） */
		```
	- 说明：
		- 与 RGBA/HSLA 的透明度不同，opacity 影响整个元素（包括背景、文本等）。
		- 值范围 0-1，0 为完全透明，1 为完全不透明。
	- 示例：
		```css
		.fade {
		  opacity: 0.7; /* 70% 不透明 */
		}
		```
- **其他颜色相关属性**
	- `currentColor`：表示当前元素的 color 值，可用于其他属性（如 border-color）。
		```css
		button {
		  color: blue;
		  border: 1px solid currentColor; /* 边框颜色与文本颜色一致 */
		}
		```
	- 系统颜色：如 SystemBlue、SystemBackground（主要用于原生 UI 开发，较少用于 Web）。
### 背景属性
---
- **background-color（背景颜色）**
	- 作用：设置元素的背景颜色。
	- 语法：
		```css
		background-color: #ffffff; /* HEX */
		background-color: transparent; /* 透明（默认） */
		background-color: rgba(0, 0, 0, 0.1); /* 带透明度 */
		```
	- 说明：
		- 支持与 `color` 属性相同的颜色表示方式。
		- `transparent` 为默认值，显示父元素背景。
	- 示例：
		```css
		.box {
		  background-color: #f0f0f0; /* 浅灰色背景 */
		}
		```
- **background-image（背景图片）**
	- 作用：为元素设置背景图片。
	- 语法：
		```css
		background-image: url('image.jpg'); /* 图片路径 */
		background-image: none; /* 无背景图片（默认） */
		background-image: linear-gradient(to right, red, blue); /* 渐变 */
		```
	- 说明：
		- 图片路径需用 `url()` 指定，支持相对或绝对路径。
		- 支持渐变背景（如 `linear-gradient`、`radial-gradient`）。
	- 示例：
		```css
		.hero {
		  background-image: url('hero-bg.jpg');
		}
		.gradient {
		  background-image: linear-gradient(90deg, #ff0000, #0000ff);
		}
		```
- **background-repeat（背景重复）**
	- 作用：控制背景图片的重复方式。
	- 语法：
		```css
		background-repeat: repeat | repeat-x | repeat-y | no-repeat | space | round;
		```
	- 说明：
		- `repeat`：默认，水平和垂直重复。
		- `repeat-x`：仅水平重复。
		- `repeat-y`：仅垂直重复。
		- `no-repeat`：不重复。
		- `space`：平铺但不裁剪，两端对齐。
		- `round`：平铺但调整图片大小以避免裁剪。
	- 示例：
		```css
		.pattern {
		  background-image: url('tile.png');
		  background-repeat: repeat;
		}
		.single-image {
		  background-repeat: no-repeat;
		}
		```
- **background-position（背景定位）**
	- 作用：设置背景图片的起始位置。
	- 语法：
		```css
		background-position: top left | 50% 50% | 10px 20px;
		```
	- 说明：
		- 关键字：`top`、`bottom`、`left`、`right`、`center`。
		- 百分比：相对于容器尺寸。
		- 绝对单位：px、rem 等。
		- 支持简写（如 `background-position: 0 0`）。
	- 示例：
		```css
		.bg-image {
		  background-image: url('image.jpg');
		  background-position: center center; /* 图片居中 */
		}
		```
- **background-size（背景尺寸）**
	- 作用：控制背景图片的大小。
	- 语法：
		```css
		background-size: auto | cover | contain | 100px 200px | 50%;
		```
	- 说明：
		- `auto`：默认，保持图片原始尺寸。
		- `cover`：缩放图片以覆盖整个容器，可能裁剪。
		- `contain`：缩放图片以完全显示，可能留白。
		- 具体值：指定宽高（如 100px 100px）。
		- 百分比：相对于容器尺寸。
	- 示例：
		```css
		.cover-bg {
		  background-image: url('bg.jpg');
		  background-size: cover;
		}
		```
- **background-attachment（背景附着）**
	- 作用：控制背景图片随内容滚动的行为。
	- 语法：
		```css
		background-attachment: scroll | fixed | local;
		```
	- 说明：
		- `scroll`：默认，随元素滚动。
		- `fixed`：固定于视口，不随元素滚动。
		- `local`：随元素内容滚动（常用于溢出容器）。
	- 示例：
		```css
		.fixed-bg {
		  background-image: url('bg.jpg');
		  background-attachment: fixed; /* 背景固定，视差效果 */
		}
		```
- **background-clip（背景裁剪）**
	- 作用：指定背景的绘制区域。
	- 语法：
		```css
		background-clip: border-box | padding-box | content-box;
		```
	- 说明：
		- `border-box`：默认，背景覆盖边框、填充和内容区域。
		- `padding-box`：背景覆盖填充和内容区域。
		- `content-box`：背景仅覆盖内容区域。
	- 示例：
		```css
		.clip-box {
		  background-color: #f0f0f0;
		  border: 5px dashed #000;
		  background-clip: content-box; /* 背景仅在内容区 */
		}
		```
- **background-origin（背景定位原点）**
	- 作用：指定背景图片定位的起点。
	- 语法：
		```css
		background-origin: border-box | padding-box | content-box;
		```
	- 说明：
		- 类似 `background-clip`，但只影响定位起点。
	- 示例：
		```css
		.origin-box {
		  background-image: url('image.jpg');
		  background-origin: content-box; /* 从内容区开始定位 */
		}
		```
- **background-blend-mode（背景混合模式）**
	- 作用：定义背景层（图片或颜色）之间的混合效果。
	- 语法：
		```css
		background-blend-mode: normal | multiply | screen | overlay | darken | lighten;
		```
	- 说明：
		- 用于多背景或背景与颜色混合，常见于设计效果。
	- 示例：
		```css
		.blend {
		  background-image: url('image.jpg');
		  background-color: #ff0000;
		  background-blend-mode: multiply; /* 背景图片与红色混合 */
		}
		```
- **background（背景简写）**
	- 作用：将多个背景属性合并为一行。
	- 语法：
		```css
		background: [color] [image] [position] / [size] [repeat] [attachment] [origin] [clip];
		```
	- 说明：
		- 属性顺序灵活，但 size 需跟在 position 后，用斜杠分隔。
		- 未指定的属性使用默认值。
	- 示例：
		```css
		.box {
		  background: #f0f0f0 url('bg.jpg') center / cover no-repeat fixed;
		}
		```
### 渐变背景
---
- **渐变概述**
	- 定义：渐变是一种从一种颜色平滑过渡到另一种颜色的效果，CSS 支持通过 `background-image` 设置线性渐变（`linear-gradient`）、径向渐变（`radial-gradient`）等。
	- 作用：用于创建动态、丰富的背景效果，替代静态图片，优化性能并增强视觉表现。
	- 属性：渐变主要通过 `background-image` 定义，但常与其他背景属性（如 `background-size`、`background-position`）组合使用。
- **线性渐变（linear-gradient）**
	- 作用：沿直线方向（或指定角度）从一种颜色过渡到另一种颜色。
	- 语法：
		```css
		background-image: linear-gradient([direction | angle], color-stop1, color-stop2, ...);
		```
	- 说明：
		- `direction` | `angle`：渐变方向，可用关键字（如 `to right`、`to bottom left`）或角度（如 `45deg`）。
		- `color-stop`：颜色停止点，指定颜色及可选的停止位置（如 `red 20%`）。
	- 示例：
		```css
		.linear {
		  background-image: linear-gradient(to right, red, blue); /* 从左到右，红到蓝 */
		}
		.angled {
		  background-image: linear-gradient(45deg, yellow 20%, green 80%); /* 45度角渐变 */
		}
		.multi-stop {
		  background-image: linear-gradient(to bottom, red 0%, orange 50%, blue 100%); /* 多色渐变 */
		}
		```
- **径向渐变（radial-gradient）**
	- 从中心点向外辐射的颜色渐变，可形成圆形或椭圆形效果。
	- 语法：
		```css
		background-image: radial-gradient([shape size at position], color-stop1, color-stop2, ...);
		```
	- 说明：
		- `shape`：`circle`（圆形）或 `ellipse`（椭圆，默认）。
		- `size`：关键字（如 `closest-side`、`farthest-corner`）或具体尺寸。
		- `position`：渐变中心点（如 `center`、`10px 20px`）。
		- `color-stop`：类似线性渐变，指定颜色和停止位置。
	- 示例：
		```css
		.radial {
		  background-image: radial-gradient(circle, red, blue); /* 圆形渐变，中心红到边缘蓝 */
		}
		.positioned {
		  background-image: radial-gradient(circle at top left, yellow, green); /* 从左上角开始 */
		}
		```
- **重复渐变**
	- 作用：创建重复的渐变图案，适用于线性（`repeating-linear-gradient`）或径向（`repeating-radial-gradient`）。
	- 语法：
		```css
		background-image: repeating-linear-gradient([direction | angle], color-stop1, color-stop2, ...);
		background-image: repeating-radial-gradient([shape size at position], color-stop1, color-stop2, ...);
		```
	- 示例：
		```css
		.repeating {
		  background-image: repeating-linear-gradient(45deg, red 0%, red 10%, blue 10%, blue 20%); /* 重复条纹 */
		}
		```
- **圆锥渐变（conic-gradient）**
	- 作用：围绕中心点旋转的渐变，类似饼图效果。
	- 语法：
		```css
		background-image: conic-gradient([from angle] [at position], color-stop1, color-stop2, ...);
		```
	- 示例：
		```css
		.conic {
		  background-image: conic-gradient(red, yellow, green); /* 扇形渐变 */
		}
		```




# 过渡、动画与变换
---






# 响应式设计
---

















# 预处理器与框架
---





















# 性能与优化
---

























# 浏览器兼容性
---















# 高级特性
---







# 调试与工具
----




















# CSS未来趋势
---



vertical-align：垂直对齐


# CSS基础

## CSS简介与历史

CSS（层叠样式表）用于格式化网页，使用CSS可以控制颜色、字体、文本大小、元素之间的间距、元素的定位和布局方式、要使用的背景图像或背景颜色、不同设备和屏幕尺寸的不同显示等等等等。如果HTML是一只光溜溜的鸟，那CSS就是它的羽毛，**HTML描述网页内容，CSS定义网页样式**。

早期的HTML中存在着诸如`<font>`等样式标签，因为这些样式标签不能重复使用，并且需要为每个元素单独设置，这为网页的构建、样式添加造成了巨大的工作量与昂贵的代价，为了解决这个问题，万维网联盟（W3C）在1996年发布了CSS1，引入了基本的样式能力，从而将样式从HTML中删除，经过不断发展，CSS经历了从基础样式控制到强大布局和动画能力的持续演进。

## CSS语法

CSS语法规则由选择器和声明块组成。

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/ba995cf5-478f-49ac-89ae-89d206da3e0c/CsQs8Yimageimage_kvW147naP4.png)

选择器指向您想要设置样式的 HTML 元素，声明块包含一个或多个以分号分隔的声明，每个声明都包含一个 CSS 属性名称和一个值，以冒号分隔，多个 CSS 声明用分号分隔，声明块用大括号括起来。

CSS中注释语法为：

```css
/*这是CSS中的注释*/
```

## HTML引入CSS

在HTML中使用CSS样式主要有三种方式：

- 内联：在HTML元素内使用style属性。
    
    ```html
    <h1 style="color:blue;">A Blue Heading</h1>
    ```
    
- 内部：在<header>元素内使用<style>元素。
    
    ```html
    <head>
    	...
      <style>
        body {background-color: powderblue;}
        h1 {color: blue;}
        p {color: red;}
      </style>
    </head>
    ```
    
- 外部：通过<link>元素链接到外部CSS文件，可以是另外网站的CSS文件。
    
    ```html
    <head>
      <link rel="stylesheet" href="styles.css" />
      <link rel="stylesheet" href="<https://www.w3schools.com/html/styles.css>" />
    </head>
    ```
    

这三种引入方式的优先级：内联样式>内部样式>外部样式表。另外CSS的选择器优先级也会影响样式的应用顺序：内联样式>ID选择器>类、伪类和属性选择器>元素和伪元素选择器。

## 颜色和背景

### CSS颜色

> CSS中颜色是通过预定义的颜色名称或者RGB、RGBA、HSL、HSLA、HEX的值指定的。

---

**CSS中可以通过预定义的颜色名称来指定颜色。**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/bf522427-4250-4cd8-adc4-6e3a3be60ef9/Untitled.png)

CSS/HTML中支持140种标准颜色的名称：

[CSS Colors](https://www.w3schools.com/css/css_colors.asp)

- **代码示例**
    
    ```css
    /* 设置背景颜色 */
    <h1 style="background-color:DodgerBlue;">Hello World</h1>
    /* 设置文本颜色 */
    <h1 style="color:Tomato;">Hello World</h1>
    /* 设置边框颜色 */
    <h1 style="border:2px solid Tomato;">Hello World</h1>
    ```
    

---

**RGB颜色值，是用红色、绿色和蓝色混合形成不同颜色的表示方法。**

`rgb(red,green,bule)` 每个参数定义了十进制0-255之间的颜色强度，例如红色表示为`rgb(255,0,0)`，黑色表示为`rgb(0,0,0)`，白色表示为`rgb(255,255,255)`。

对于灰色阴影类颜色，通常使用3个相同的参数值来定义，例如：`rgb(60,60,60)`，`rgb(90,90,90)`等。

RGBA颜色值是指带有Alpha通道的RGB颜色值的拓展，Alpha通道定义了颜色的不透明度。Alpha参数是0.0（完全透明）-1.0（完全不透明）之间的数字。

- **样式示例**
    
    RGB颜色值：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/2dfcf44a-6636-4241-b7b1-8291a52f4048/Untitled.png)
    
    阴影：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/2d2a98f6-8d1d-4c23-afcb-40e5d0c9e392/Untitled.png)
    
    不透明度：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/1e717492-1fc7-4468-98d0-9b78eac1b22e/Untitled.png)
    

---

**HSL颜色值，是用色调、饱和度和亮度组合形成不同颜色的表示方法。**

`hsl(hue,saturation,lightness)` 色调是色轮上0-360的一个度数，0是红色、120是绿色、240是蓝色，是对颜色的选择；饱和度是一个百分比，0%表示灰度、100%表示全色，描述了颜色强度；亮度也是个百分比，0%为黑色、50%既不亮也不暗，100%为白色，描述了赋予颜色多少光；

对于灰色阴影类颜色，通常是将色调和饱和度设置为0来定义，通过调整亮度以获得更暗或者更亮的阴影。

HSLA颜色值是指带有Alpha通道的HSL颜色值的拓展，Alpha通道定义了颜色的不透明度。Alpha参数是0.0（完全透明）-1.0（完全不透明）之间的数字。

- **样式示例**
    
    HSL颜色值：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/27f327e9-984e-4da1-99bd-825df3926833/Untitled.png)
    
    饱和度：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/5fccc8a1-748c-4680-8b96-c3745c898afe/Untitled.png)
    
    亮度：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/d60a5a6c-9ee4-4737-b3ef-f425c7e634fb/Untitled.png)
    
    阴影：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/b5106982-c424-4464-b22e-f0f9c58e7817/Untitled.png)
    
    不透明度：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/3f75eedd-4e93-47b7-8d33-f6e6a62e672b/Untitled.png)
    

---

**HEX颜色值，是用红色、绿色和蓝色混合形成不同颜色的表示方法。**

`#rrggbb`每对参数定义了十六进制00-ff之间的颜色强度，例如红色表示为`#ff0000`，黑色为`#000000`，白色为`#ffffff`。

对于灰色阴影类颜色，通常使用3对2个相同的参数值来定义，例如：`#3c3c3c`，`#616161`等。

如果每对参数的两个值都相同，可以缩写为3位十六进制代码，例如：`#ff00cc`可以缩写为`#f0c`。

- **样式示例**
    
    HEX颜色值：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/cbc87ffc-51b7-45bd-b3e7-67b87270ea01/Untitled.png)
    
    阴影：
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/1a62b025-1408-4ef1-a153-2bbfc1a1de3c/Untitled.png)
    

`linear-gradient` 是一种 CSS 函数，用于生成一个渐变色。它创建的渐变是沿着一条直线（通常是水平、垂直或斜线）的颜色过渡效果。你可以通过指定颜色的变化以及渐变的方向来控制视觉效果。

### 基本语法

```css
css
复制编辑
background: linear-gradient(direction, color1, color2, ...);

```

- `direction`：渐变的方向，可以是角度（如 `45deg`），也可以是关键字（如 `to right`、`to bottom` 等）。
- `color1`, `color2`, ...：渐变的颜色，可以有多个颜色，从第一个颜色到最后一个颜色之间会平滑过渡。

### CSS背景

> CSS 背景属性用于为元素添加背景效果。

`background-color` 属性指定元素的背景颜色。

对于颜色的不透明度，可以使用`opacity` 属性指定，属性值为0.0（完全透明）-1.0（完全不透明）之间，但是当使用这个属性时，其所有的子元素都会继承这个属性，如果不想对子元素应用不透明度，可以使用带有Alpha通道的颜色值。

`background-image` 属性指定元素的背景图像。

语法：

```css
body {
  background-image: url("paper.gif");
}
```

默认情况下，图像会出现在左上方，并且在水平和垂直方向重复图像。

`background-repeat` 属性指定元素背景图像的重复动作。

属性值 `repeat-x` 表示水平方向重复；`repeat-y` 表示垂直方向重复，`no-repeat` 表示不重复。

`background-position`属性指定元素背景图像的位置。

属性值代表x轴和y轴上的位置，例如：right top表示右上，center center表示中间，center bottom表示中下。

`background-attachment` 属性指定元素背景图像滚动或固定。

有三个属性值：`scroll` 背景图像随着页面的滚动而滚动，默认值；`fixed`背景图像不会随着页面的滚动而滚动；`local`背景图像会随着元素的内容滚动而滚动。当元素的大小装不下元素内容时，元素就会出现滚动条（内滚动条），`scroll` 只会在外滚动条滚动时滚动；`local` 是在内外滚动条滚动时都会滚动。`fixed` 是内外滚动条滚动时都不滚动。

为了缩减代码，可以在一个属性中指定所有背景属性的属性值，称为速记属性，顺序为：

- `background-color`
- `background-image`
- `background-repeat`
- `background-attachment`
- `background-position`

```css
body {
  background: #ffffff url("img_tree.png") no-repeat fixed right top;
}
```

## 长度单位

在CSS中长度单位可以分为绝对长度和相对长度。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **绝对长度单位**

</aside>

绝对长度单位表示固定的长度，不会随上下文变化：

1. **`px`**（像素）：屏幕上的一个点。
2. **`cm`**（厘米）：1 厘米。
3. **`mm`**（毫米）：1 毫米。
4. **`in`**（英寸）：1 英寸等于 2.54 厘米。
5. **`pt`**（点）：1 点等于 1/72 英寸。
6. **`pc`**（派卡）：1 派卡等于 12 点。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **相对长度单位**

</aside>

相对长度单位根据上下文的不同而变化，适应性强：

1. **`em`**：相对于父元素或默认大小的字体为参照。1em 等于当前元素字体大小的 100%（默认16px）
2. **`rem`**：相对于根元素（`<html>` 元素）的字体大小。1rem 等于根元素字体大小的 100%。
3. **`%`**（百分比）：相对于父元素的尺寸。100% 等于父元素的全尺寸。
4. **`vw`**（视口宽度）：1vw 等于视口宽度的 1%。
5. **`vh`**（视口高度）：1vh 等于视口高度的 1%。
6. **`vmin`**：视口宽度和高度中较小的那个的 1%。
7. **`vmax`**：视口宽度和高度中较大的那个的 1%。
8. **`ex`**：相对于当前字体的 x-height，即字母 "x" 的高度。
9. **`ch`**：相对于当前字体的 "0" 字符的宽度。

# 选择器

> CSS 选择器选择想要设置样式的 HTML 元素。

## 简单选择器

### 元素选择器

元素选择器根据元素名称选择 HTML 元素。写法为直接写元素名。

```css
p {
  text-align: center;  
  color: red;
}
```

还可以根据元素属性进行更细粒度的筛选：

```css
p[name="p1"] {
  text-align: center;  
  color: red;
}
```

### id选择器

使用 HTML 元素的`id` 属性来选择元素，因为元素的 id 在页面内是唯一的，因此 id 选择器用于选择一个唯一的元素。写法为井号（#）后跟元素`id`属性值。

```css
#para1 {
  text-align: center;  color: red;} 将id属性值为para1的元素居中对齐，文本颜色设置为红色。
```

### 类选择器

使用HTML元素的`class`属性来选择元素，class属性不唯一，可以将相同样式的元素设置为同一`class`属性。写法为英文句号（.）后跟元素`class`属性值。

```css
.center {
  text-align: center;  color: red;} 将class属性值为center的元素居中对齐，文本颜色设置为红色。
```

还可以将范围限定在特定HTML元素类型中。

```css
p.center {
  text-align: center;  color: red;} 只有class属性值为center的<p>元素会将元素居中对齐，文本颜色设置为红色。
```

### 通用选择器

选择页面上所有的HTML元素。写法为星号（ *）。

```css
* {
  text-align: center;  color: blue;} 将页面上所有HTML元素居中对齐，文本设置为红色。
```

### 分组选择器

将具有相同样式的HTML元素合为一组，简化代码。写法为元素之间以逗号分隔。

```css
h1, h2, p {
  text-align: center;  color: red;} 将所有的<h1><h2><p>元素居中对齐，文本颜色设置为红色。
```

## 组合选择器

组合选择器是将简单选择器组合在一起的选择器，描述了选择器之间的关系。一个CSS选择器可以包含多个简单选择器，在简单选择器之间，可以包含一个组合器，形成组合选择器。

### 后代选择器

选择作为指定元素的后代元素（包括子孙代元素）。写法为指定元素与后代元素之间空格分隔。

```css
div p {
  background-color: yellow;} 将<div>元素内的所有<p>后代元素背景颜色设置为黄色。
```

### 子选择器

选择作为指定元素的子元素。写法为指定元素与子元素之间大于号（>）分隔。

```css
div > p {
  background-color: yellow;} 将<div>元素内的所有<p>子元素背景颜色设置为黄色。
```

### 相邻兄弟选择器

选择作为**紧接**在指定元素**之后**的相邻兄弟元素，仅选择紧接在指定元素后的第一个兄弟元素，不会选择其他兄弟元素，这两个兄弟元素必须相邻！写法为指定元素与相邻兄弟元素之间加号（+）分隔。

```css
div + p {
  background-color: yellow;} 将紧接在<div>元素之后的<p>兄弟元素背景颜色设置为黄色。
```

### 通用兄弟选择器

选择作为指定元素之后的所有兄弟元素，不管它们之间是否存在其它元素。写法为指定元素与兄弟元素之间波浪号（~）分隔。

```css
div ~ p {
  background-color: yellow;} 将<div>元素之后的所有<p>兄弟元素背景颜色设置为黄色。
```

## 伪类选择器

### 伪类定义

伪类用于定义元素的特殊状态。例如：鼠标悬停在元素上时、访问过的链接和未访问的链接、当元素获得焦点等等。语法为：

```css
selector:pseudo-class {
  property: value;}
```

### 所有CSS伪类

- `:link`
    
    用于选择未访问过的链接的状态。
    
- `:visited`
    
    用于选择已访问过的链接的状态。
    
- `:hover`
    
    用于选择鼠标悬停（hover）在元素上时的状态。
    
- `:focus`
    
    用于选择获得焦点的元素，当用户在页面上点击或使用键盘导航来选择某个可交互元素时，该元素就会被称为 “focused”（聚焦）。该伪类通常用于为用户与之交互的元素添加样式，以提高用户体验或增加可视化效果。
    
    ```css
    input:focus {
      outline: 2px solid blue; 
      /* 添加聚焦时的边框 */  
      background-color: lightyellow; 
      /* 聚焦时改变背景颜色 */
    }
    ```
    
- `:active`
    
    用于选择当前被用户激活（点击）的元素，常用在链接、按钮这些可被点击的元素上。`:active` 伪类只在用户按住鼠标点击时处于激活状态，一旦释放鼠标按钮，该状态就会结束。
    
    CSS 定义中 `a:hover` 必须位于 `a:link` 和 `a:visited` 之后才能生效！在 CSS 定义中， `a:active` 必须位于 `a:hover` 之后才能生效！
    
- `:lang(language)`
    
    根据不同语言，结合元素的`lang`属性，定义不同规则（样式）。
    
    ```css
    div:lang(zh) {
      font-family: "Microsoft YaHei", sans-serif; 
      /* 设置中文字体 */  
      color: #333; 
      /* 设置中文文本颜色 */
    }
    ...
    <div lang="zh">这是一段中文文本。</div>
    ```
    
- `:checked`
    
    用于选择被选中（用户点击了它）的表单元素，仅适用于单选按钮、复选框和**`*<option>*`**元素。这个伪类通常用于样式化表单元素的选中状态，以提高用户体验或增强可视化效果。
    
    ```css
    input:checked {
      /* 应用特定样式 */
    }
    ```
    
    还可以通过属性进行更细粒度的筛选：
    
    ```css
    input[type="checkbox"]:checked {
      /* 应用特定样式 */
    }
    ```
    
- `:enabled`
    
    用于选中**没有被禁用**（表单元素没有设置属性`disabled`，主要是**`*<input>*`**和**`*<button>*`**）的表单元素，这些表单元素可以和用户交互。
    
    ```css
    button:enabled {
      background-color: #4CAF50; /* 设置背景色 */  color: white; /* 设置文字颜色 */}
    ```
    
- `:empty`
    
    用于选中没有任何内容的空元素（没有子元素和文本节点，空格和换行符不会被视为内容）。
    
    ```css
    div:empty {
      border: 1px dashed red; /* 添加边框 */  background-color: #fdd; /* 设置背景色 */}
    ```
    
- `:first-child`
    
    选择作为其父元素的第一个子元素的指定元素。如`p:first-child`是匹配作为任意元素的第一个子元素的`<p>`_**元素，而不是选择**_`<p>`元素的第一个子元素。
    
    ```css
    /*匹配作为任何元素的第一个子元素的任何 <p> 元素*/p:first-child {
      color: blue;}
    /*匹配所有 <p> 元素中的第一个 <i> 后代元素，不管层级多深*/p i:first-child {
      color: blue;}
    /*匹配作为另一个元素的第一个子元素的 <p> 元素中的所有 <i> 后代元素*/p:first-child i {
      color: blue;}
    ```
    
- `:last-child`
    
    选择作为其父元素的最后一个子元素的指定元素。如`p:last-child`是匹配作为任意元素的第一个子元素的***`<p>`_**元素，而不是选择**_`<p>`***元素的第一个子元素。
    
    ```css
    /*匹配作为其父元素的最后一个子元素的任何 <p> 元素*/p:last-child {
      color: blue;}
    /*匹配所有 <p> 元素中的最后一个 <i> 后代元素，不管层级多深*/p i:last-child {
      color: blue;}
    /*匹配作为另一个元素的最后一个子元素的 <p> 元素中的所有 <i> 后代元素*/p:last-child i {
      color: blue;}
    ```
    
- `:only-child`
    
    选择作为其父元素唯一元素的指定元素。如`p:only-child`是匹配作为任意元素的唯一子元素的***`<p>`***元素。
    
    ```css
    /*匹配作为其父元素的唯一子元素的任何 <p> 元素*/p:only-child {
      color: blue;}
    /*匹配所有 <p> 元素中的唯一一个 <i> 后代元素，不管层级多深*/p i:only-child {
      color: blue;}
    /*匹配作为另一个元素的唯一子元素的 <p> 元素中的所有 <i> 后代元素*/
    p:only-child i {
      color: blue;}
    ```
    
- `:nth-child(n)`
    
    选择作为其父元素的第n个指定子元素。如`p:nth-child(2)`是匹配作为任意元素的第二个子元素的***`<p>`***元素。n可以是数字、关键字（odd或even）或者公式（an + b）：a表示周期大小，n是计数器（从0开始），b是偏移值）。
    
    ```css
    /* 选择作为同一父元素下第二个子元素的<p>元素 */
    div:nth-child(2) {
      background: red;}
    /* 选择作为同一父元素下第二个子元素的<li>元素 */li:nth-child(2) {
      background: lightgreen;}
    /* 选择作为同一父元素下第二个子元素的所有元素 */:nth-child(3) {
      background: yellow;}
    ```
    
- `:nth-last-child(n)`
    
    选择作为其父元素的第n个子元素，类似于`:nth-child(n)`，不同的是`:nth-last-child(n)`是从后往前数。如`p:nth-last-child(2)`是匹配作为任意元素从后往前数的第二个子元素的***`<p>`***元素。n可以是数字、关键字（odd或even）或者公式（an + b）：a表示周期大小，n是计数器（从0开始），b是偏移值）。
    
    ```css
    /* 选择作为同一父元素下从后往前数的第二个元素的<p>元素 */p:nth-last-child(2) {
      background: red;}
    /* 选择同一父元素下从后往前数的奇数<p>元素 */p:nth-last-child(odd) {
      background: red;}
    /* 选择同一父元素下从后往前数索引为3的倍数的<p>元素 */p:nth-last-child(3n+0) {
      background: red;}
    ```
    
- `:first-of-type`
    
    选择其父元素的第一个指定类型的子元素，不管该元素之前是否有其它元素。在下面的例子中，只会选中内容为”第一个段落”的***`<p>`***元素。
    
    ```css
    p:first-of-type {
      color: red;}
    ...
    <div>  <span>第一个span</span>  <p>第一个段落</p>  <p>第二个段落</p>  <p>第三个段落</p></div>
    ```
    
- `:last-of-type`
    
    选择其父元素的最后一个指定类型的子元素，不管该元素之后是否有其它元素。在下面的例子中，只会选中内容为”第三个段落”的***`<p>`***元素。
    
    ```css
    p:last-of-type {
      color: red;}
    ...
    <div>  <p>第一个段落</p>  <p>第二个段落</p>  <p>第三个段落</p>  <span>第一个span</span></div>
    ```
    
- `:only-of-type`
    
    选择其父元素的唯一指定类型的子元素，不管该元素前后是否有其它元素。在下面的例子中，只会选中内容为”第三个段落”的***`<p>`***元素。
    
    ```css
    p:only-of-type {
      color: red;}
    ...
    <div>  <p>第一个段落</p>  <p>第二个段落</p></div><div>  <p>第三个段落</p>  <span>第一个span</span></div>
    ```
    
- `:nth-of-type(n)`
    
    选择其父元素的第n个指定类型的子元素，不管该元素之前是否有其它元素。在下面的例子中，只会选中内容为”第二个段落”的***`<p>`***元素。n可以是数字、关键字（odd或even）或者公式（an + b）：a表示周期大小，n是计数器（从0开始），b是偏移值）。
    
    ```css
    p:nth-of-type(2) {
      color: red;}
    ...
    <div>  <span>第一个span</span>  <p>第一个段落</p>  <p>第二个段落</p>  <p>第三个段落</p></div>
    ```
    
- `:nth-last-of-type(n)`
    
    选择其父元素从后往前数的第n个子元素，不管该元素之后是否有其它元素。在下面的例子中，只会选中内容为”第三个段落”的***`<p>`***元素。n可以是数字、关键字（odd或even）或者公式（an + b）：a表示周期大小，n是计数器（从0开始），b是偏移值）。
    
    ```css
    p:nth-last-of-type(2) {
      color: red;}
    ...
    <div>  <span>第一个span</span>  <p>第一个段落</p>  <p>第二个段落</p>  <p>第三个段落</p>  <p>第四个段落</p>  <span>第二个span</span></div>
    ```
    
- `:in-range`
    
    用于选择值在指定范围的元素。仅适用于具有 `min` 和/或 `max` 属性的输入元素。
    
    ```css
    input:in-range {
      border: 2px solid yellow;}
    ```
    
- `:out-of-range`
    
    用于选择值超出指定范围的元素。仅适用于具有 `min` 和/或 `max`属性的输入元素。
    
    ```css
    input:out-of-range {
      border: 2px solid red;}
    ```
    
    - `:invalid`
        
        用于选择表单元素具有无效值的元素。仅适用于有限制的表单元素，例如具有 min 和 max 属性的输入元素、电子邮件字段、数字字段等。
        
        ```css
        input:invalid {
          border: 2px solid red;}
        ```
        
- `:valid`
    
    用于选择表单元素具有有效值的元素。仅适用于有限制的表单元素，例如具有 min 和 max 属性的输入元素、电子邮件字段、数字字段等。
    
    ```css
    input:valid {
      background-color: yellow;}
    ```
    
- `:not(selector)`
    
    选择不是指定元素或不是选择器选中的元素，提供了选择器的非集。
    
    ```css
    :not(p) {
      background: #ff0000;}
    ```
    
- `:disabled`
    
    用于选中**被禁用**（表单元素设置了属性`disabled`，主要是***`<input>`_**和**_`<button>`** *）的表单元素，这些表单元素无法和用户交互，提交时也不会将这些表单元素的数据发往处理程序。
    
    ```css
    button:disabled {
      opacity: 0.5; /* 设置透明度 */  cursor: not-allowed; /* 设置鼠标样式为不允许 */}
    ```
    
- `:optional`
    
    用于选择可选的表单元素，即这个表单元素不是必填的（没有`required`属性），仅适用于***`<input>`_**、**_`<select>`_**和**_`<textarea>`***元素。
    
    ```css
    input:optional {
      background-color: yellow;}
    ```
    
- `:required`
    
    用于选择必选的表单元素，即这个表单元素是必填的（设置了`required`属性），仅适用于***`<input>`_**、**_`<select>`_**和**_`<textarea>`***元素。
    
    ```css
    input:required {
      background-color: yellow;}
    ```
    
- `:read-only`
    
    用于选择只读的表单元素，即这个表单元素不能被用户输入（设置了`readonly`属性）。
    
    ```css
    input:read-only {
      background-color: yellow;}
    ```
    
- `:read-write`
    
    用于选择可读和可写的表单元素，即这个元素没有被设置`readonly`和`disabled`属性。
    
    ```css
    input:read-write {
      background-color: yellow;}
    ```
    
- `:root`
    
    用于选择文档的根元素，在HTML中，根元素始终是***`<html>`***元素。
    
    ```css
    :root {
      background: #ff0000;}
    ```
    
- `:target`
    
    用于选择链接书签的目标元素，在超链接元素中，我们可以使用链接书签，如 `<a href="#c4">Jump to HTML Links</a>` 在点击该超链接之后，可以跳转到同一页面中`id`为c4的元素上，这个元素就称为目标元素。
    
    ```css
    :target {
      border: 2px solid #d4d4d4;
      background-color: #e5eecc;
    }
    ```
    
- `:has(n)`
    
    只有元素包含n的时候才会匹配
    

### 伪类与HTML类

在 CSS 中，有些伪类可以单独使用，而有些则必须与其他选择器一起使用。这取决于伪类的定义以及它们所代表的状态或行为。

1. **可以单独使用的伪类**：
    - `:hover`：用于选择鼠标悬停在元素上时的状态，可以单独应用于任何元素。
    - `:focus`：用于选择当前获取焦点的元素，可以单独应用于表单元素等。
    - `:active`：用于选择用户点击时的元素，可以单独应用于链接和按钮等可点击元素。
2. **必须与其他选择器一起使用的伪类**：
    - `:link` 和 `:visited`：用于选择链接元素的状态（未访问和已访问），必须与链接选择器一起使用，如 `a:link` 或 `a:visited`。
    - `:first-child` 和 `:last-child`：用于选择元素的第一个子元素和最后一个子元素，必须与父元素选择器一起使用。
    - `:nth-child`：用于选择指定位置的子元素，必须与父元素选择器一起使用。

因此，大多数伪类都可以单独使用，但有些特定的伪类需要与其他选择器一起使用才能生效。

## 伪元素选择器

### 伪元素定义

伪元素用于定义元素的指定部分样式，比如：设置元素的第一个字母或行的样式、在元素之前或之后插入内容等等。语法为：

```css
selector::pseudo-element {
  property: value;
}
```

### 所有伪元素

- `::first-line`
    
    用于选择元素内容的第一行。只能应用于块级元素。
    
    ```css
    p::first-line {
      color: #ff0000;
      font-variant: small-caps;
    }
    ```
    
- `::first-letter`
    
    用于选择元素内容的第一个字母。只能应用于块级元素。
    
    ```css
    p::first-letter {
      color: #ff0000;
      font-size: xx-large;
    }
    ```
    
- `::before`
    
    用于在元素内容之前插入内容。
    
    ```css
    h1::before {
      content: url(smiley.gif);
    }
    ```
    
- `::after`
    
    用于在元素内容之后插入内容。
    
    ```css
    h1::after {
      content: url(smiley.gif);
    }
    ```
    
- `::marker`
    
    用于选择列表项的标记。
    
    ```css
    ul li::marker {
      color: red;
      font-size: 23px;
    }
    ```
    
- `::selection`
    
    用于匹配用户选择的元素部分。例如要复制一段话，需要先将这段话选择（按住右键划选）。
    
    ```css
    p::selection {
      color: red;
      background: yellow;
    }
    ```
    
- `::-webkit-scrollbar`
    
    它用于自定义网页中的滚动条样式，尤其是在基于 WebKit 引擎（如 Google Chrome 和 Safari）的浏览器中。它允许你对滚动条的各个部分进行样式设置，比如滚动条的整体外观、轨道、滑块等。
    
    - `::-webkit-scrollbar` - 用于控制整个滚动条的宽度和高度。
    - `::-webkit-scrollbar-thumb` - 用于控制滚动条的“滑块”（即可拖动的部分）。
    - `::-webkit-scrollbar-track` - 用于控制滚动条的“轨道”（即滑块所在的轨道背景）。
    - `::-webkit-scrollbar-button` - 用于控制滚动条的按钮部分（通常位于滚动条的顶部和底部）。
    - `::-webkit-scrollbar-corner` - 用于控制滚动条的交叉区域（当水平和垂直滚动条都存在时，交叉部分的区域）。

### 伪元素和HTML类

所有伪元素都可以单独使用，也可以结合其它选择器，进行更细粒度的筛选。

## 属性选择器

> 可以对具有特定属性或属性值的元素进行选择。

- `[attribute]`
    
    存在属性选择器。用于选择具有指定属性的元素。
    
    ```css
    a[target] {
      background-color: yellow;
    }
    ```
    
- `[attribute="value"]`
    
    属性值等于选择器。用于选择具有指定属性和属性值的元素。
    
    ```css
    a[target="_blank"] {
      background-color: yellow;
    }
    ```
    
- `[attribute~="value"]`
    
    属性值包含单词（独立单词）选择器。用于选择属性值包含指定单词的元素。
    
    ```css
    img[title~="flower"] {
      border: 5px solid yellow;
    }
    ```
    
- `[attribute|="value"]`
    
    属性值以指定值开头（包括连字符）选择器。用于选择属性值以指定值开头后跟连字符（-），或完全匹配指定值的元素。该指定值必须是完整单词，可以单独使用，也可以后跟连字符。
    
    ```css
    [class|="top"] {
      background: yellow;
    }
    ...
    <h1 class="top-header">Welcome</h1>
    <p class="top-text">Hello world!</p>
    <p class="top">Are you learning CSS?</p>
    ```
    
- `[attribute^="value"]`
    
    属性值以指定值开头选择器。用于选择属性值是以指定值开头的元素，或完全匹配的元素。该指定值不需要是完整单词。
    
    ```css
    [class^="top"] {
      background: yellow;
    }
    ...
    <h1 class="top-header">Welcome</h1>
    <p class="top-text">Hello world!</p>
    <p class="topcontent">Are you learning CSS?</p>
    <p class="top">Yes，I learning CSS</p>
    ```
    
- `[attribute$="value"]`
    
    属性值以指定值结尾选择器。用于选择属性值以指定值结尾的元素。该指定值不需要是完整单词。
    
    ```css
    [class$="test"] {
      background: yellow;
    }
    ...
    <div class="first_test">The first div element.</div>
    <div class="my-test">The third div element.</div>
    <p class="mytest">This is some text in a paragraph.</p>
    ```
    
- `[attribute*="value"]`
    
    属性值包含指定值选择器。用于选择属性值包含指定值的元素。该指定值不需要是完整单词。
    
    ```css
    [class*="te"] {
      background: yellow;
    }
    ...
    <div class="first_test">The first div element.</div>
    <div class="my-test">The third div element.</div>
    <p class="mytest">This is some text in a paragraph.</p>
    ```
    

# 文本和字体

> CSS 有很多用于格式化文本的属性。

## 文本

### 文本颜色

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **文本颜色**

</aside>

使用**`color`**设置文本颜色

### 文本对齐

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **文本对齐**

</aside>

`text-align` 属性用于设置文本的水平对齐方式。

属性值可以是左对齐（left）、右对齐（right）、居中对齐（center）、左右对齐（justify）

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **文本最后对齐**

</aside>

`text-align-last` 属性指定如何对齐文本的最后一行，属性值同上。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **文字方向**

</aside>

`direction` 和 `unicode-bidi` 属性可用于更改元素的文本方向，以处理不同书写方向的语言。

`direction`属性值有`ltr`（从左到右）、`rtl` （从右到左）、`inherit` （从父元素继承）

`unicode-bidi`属性提供更细粒度控制，属性值有：

- **`normal`**：默认值，按正常双向文本（混合了不同书写方向的文本）算法显示。英语等语言从左向右，阿拉伯文等语言从右向左，如果混合时，两种语言方向不变，标点符号根据文本方向变化。
    
    ```jsx
    <div style="direction: rtl; unicode-bidi: normal;">
        Mixed English and العربية.
    </div>
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/721020ae-bb18-46d0-8835-8aec14afb551/Untitled.png)
    
- **`embed`** ：将一段文本嵌入到一个新的双向文本上下文中，而不影响周围文本。
    
    ```jsx
    <div style="direction: rtl;">
        هذا نص عربي <span style="unicode-bidi: embed; direction: ltr;">Embedded English text</span> هنا.
    </div>
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/02329e01-b324-469c-9744-25f626f0d423/Untitled.png)
    
- **`bidi-override`** ：使文本覆盖正常的双向文本算法，使文本严格按照指定方向显示。
    
    ```jsx
    <div style="direction: rtl; unicode-bidi: bidi-override;">
        Mixed English and العربية.
    </div>
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/95b2cb93-f58e-4360-8197-af425c410920/Untitled.png)
    
- **`isolate`** ：隔离元素内的双向文本，使其不影响外部文本。
    
    ```jsx
    <div>
        This is some English text <span style="unicode-bidi: isolate; direction: rtl;">مع بعض النص العربي</span> more English text.
    </div>
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/d7eac236-59a0-4abf-8bf4-48158dd4db86/Untitled.png)
    
- **`isolate-override`**：结合 `isolate` 和 `bidi-override` 的效果，使文本既被隔离又覆盖正常的双向文本算法。
    
    ```jsx
    <div>
        Some English text <span style="unicode-bidi: isolate-override; direction: rtl;">Mixed العربية and English</span> more English text.
    </div>
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/6eb48ceb-28b0-4f0b-8d50-b8575a1fb404/Untitled.png)
    
- **`plaintext`**：文本按单一方向显示，不考虑内部字符方向。
    
    ```jsx
    <div style="unicode-bidi: plaintext;">
        This is some text. هذا نص عربي.
    </div>
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/732efcb2-4cc8-4209-8e32-73d1bbe32929/Untitled.png)
    

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **垂直对齐**

</aside>

`vertical-align` 属性用于在行内元素、表格单元格或对齐框中元素的垂直对齐方式。这个属性通常用于对齐行内元素（如图像、文本或其他行内块级元素）相对于其周围文本的垂直位置。属性值有：

- baseline：默认值，将元素的基线与其父元素的基线对齐。
- sub：将元素垂直对齐为下标（subscript）。
- super：将元素垂直对齐为上标（superscript）。
- text-top：将元素的顶端与其父元素的字体顶部对齐。
- text-bottom：将元素的底端与其父元素的字体底部对齐。
- middle：将元素的中线与父元素的中线对齐。
- top：将元素的顶端与其包含块的顶端对齐。
- bottom：将元素的底端与其包含块的底端对齐。
- 百分比值：使用一个相对于元素行高的百分比值，来调整元素的垂直位置。

### 文本装饰

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **装饰线**

</aside>

`text-decoration-line` 属性用于向文本添加装饰线。

- none：默认值，无装饰线，可以**用于删除超链接的默认下划线**。
- underline：文本下方的下划线。
- overline：文本上方的上划线。
- line-through：文本中间的删除线（贯穿线）。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **装饰线颜色**

</aside>

`text-decoration-color` 属性用于设置装饰线的颜色。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **装饰线样式**

</aside>

`text-decoration-style` 属性用于设置装饰线的样式。

- **`solid`**：实线（默认值）。
- **`double`**：双线。
- **`dotted`**：点线。
- **`dashed`**：虚线。
- **`wavy`**：波浪线。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **装饰线粗细**

</aside>

`text-decoration-thickness` 属性用于设置装饰线的粗细。

- **自动（`auto`）**：由浏览器自动确定装饰线的厚度。通常与字体大小相关。
- **长度值（`<length>`）**：可以使用具体的长度单位（如 `px`, `em`, `rem` 等）来指定装饰线的厚度。
- **百分比值（`<percentage>`）**：指定为字体大小的百分比。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **装饰线简写**

</aside>

`text-decoration` 属性是以上属性的简写属性。

`text-decoration-line` 属性是必须的，其他三个属性可选。

```css
p {
  text-decoration: underline red double 5px;
}
```

### 文本转换

`text-transform` 属性用于指定文本中的大写和小写字母。

- **`none`**：不进行任何大小写转换（默认值）。
- **`capitalize`**：将每个单词的首字母转换为大写。
- **`uppercase`**：将所有字母转换为大写。
- **`lowercase`**：将所有字母转换为小写。
- **`full-width`**：将字符转换为全角字符（占用两个标准字符宽度的字符）。这个值主要用于东亚文本。主要作用为文本美观和格式统一。

### 文本间距

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **文本缩进**

</aside>

`text-indent` 属性用于指定文本第一行的缩进。

- **长度值（`<length>`）**：指定具体的缩进值，可以使用像素（`px`）、百分比（`%`）、`em` 等单位。
- **百分比值（`<percentage>`）**：相对于包含块的宽度来计算缩进值。
- **负值**：可以使用负值来使文本向左缩进。

```css
p {
  text-indent: 50px;
}
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **字母间距**

</aside>

`letter-spacing` 属性用于指定文本中字符之间的间距。

- **正常（`normal`）**：使用默认的字符间距。这是默认值。
- **长度值（`<length>`）**：指定具体的字符间距，可以使用像素（`px`）、百分比（`%`）、`em` 等单位。
- **负值**：拉近字母距离。

```css
h2 {
  letter-spacing: -2px;
}
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **行高**

</aside>

`line-height` 属性用于指定行之间的间距。

- **普通（`normal`）**：默认值，行高由浏览器计算，一般是字体大小的 1.2 至 1.5 倍之间。
- **数字值**：设置为一个数字，此数字会与元素的字体大小相乘以计算行高，也就是字体大小的倍数
- **长度值（`<length>`）**：可以使用具体的长度单位，如 `px`, `em`, `rem` 等。
- **百分比值（`<percentage>`）**：行高为字体大小的百分比。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **字间距**

</aside>

`word-spacing` 属性用于指定文本中单词之间的间距。

- **正常（`normal`）**：使用默认的单词间距。这是默认值。
- **长度值（`<length>`）**：可以使用具体的长度单位（如 `px`, `em`, `rem` 等）来指定单词间距。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **空白字符**

</aside>

`white-space` 属性指定如何处理元素内的空白字符（空格、制表符、换行符）。

- **`normal`**：合并空白字符，并根据需要换行。这是默认值。
- **`nowrap`**：合并空白字符，并防止换行。
- **`pre`**：保留空白字符，并保留换行符。文本会按照原始格式显示（类似于 HTML 的 `<pre>` 元素）。
- **`pre-wrap`**：保留空白字符，并保留换行符，同时允许文本换行以适应容器宽度。
- **`pre-line`**：合并空白字符，并保留换行符，同时允许文本换行以适应容器宽度。
- **`break-spaces`**：类似于 `pre-wrap`，但保留的空白字符包括所有类型的空白字符（如 U+2028 行分隔符）。

### 文本阴影

`text-shadow` 属性为文本添加阴影。需要指定水平阴影（向右偏移）和垂直阴影（向下偏移）。

```css
h1 {
  text-shadow: 2px 2px;
}
```

可以为负值，代表向反方向偏移。

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/fe8c38b6-eead-49e4-ae52-2c3e8cdc9bf9/Untitled.png)

可以给阴影添加颜色

```css
h1 {
  text-shadow: 2px 2px red;
}
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/f03e1fb7-edf7-47fb-8013-8b1b18cc8181/Untitled.png)

可以给阴影添加模糊效果：

```css
h1 {
  text-shadow: 2px 2px 5px red;
}
```

模糊半径决定了阴影的模糊程度，值越大越模糊，0表示没有模糊。

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/84605e79-28d6-447d-b764-2f7f70935ca1/Untitled.png)

多个阴影效果使用逗号分隔。

`box-shadow` 是 CSS 中用来为元素添加阴影效果的属性。它允许你为元素的框架（即盒子模型）添加一或多个阴影，常用于提高元素的立体感或视觉层次感。

### 基本语法

```css
css
复制编辑
box-shadow: h-offset v-offset blur spread color inset;

```

- `h-offset`：阴影的水平偏移量，正值表示向右偏移，负值表示向左偏移。
- `v-offset`：阴影的垂直偏移量，正值表示向下偏移，负值表示向上偏移。
- `blur`：模糊半径，数值越大阴影越模糊，越小则阴影越清晰。如果不指定，默认为 `0`，即没有模糊效果。
- `spread`：阴影扩展的大小，正值使阴影扩大，负值则收缩阴影。
- `color`：阴影的颜色，可以使用任意合法的 CSS 颜色值（例如，`#000`、`rgba(0, 0, 0, 0.5)` 等）。
- `inset`（可选）：可选关键字，指定阴影是内阴影（从元素内部投射）而非外阴影。默认是外阴影。

## 字体

### 字体系列

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **通用字体（Generic Font Families）**

</aside>

Generic Font Families 是一组宽泛的字体类别，用于在 CSS 中指定字体时作为最后的后备选项。当指定的具体字体不可用时，浏览器会选择与这些泛型字体族对应的默认字体。使用泛型字体族可以确保网页在各种设备和操作系统上都有合理的字体显示效果。

- **Serif** fonts（衬线字体）
- **Sans-serif** fonts（无衬线字体）
- **Monospace** fonts（等宽字体）
- **Cursive** fonts（草书字体）
- **Fantasy** fonts（幻想字体）

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/34bfb71b-63cd-4c03-95a5-00dc0da76297/Untitled.png)

```css
.p1 {
  font-family: "Times New Roman", Times, serif;
}
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **字体属性**

</aside>

`font-family` 属性来指定文本的字体。

如果字体名称超过一个单词，则必须用引号引起来，例如：“Times New Roman”。

`font-family` 属性应包含多个字体名称作为“后备”系统，以确保浏览器/操作系统之间的最大兼容性，以想要的字体开始，用通用字体进行兜底。字体名称应以逗号分隔。

```css
.p3 {
  font-family: "Lucida Console", "Courier New", monospace;
}
```

### 网络安全字体

网络安全字体（Web Safe Fonts）是一组在不同操作系统和浏览器中普遍存在并且显示效果一致的字体。使用这些字体可以确保网页在各种设备和平台上都能有相同的外观和可读性。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **后备字体**

</aside>

不存在 100% 完全网络安全的字体。总是有可能找不到字体或未正确安装字体，因此，始终使用后备字体非常重要。当指定的具体字体不可用时，浏览器会选择与这些泛型字体族对应的默认字体。

始终以通用字体系列名称结束列表。

```css
p {
font-family: Tahoma, Verdana, sans-serif;
}
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **常见的网络安全字体**

</aside>

- **Arial** - 通常用于无衬线字体。Arial 是在线和印刷媒体使用最广泛的字体
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/e75cf065-37d9-4dc2-ae93-a6e435272c55/Untitled.png)
    
- **Helvetica** - 类似于 Arial，常见于 Mac 系统。
    
- **Verdana** - 专为屏幕显示设计的无衬线字体，字间距较宽。
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/a3cc8cc0-b7ad-4042-bf76-79405c230905/Untitled.png)
    
- **Tahoma** - 类似于 Verdana，但字间距稍小。
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/dfbfd39d-7e56-417a-aaf0-84db59170398/Untitled.png)
    
- **Trebuchet MS** - 现代无衬线字体。
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/f0b3767c-3a0a-493a-a0ad-8b60e83bd051/Untitled.png)
    
- **Times New Roman** - 传统衬线字体，常用于正式文档。
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/024761f2-4165-46b2-a062-1ba0f5bcffc1/Untitled.png)
    
- **Georgia** - 专为屏幕显示设计的衬线字体。
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/a87fca39-197a-4e4b-a615-ab7cbaee029c/Untitled.png)
    
- **Garamond** - 经典的衬线字体。
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/14e0190a-b903-4298-bf09-d60c061dd548/Untitled.png)
    
- **Courier New** - 等宽字体，常用于代码显示。
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/989ca371-f0f9-44e4-beea-50c9f3c1b981/Untitled.png)
    
- **Brush Script MT** - 手写风格字体
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/019f6831-1e2a-45d3-8df0-71d6295a9156/Untitled.png)
    

### 字体样式

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **字体样式**

</aside>

`font-style` 属性主要用于指定文本的斜体、倾斜或常规样式。

- **normal** - 正常字体样式（默认值），即没有斜体或倾斜效果
- **italic** - 斜体样式，通常用于强调或引文。
- **oblique** - 倾斜样式，类似于斜体，但它通过倾斜文本而不是使用特定的斜体字体来实现效果。
- **oblique `<angle>`** - 可以指定一个具体的角度来倾斜文本。这个属性值在一些现代浏览器中得到支持。
- **inherit** - 继承父元素的字体样式。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **字体粗细**

</aside>

`font-weight` 属性指定字体的粗细

- **`normal`**：标准字重，通常对应于 400。
- **`bold`**：加粗字重，通常对应于 700。
- **`bolder`**：相对于父元素的字体更粗。
- **`lighter`**：相对于父元素的字体更细。
- **`数值`**：范围在 100 到 900 之间，每 100 为一个增量 并非所有字体都支持所有的 `font-weight` 值，某些字体可能只有 `normal` 和 `bold` 两个字重，其他中间值会由浏览器自动模拟。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **字体变体**

</aside>

`font-variant` 属性用于控制字体显示的小型大写字母（small-caps）以及其他与字体变体相关的属性。

- `normal`：正常显示（默认值）。
- `small-caps`：小型大写字母，所有小写字母都会转换为大写字母，转换后的大写字母以比文本中原始大写字母更小的字体大小显示

在 CSS Fonts Level 3 和更高版本中，`font-variant` 属性被拆分为多个更具体的属性，以更细粒度地控制不同字体变体效果。这些属性包括：

1. **`font-variant-caps`**: 控制大写字母的不同变体形式。
    - `normal`：正常显示（默认值）。
    - `small-caps`：小型大写字母。
    - `all-small-caps`：所有字母都使用小型大写字母。
    - `petite-caps`：小型大写字母（较小）。
    - `all-petite-caps`：所有字母都使用较小的小型大写字母。
    - `unicase`：大小写不区分的单一形式。
    - `titling-caps`：用于标题的大写字母形式。
2. **`font-variant-numeric`**: 控制数字的不同显示形式。
    - `normal`：正常显示（默认值）。
    - `lining-nums`：使用对齐的数字。
    - `oldstyle-nums`：使用老式风格的数字。
    - `proportional-nums`：使用比例宽度的数字。
    - `tabular-nums`：使用等宽的数字。
    - `diagonal-fractions`：使用斜分数形式。
    - `stacked-fractions`：使用堆叠分数形式。
3. **`font-variant-ligatures`**: 控制连字的使用。
    - `normal`：默认连字行为。
    - `none`：禁用连字。
    - `common-ligatures`：启用常见连字。
    - `no-common-ligatures`：禁用常见连字。
    - `discretionary-ligatures`：启用选择性连字。
    - `no-discretionary-ligatures`：禁用选择性连字。
    - `historical-ligatures`：启用历史连字。
    - `no-historical-ligatures`：禁用历史连字。
    - `contextual`：启用上下文连字。
    - `no-contextual`：禁用上下文连字。
4. **`font-variant-east-asian`**: 控制东亚字体的不同显示形式。
    - `normal`：正常显示（默认值）。
    - `ruby`：使用ruby注音形式。
    - `jis78`、`jis83`、`jis90`、`jis04`：使用不同 JIS 标准的字符形式。
    - `simplified`：使用简体字。
    - `traditional`：使用繁体字。
    - `full-width`：使用全角字符。
    - `proportional-width`：使用比例宽度字符。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **字体大小**

</aside>

`font-size` 是 CSS 中用于设置文本字体大小的属性。

参照[CSS长度单位](https://www.notion.so/CSS3-fb952830f9394aacb4738ac204e9ba88?pvs=21)

CSS中字体默认大小为16px。

### 谷歌字体

谷歌字体是一个拥有1000余种字体的免费字体库。

[https://fonts.google.com/](https://fonts.google.com/)

[https://www.googlefonts.cn/](https://www.googlefonts.cn/)

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **使用谷歌字体**

</aside>

只需要在HTML中引入谷歌字体相对应的字体库即可。

比如引入“**Sofia**”这个字体：

```html
<head>
<link rel="stylesheet" href="<https://fonts.googleapis.com/css?family=Sofia>">
<style>
body {
  font-family: "Sofia", sans-serif;
}
</style>
</head>
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/14460545-9e73-4ac7-959c-d1aa0f40c25a/Untitled.png)

如果要引入多种谷歌字体时，只需要竖线（**`|`**）分割。

```html
<head>
<link rel="stylesheet" href="<https://fonts.googleapis.com/css?family=Audiowide|Sofia|Trirong>">
<style>
h1.a {font-family: "Audiowide", sans-serif;}
h1.b {font-family: "Sofia", sans-serif;}
h1.c {font-family: "Trirong", serif;}
</style>
</head>
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/deb9e680-d693-4b99-a203-e7bebf6b7895/Untitled.png)

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **谷歌字体效果**

</aside>

引入谷歌字体后可以自己添加字体样式，如字体大小、文本阴影之类的。

```html
<head>
<link rel="stylesheet" href="<https://fonts.googleapis.com/css?family=Sofia>">
<style>
body {
  font-family: "Sofia", sans-serif;
  font-size: 30px;
  text-shadow: 3px 3px 3px #ababab;
}
</style>
</head>
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/e040ffc5-9c07-4f50-a9ad-60ba4e86d0d2/Untitled.png)

也可以使用谷歌字体预设的字体效果。

首先将 `effect=*effectname*` 添加到 Google API，然后将特殊的类名称添加到要使用特殊效果的元素。类名始终以 `font-effect-` 开头并以 `*effectname*` 结尾。

```html
<head>
<link rel="stylesheet" href="<https://fonts.googleapis.com/css?family=Sofia&effect=fire>">
<style>
body {
  font-family: "Sofia", sans-serif;
  font-size: 30px;
}
</style>
</head>
<body>

<h1 class="font-effect-fire">Sofia on Fire</h1>

</body>
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/df996c6b-bb4d-469d-85fc-de0c57a0eeb3/Untitled.png)

要请求多种字体效果，只需用竖线字符 ( **`|`** ) 分隔效果名称

```html
<head>
<link rel="stylesheet" href="<https://fonts.googleapis.com/css?family=Sofia&effect=neon|outline|emboss|shadow-multiple>">
<style>
body {
  font-family: "Sofia", sans-serif;
  font-size: 30px;
}
</style>
</head>
<body>

<h1 class="font-effect-neon">Neon Effect</h1>
<h1 class="font-effect-outline">Outline Effect</h1>
<h1 class="font-effect-emboss">Emboss Effect</h1>
<h1 class="font-effect-shadow-multiple">Multiple Shadow Effect</h1>

</body>
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/01888824-9265-4702-aacd-5c1052822f0f/Untitled.png)

### 属性简写

`font` 属性是以下属性的简写属性：

- `font-style`
- `font-variant`
- `font-weight`
- `font-size/line-height`
- `font-family`

`font-size` 和 `font-family` 值是必需的。如果缺少其他值之一，则使用它们的默认值。

```css
p.a {
  font: 20px Arial, sans-serif;
}

p.b {
  font: italic small-caps bold 12px/30px Georgia, serif;
}
```

# 图像

## 云图标

在HTML中添加图标最简易的方法是使用图标库。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **Font Awesome Icons**

</aside>

**使用 Font Awesome Kits 引入图标库：**

在`<head>`元素中添加

`<script src="<https://kit.fontawesome.com/*yourcode*.js>" crossorigin="anonymous"></script>` （每个注册用户都有不同的url）

```html
<!DOCTYPE html>
<html>
<head>
<script src="<https://kit.fontawesome.com/a076d05399.js>" crossorigin="anonymous"></script>
</head>
<body>

<i class="fas fa-cloud"></i>
<i class="fas fa-heart"></i>
<i class="fas fa-car"></i>
<i class="fas fa-file"></i>
<i class="fas fa-bars"></i>

</body>
</html>
```

**通过CDN引入图标库：**

```html
<head>
  <!-- Font Awesome Free 6.0.0 -->
  <link rel="stylesheet" href="<https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css>">
</head>
```

使用图标如上

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **Bootstrap Icons**

</aside>

CDN引入：

```html
<head>
    <link rel="stylesheet" href="<https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css>">
</head>
```

使用：

```html
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" href="<https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css>">
</head>
<body>

<i class="glyphicon glyphicon-cloud"></i>
<i class="glyphicon glyphicon-remove"></i>
<i class="glyphicon glyphicon-user"></i>
<i class="glyphicon glyphicon-envelope"></i>
<i class="glyphicon glyphicon-thumbs-up"></i>

</body>
</html>
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **Google Icons**

</aside>

CDN引入：

```html
<head>
    <link rel="stylesheet" href="<https://fonts.googleapis.com/icon?family=Material+Icons>">
</head>
```

使用：

```html
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" href="<https://fonts.googleapis.com/icon?family=Material+Icons>">
</head>
<body>

<i class="material-icons">cloud</i>
<i class="material-icons">favorite</i>
<i class="material-icons">attachment</i>
<i class="material-icons">computer</i>
<i class="material-icons">traffic</i>

</body>
</html>
```

## 鼠标光标

鼠标光标样式通过`cursor`属性定义，属性值有及效果查看：[https://www.w3schools.com/css/tryit.asp?filename=trycss_cursor](https://www.w3schools.com/css/tryit.asp?filename=trycss_cursor)

### **`object-fit` 详解**

`object-fit` 是一个 CSS 属性，**用于控制 `img`、`video` 或 `object` 如何适应其容器**，特别适用于 `width` 和 `height` 已固定的情况下。

---

## **1. `object-fit` 的常见取值**

|值|解释|
|---|---|
|`fill` (默认)|图片拉伸填充整个容器，可能会变形。|
|`contain`|保持原始比例缩放，使整个图片可见，可能会有空白区域。|
|`cover`|保持原始比例缩放，使图片完全覆盖容器，可能会被裁剪。|
|`none`|图片不缩放，超出部分会溢出容器。|
|`scale-down`|取 `none` 和 `contain` 之间的最小尺寸。|

---

## **2. `object-fit` 各模式示例**

假设我们有如下 HTML：

```html
html
复制编辑
<div class="image-container">
    <img src="example.jpg" alt="示例图片">
</div>

```

CSS：

```css
css
复制编辑
.image-container {
    width: 200px;
    height: 200px;
    border: 2px solid black;
    overflow: hidden;
}

.image-container img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

```

### **不同 `object-fit` 效果**

### ✅ `object-fit: fill;`（默认）

- **图片会拉伸填满容器，可能会变形。**
- **适用于矢量图或不在意变形的情况。**
- **可能导致图像比例失真。**

### ✅ `object-fit: contain;`

- **图片等比缩放，使整个图片可见。**
- **可能会有空白区域（因为图片会缩小以适应容器）。**
- **适用于不希望图片被裁剪的情况，比如展示完整的 Logo 或 UI 设计稿。**

### ✅ `object-fit: cover;`

- **图片等比缩放，完全填满容器，超出部分裁剪。**
- **适用于背景图片、头像等场景，确保图片不变形，同时填充整个区域。**

### ✅ `object-fit: none;`

- **图片不缩放，显示原始大小，超出部分隐藏（类似 `overflow: hidden;`）。**
- **适用于不希望缩放图片的情况。**

### ✅ `object-fit: scale-down;`

- **行为类似 `none` 或 `contain`，取两者中尺寸最小的一个。**
- **适用于自适应内容，需要优先保证完整性，但尽量不放大。**

---

## **3. `object-position`：控制对齐方式**

如果 `object-fit` 使图片被裁剪（如 `cover`），你可以使用 `object-position` 让它对齐到特定位置：

```css
css
复制编辑
img {
    object-fit: cover;
    object-position: top center; /* 让图片顶部对齐 */
}

```

- `object-position: left top;`（左上角）
- `object-position: center center;`（默认，居中）
- `object-position: right bottom;`（右下角）

---

## **4. `object-fit` 在你的代码中的作用**

你在 `.container header .profile img` 上使用了：

```css
css
复制编辑
.container header .profile img {
    position: relative;
    width: 100%;
    object-fit: cover;
    aspect-ratio: 1 / 1;
}

```

### **分析**

1. `width: 100%` 使 `img` 的宽度等于 `.profile` 的宽度（但 `.profile` 被 `flex-shrink` 压缩了）。
2. `aspect-ratio: 1 / 1` 让图片保持正方形。
3. `object-fit: cover` 让图片填充整个区域，超出的部分裁剪。

### **结果**

由于 `.profile` 可能被 `flex` 机制压缩，导致 `width` 变小，因此 `object-fit: cover` 会裁剪图片的一部分，**但不会让图片变形**。

**如果你不想裁剪图片，可以用 `object-fit: contain`**：

```css
css
复制编辑
.container header .profile img {
    object-fit: contain;
}

```

---

### **`aspect-ratio` 详解**

`aspect-ratio` 是 CSS 中用于**控制元素的宽高比（比例）**的属性，可以确保元素在不同屏幕或容器大小下保持特定的比例，不需要手动设置 `width` 和 `height`。

---

## **1. `aspect-ratio` 语法**

```css
css
复制编辑
element {
    aspect-ratio: 宽 / 高;
}

```

- 其中 `宽` 和 `高` 是一个比例，而不是具体的像素值。
- 该属性通常用于 `img`、`video`、`div` 等块级元素，但也适用于 `flex` 和 `grid` 布局中的子元素。

---

## **2. `aspect-ratio` 的常见用法**

### **✅ 1. 让图片保持 16:9 的比例**

```css
css
复制编辑
img {
    width: 100%;
    aspect-ratio: 16 / 9;
}

```

**效果**：

- 宽度自适应（100%）。
- 高度自动调整，保证 `16:9` 的比例，不会变形。

---

### **✅ 2. 让 `div` 保持 1:1 的正方形**

```css
css
复制编辑
.square {
    width: 150px;
    aspect-ratio: 1 / 1;
    background-color: lightblue;
}

```

**效果**：

- `width = 150px`
- `height` 会自动计算为 `150px`，形成正方形。

---

### **✅ 3. 在 `grid` 布局中使用**

```css
css
复制编辑
.grid-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
}

.grid-item {
    aspect-ratio: 4 / 3;
    background-color: lightcoral;
}

```

**效果**：

- `grid-item` 的宽度由 `grid` 自动分配。
- `height` 按 `4:3` 计算，保证图片比例一致。

---

## **3. `aspect-ratio` 在你的代码中的作用**

在你的 CSS 中，你给 `img` 添加了：

```css
css
复制编辑
.container header .profile img {
    width: 100%;
    object-fit: cover;
    aspect-ratio: 1 / 1;
}

```

### **分析**

1. `width: 100%` 让 `img` 的宽度等于 `.profile` 容器的宽度。
2. `aspect-ratio: 1 / 1` 强制 `img` 保持**正方形**。
3. `object-fit: cover` 确保图片填满容器，多余部分会被裁剪。

### **问题**

- `.profile` 可能在 `flex` 布局中被压缩，导致 `width` 变小。
- `img` 依然会保持 `1:1`，但由于 `flex-shrink`，它的尺寸可能整体变小。
- 这就导致**头像被压缩，而 `object-fit: cover` 让图片部分被裁剪**。

### **解决方案**

### ✅ **方案 1：固定 `.profile` 的宽度**

```css
css
复制编辑
.container header .profile {
    flex-shrink: 0;
    width: 80px; /* 设置固定宽度 */
}

```

### ✅ **方案 2：使用 `contain` 代替 `cover`**

```css
css
复制编辑
.container header .profile img {
    object-fit: contain;
}

```

- **`cover` 会裁剪超出部分**
- **`contain` 让图片完全显示，但可能会留空白**

---

## **4. `aspect-ratio` 和 `height` 结合使用**

如果 `width` 没有显式设置，可以配合 `height` 控制：

```css
css
复制编辑
.aspect-box {
    height: 200px;
    aspect-ratio: 16 / 9;
}

```

- `height: 200px`
- `width` 按 `16:9` 计算，自动调整。

---

## **5. 兼容性**

|浏览器|支持情况|
|---|---|
|Chrome|✅|
|Edge|✅|
|Firefox|✅|
|Safari|✅|
|IE11|❌ 不支持|

---

# 应用

## 链接

```css
a:link, a:visited {
  background-color: #f44336;
  color: white;
  padding: 14px 25px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
}

a:hover, a:active {
  background-color: red;
}
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/027937be-c1e8-4caf-842d-5f3d1648b5b8/Untitled.png)

## 列表

CSS中允许为列表设置不同标记、以图象为标记以及设置列表的背景色。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **标记样式**

</aside>

在HTML中，列表的标记类型使用`<ul>`或者`<ol>`标签的`type`属性定义，在CSS中有序列表和无序列表统一使用`list-style-type`定义标记类型。[列表与表格](https://www.notion.so/d4d2db4f14fc4c5a976fd4cc15e2cf8a?pvs=21)

在HTML5中列表的type属性已被废弃，不建议使用，推荐使用CSS的`list-style-type` 属性。

`list-style-type` 属性值在无序列表和有序列表中的属性值：

无序列表：

1. `disc`: 实心圆（默认值）
2. `circle`: 空心圆
3. `square`: 实心方块
4. `none`: 无标记

有序列表：

1. `decimal`: 数字（默认值）
2. `decimal-leading-zero`: 带前导零的数字（如01, 02, 03）
3. `lower-roman`: 小写罗马数字（如i, ii, iii）
4. `upper-roman`: 大写罗马数字（如I, II, III）
5. `lower-alpha`: 小写字母（如a, b, c）
6. `upper-alpha`: 大写字母（如A, B, C）
7. `lower-greek`: 小写希腊字母（如α, β, γ）
8. `lower-latin`, `upper-latin`: 小写/大写拉丁字母
9. 其他值：`armenian`, `georgian`, `hebrew`, `cjk-ideographic`, `katakana`, `hiragana`, 等等

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **以图像作为标记**

</aside>

使用图像作为列表标记的符号时，使用属性：`list-style-image` ：

```css
ul {
  list-style-image: url('sqpurple.gif');
}
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **标记的位置**

</aside>

列表标记的位置使用`list-style-position` 属性定义。属性值：

1. `inside`：将标记符号放在列表项内，作为文本的一部分·，第二行文本将会以标记符号对齐。
2. `outside`：将标记符号放在列表项外，不作为文本的一部分，第二行文本将会以文本对齐。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **速记属性**

</aside>

`list-style` 属性是一个简写属性。它用于在一个声明中设置所有列表属性：

```css
ul {
  list-style: square inside url("sqpurple.gif");
}
```

顺序为：type、position、image。

如果指定了图像作为标记符号，type则用于在图像无法正常显示时显示。如果缺少其中某值，将会使用默认值。

# 盒模型

> CSS盒模型是指在网页布局中，每个HTML元素都可以看作一个矩形的盒子，这个盒子包含了元素的内容、内边距、边框和外边距。CSS盒模型对于网页布局和样式设计具有重要意义，它定义了元素在页面上所占的空间和排列方式。

## 盒模型

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/8f888109-f2ec-40eb-8b73-8de90333f2ba/Untitled.png)

- **Margin(外边距)** - 除了边框外的区域，外边距是透明的，用于控制元素与其他元素之间的距离。
- **Border(边框)** - 围绕在内边距和内容外的线条，用于给元素添加边框效果。
- **Padding(内边距)** - 内容区域与边框之间的空间，内边距是透明的，用于控制内容与边框之间的间距。
- **Content(内容)** - 元素内部包含的实际内容，如文本、图像、子元素等。

## 宽度和高度

> `width`和`height`用于定义元素内容区域的宽高。

在CSS中使用`height`和`width` 属性设置元素的宽度和高度，实际上设置的是内容（content）区域的宽高，计算元素的总宽度和高度时，需要加上内边距和边框宽高。

需要注意的是，`height`和`width`属性只能对块级元素生效，对于内联元素宽度，是由其内容决定的，高度是由内容和行高决定的。如果要想一个内联元素能够设置宽高，可以将内联元素设置为内联块元素，将内联元素的`display`属性设置为`inline-block` 。

```css
div {
  width: 320px;
  height: 50px;
  padding: 10px;
  border: 5px solid gray;
  margin: 0;
}
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/d475f52e-78cf-4021-ab3d-b64bfebfbf7e/Untitled.png)

元素总宽度：宽度+左右内边距+左右边框。

元素总高度：高度+上下内边距+上下边框。

元素的外边距会影响盒子在页面上占用的空间，但外边距不包含在盒子的实际大小中。

元素的宽高可能具有以下值：

- `auto` ：浏览器自动计算宽高，默认值。
- `length` ：自定义宽高，单位为px、cm等。
- `%` ：相对于元素包含块的百分比，高度百分比在很多情况下不会产生预期效果，因为元素高度通常是由其内容的高度决定的，而不是包含块的高度。
- `initial` ：将高度、宽度设置为其默认值。
- `inherit` ：从父元素继承宽高。

`max-width` 属性用于设置元素的最大宽度（px、cm、%），默认情况下为`none`。当浏览器窗口小于元素宽度时，浏览器会向页面添加水平滚动条，而设置了最大宽度后，当浏览器窗口小于这个值时，元素的文本内容就会自动换行。

## 边框

### 样式宽度和颜色

> CSS边框属性定义元素边框的样式、宽度和颜色。

<aside> 💡 **`border-style`属性指定边框的样式。**

</aside>

具有以下预设值：

- `dotted` ：定义点虚线边框。
- `dashed` ：定义线虚线边框。
- `solid` ：定义实心边框。
- `double` ：定义双边框。
- `groove` ：定义3D凹槽边框。效果取决于边框颜色值。
- `ridge` ：定义3D脊状边框。效果取决于边框颜色值。
- `inset` ：定义3D插入边框。效果取决于边框颜色值。
- `outset` ：定义3D起始边框。效果取决于边框颜色值。
- `none` ：定义无边框。
- `hidden` ：定义隐藏边框。

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/5702af6b-c46e-4e75-8e50-1c18d3ecb715/Untitled.png)

`border-style` 属性是边框属性的基础属性，如果没有这个属性，其它边框属性将不会产生效果。

---

<aside> 💡 **`border-width`** **属性指定边框的宽度。**

</aside>

可以使用自定义值（以px、pt、cm、em为单位），也可以使用预设的值：

- `thin`
- `medium`
- `thick`

---

<aside> 💡 **`border-color` 属性用于设置边框的颜色**

</aside>

[CSS颜色](https://www.notion.so/CSS3-fb952830f9394aacb4738ac204e9ba88?pvs=21)

---

<aside> 💡 **简写**

</aside>

使用该属性简化代码，三个值，依次代表宽度、样式（必填）和颜色。

```css
p {
  border: 5px solid red;
}
p2 {
  border-left: 6px solid red;
}
```

边框的样式、宽度和颜色属性可以具有一到四个值，从上边框开始顺时针指定，对应着上、右、下和左边框。

```css
/* 四个值：上、右、下、左 */
p {
  border-style: dotted solid double dashed;
}
/* 三个值：上、左右、下 */
p {
  border-width: 1px 2px 3px;
}
/* 两个值：上下、左右 */
p {
  border-color: red bule;
}
/* 一个值：上下左右 */
p {
  border-style: dotted;
}
```

边框的样式、宽度和颜色属性也可以对四个边框分别指定。

```css
p {
  border-top-style: dotted;
  border-right-style: solid;
  border-bottom-style: dotted;
  border-left-style: solid;
}
```

### 圆角

> 元素边框的圆角属性定义边框的圆角程度。

<aside> 💡 **`border-radius` 属性用于向元素添加圆角边框。**

</aside>

`border-radius` 属性可以接受一到四个值，分别控制元素四个角的圆角半径（从左上角开始按顺时针）。

```css
/* 单个值 - 所有角的圆角半径相同 */
border-radius: 10px;
/* 两个值 - 第一个值用于左上角和右下角，第二个值用于右上角和左下角 */
border-radius: 10px 20px;
/* 三个值 - 第一个值用于左上角，第二个值用于右上角和左下角，第三个值用于右下角 */
border-radius: 10px 20px 30px;
/* 四个值 - 分别对应左上角、右上角、右下角和左下角 */
border-radius: 10px 20px 30px 40px;
```

`border-radius` 属性实际上是 `border-top-left-radius` 、 `border-top-right-radius` 、 `border-bottom-right-radius` 和 `border-bottom-left-radius` 属性的简写属性，所以可以单独指定每个角的圆角半径。

```css
border-top-left-radius: 10px;   /* 左上角 */
border-top-right-radius: 20px;  /* 右上角 */
border-bottom-right-radius: 30px;/* 右下角 */
border-bottom-left-radius: 40px; /* 左下角 */
```

除了可以使用像素值外，还可以使用**百分比**和**水平半径和垂直半径之比**来表示。

```css
border-radius: 50px / 15px
border-radius: 50%
border-radius: 10px 20px 30px 40px / 50px 60px 70px 80px;
border-radius: 50% / 25%;
```

如果圆角属性的值不使用百分比和水平半径和垂直半径之比来表示，则表示，某个角的水平半径和垂直半径一样，如果是用50%这种百分比形式表示，则表示水平半径和垂直半径都是元素总宽高的一半，这时如果元素总宽高相同，则会成为圆形；如果是50% / 25%这种形式，则表示水平半径为总宽度的一半，垂直半径为总高度的30%；如果是50px / 15px这种形式，则表示水平半径为50px，垂直半径为15px，如果设置了绝对值的话，对于响应式设计不友好，建议使用百分比。

**元素圆角的半径是相对于总宽高（内容+内边距+边框）来说的。**

## 外边距

> 外边距用于在元素周围、边框之外创建空间。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **基本语法**

</aside>

单独设置四个外边距：

```css
margin-top: 10px;
margin-right: 15px;
margin-bottom: 20px;
margin-left: 25px;
```

使用简写属性：

```css
/* 四边的外边距相同 */
margin: 10px;
/* 上下外边距为 10px，左右外边距为 20px */
margin: 10px 20px;
/* 上外边距为 10px，左右外边距为 20px，下外边距为 30px */
margin: 10px 20px 30px;
/* 上、右、下、左外边距分别为 10px, 20px, 30px, 40px */
margin: 10px 20px 30px 40px;
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **取值**

</aside>

1. 长度值：可以是px、pt、em、rem等。
2. 百分比：相对于包含块的宽度计算。
3. **`auto`**：浏览器自动计算外边距，常用于元素**水平居中**。
4. 负值：允许使用负的外边距，将元素拉近彼此。负的外边距不会将元素挤开，而是会让元素朝该方向靠拢，如果负的外边距的绝对值大于相邻元素的外边距，两个元素可能会重叠。
5. **`inherit`**：**`margin`**属性默认不会继承，但可以显式的设置该值，从父元素继承外边距设置。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **垂直外边距折叠（Margin Collapse）**

</aside>

当两个块级元素的垂直外边距相遇时，可能会发生外边距折叠。折叠后的外边距等于两个外边距中的较大值。

```html
<div class="box1"></div>
<div class="box2"></div>
```

```css
.box1 {
    margin-bottom: 20px;
}
.box2 {
    margin-top: 30px;
}
```

在上述情况下，两个元素之间的外边距为30px，而不是50px。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **水平居中**

</aside>

元素水平居中最常用的方式是将左右外边距设置为”`auto`”，并指定元素宽度。

```css
.container {
    width: 50%;
    margin: 0 auto;  /* 上下外边距为 0，左右外边距自动 */
}
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **最佳实践**

</aside>

1. 外边距折叠问题
    
    外边距折叠问题常见于相邻的块级元素或者父子元素之间，可以通过以下方式解决：
    
    - 使用边框或内边距：例如，给父元素添加 **`padding`** 或 **`border`**。
    - 设置 **`overflow`** 属性为 **`hidden`** 或 **`auto`**。
    - 设置父元素的 **`display`** 属性为 **`flex`** 或 **`grid`**。
2. 负外边距的使用
    
    负外边距在需要元素相互重叠时特别有用，但要小心使用，避免布局混乱。过度使用负外边距会使CSS维护困难，还可能会影响文档流中其它元素。
    
3. 外边框用于对元素的布局，所以它只能设置宽度，而不能设置颜色和样式。
    

## 内边距

> 内边距用于在元素内容周围，边框之内创建空间。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **基本语法**

</aside>

单独设置四个内边距：

```css
padding-top: 10px;
padding-right: 15px;
padding-bottom: 20px;
padding-left: 25px;
```

使用简写属性：

```css
/* 四边的内边距相同 */
padding: 10px;
/* 上下内边距为 10px，左右内边距为 20px */
padding: 10px 20px;
/* 上内边距为 10px，左右内边距为 20px，下内边距为 30px */
padding: 10px 20px 30px;
/* 上、右、下、左内边距分别为 10px, 20px, 30px, 40px */
padding: 10px 20px 30px 40px;
```

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **取值**

</aside>

1. 长度值：可以是px、pt、em、rem等。
2. 百分比：相对于包含块宽度计算。
3. `inherit`：**`padding`**属性默认不会继承，但可以显式的设置该值，从父元素继承内边距设置。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **内边距对盒模型的影响**

</aside>

1. 标准盒模型（content-box）
    
    如果元素具有指定宽度（内容区域），设置的内边距将会添加到元素的总宽度中。
    
    ```css
    .box {
        width: 100px;
        height: 100px;
        padding: 20px;
        box-sizing: content-box;  /* 默认值 */
    }
    ```
    
2. 边框盒模型（border-box）
    
    如果元素具有指定宽度（内容区域），设置的内边距将会从内容区域减去，而总宽度不变。
    
    ```css
    .box {
        width: 100px;
        height: 100px;
        padding: 20px;
        box-sizing: border-box;
    }
    ```
    

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **最佳实践**

</aside>

1. 内边框用于布局和对内容位置的调整，所以它只能设置宽度，不能设置颜色和样式。

## 轮廓

> 轮廓（outline）用于绘制围绕元素边框的线条。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **基本语法**

</aside>

单独设置轮廓的各个属性：

```css
.element {
    outline-width: 2px;
    outline-style: solid;
    outline-color: red;
}
```

使用简写属性：

```css
.element {
    outline: 2px solid red;
}
```

如果不设置**`outline-style`**属性，则轮廓的其它属性将不生效！

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **取值**

</aside>

- **`outline-width`**
    - 绝对长度值（px、pt、cm、em）。
    - **`thin`** （通常为1px）。
    - **`medium`** （通常为3px）。
    - **`thick`** （通常为5px）。
- **`outline-style`**
    - **`none`**: 无轮廓线。
    - **`solid`**: 实线。
    - **`dotted`**: 点线。
    - **`dashed`**: 虚线。
    - **`double`**: 双线。
    - **`groove`**: 沟槽。
    - **`ridge`**: 脊线。
    - **`inset`**: 内嵌。
    - **`outset`**: 外嵌。
- **`outline-color`**
    - 使用预定义颜色名称。
    - 使用HEX、RGB、RGBA、HSL、HSLA等颜色值。

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **轮廓偏移**

</aside>

**`outline-offset`**属性用于定义轮廓偏移量，也就是轮廓和边框的距离。默认情况下，轮廓是从边框开始，随着轮廓宽度的增加而向外延伸的。

```css
p {
  border: 1px solid black;
  outline: red solid 10px;
  margin: 10px;  
  padding: 20px;
  text-align: center;
}
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/35d1fec4-313c-4d8e-8e1c-4416f01a0a69/Untitled.png)

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **轮廓与边框区别**

</aside>

1. 不占据空间
    
    轮廓不影响元素的实际尺寸和布局，不占空间；
    
    边框会影响元素实际尺寸，计算进总宽高中。
    
2. 无法应用单个边
    
    轮廓不能设置单个边。
    
    边框、外边距、内边距都可以设置单个边。
    
3. 会和外边框重叠
    
    轮廓会和外边距重叠。
    
    边框不会和外边距重叠。
    

<aside> <img src="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" alt="[https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/重点标注.svg](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/697211af-9faf-4ad7-b5b7-29080fa40bc8/%E9%87%8D%E7%82%B9%E6%A0%87%E6%B3%A8.svg)" width="40px" /> **最佳实践**

</aside>

1. 轮廓通常用于和用户交互产生视觉反馈，所以需要围绕整个元素。

## overflow

`overflow` 属性指定当元素的内容太大而无法容纳在指定区域时是剪辑内容还是添加滚动条。

- `visible` - 默认。溢出不会被剪裁。内容在元素的框外呈现
- `hidden` - 溢出部分被裁剪，其余内容将不可见
- `scroll` - 剪切溢出，并添加滚动条以查看其余内容
- `auto` - 类似于 `scroll`，但它仅在必要时添加滚动条

# 布局（Layout）

## 显示

`display`属性定义元素在网页上的显示方式。

属性值有：

1. `block` ：使元素作为块级元素显示，占据父容器的全部宽度，并在其前后创建换行符
2. `inline` ：使元素作为内联元素显示，不会在其前后创建换行符。元素的宽度和高度取决于内容。
3. `inline-block` ：使元素保持内联显示，但同时拥有块级元素的特性（可以设置宽度和高度）。
4. `none` ：使元素完全不显示，并且不占据任何空间。
5. `flex` ：使元素成为一个弹性容器（flex container），其子元素成为弹性项目（flex items），允许更灵活的布局。
6. `grid`：使元素成为一个网格容器（grid container），其子元素成为网格项目（grid items），允许复杂的网格布局。
7. `inline-flex`：使元素成为一个内联弹性容器（inline flex container）。
8. `inline-grid`：使元素成为一个内联网格容器（inline grid container）。
9. `table`, `table-row`, `table-cell` ：使元素及其子元素的显示和布局类似于 HTML 表格。
10. `list-item`：使元素作为列表项显示，通常用于 `li` 元素。

设置元素的显示属性只会更改元素的显示方式，而不会更改元素的类型。比如，带有 `display: block;` 的内联元素不允许在其中包含其他块元素。

通过合理使用 `display` 属性，可以创建灵活和响应性强的布局，提高页面的用户体验。

---

与`display`属性类似的是`visibility` 属性，用于控制元素的可见性。

属性值有：

1. **`visible`**: 默认值，元素是可见的。
2. **`hidden`**: 元素不可见，但**仍占据空间**。
3. **`collapse`**: 适用于表格和表格行元素，隐藏元素并且不占据空间，类似于 `display: none`。
4. **`inherit`**: 继承父元素的 `visibility` 属性值。

## 定位（Position）

`position` 属性用于控制元素的定位方式。

属性值有：

1. **`static`**: 默认值，元素按正常的文档流进行布局，不受任何顶层、左、右或底部属性的影响。
    
2. **`relative`**: 相对定位，元素相对于其正常位置进行定位，可以使用顶、左、右、底部属性进行调整。
    
3. **`absolute`**: 绝对定位，元素相对于最近的定位祖先（即非 `static` 的祖先元素）进行定位。如果没有这样的祖先元素，则相对于初始包含块（通常是文档的 `<html>` 元素）进行定位。绝对定位的元素将从正常流中删除，并且可以重叠元素。
    
4. **`fixed`**: 固定定位，元素相对于浏览器窗口进行定位，即使页面滚动，元素仍然保持在相同位置。
    
5. **`sticky`**: 粘性定位，元素在跨越特定阈值之前表现为相对定位，一旦跨越阈值后表现为固定定位。必须指定左、右、顶部和底部属性之一才能起作用（阈值）。
    
    ```css
    div.sticky {
      position: -webkit-sticky; /* Safari */
      position: sticky;
      top: 0;
      background-color: green;
      border: 2px solid #4CAF50;
    }
    ```
    
    粘性元素会粘在页面顶部 ( `top: 0` )
    

使用场景：

- **`static`**: 适用于不需要特殊定位的元素，这是大多数元素的默认值。
- **`relative`**: 适用于需要在其正常位置上进行微调的元素。
- **`absolute`**: 适用于需要从文档流中移除并精确放置在某个位置的元素。[https://www.w3schools.com/css/tryit.asp?filename=trycss_position_absolute](https://www.w3schools.com/css/tryit.asp?filename=trycss_position_absolute)
- **`fixed`**: 适用于需要始终固定在浏览器窗口中的元素，如导航栏、悬浮按钮等。
- **`sticky`**: 适用于需要在特定滚动位置固定的元素，如固定在顶部的标题栏。[https://www.w3schools.com/css/tryit.asp?filename=trycss_position_sticky](https://www.w3schools.com/css/tryit.asp?filename=trycss_position_sticky)

---

`top`、`right`、`bottom` 和 `left` 属性用于定位元素，这些属性只有在元素的 `position` 属性设置为 `relative`、`absolute`、`fixed` 或 `sticky` 时才会生效。它们指定元素在定位容器中的偏移量。

1. **`top`**: 定义元素的顶部边缘与其包含块顶部边缘之间的偏移量。
2. **`right`**: 定义元素的右边缘与其包含块右边缘之间的偏移量。
3. **`bottom`**: 定义元素的底部边缘与其包含块底部边缘之间的偏移量。
4. **`left`**: 定义元素的左边缘与其包含块左边缘之间的偏移量。

---

`clip-path` 属性替代`clip`属性用于在 CSS 中用于创建裁剪区域，只显示元素的一部分。定义一个路径或形状，以确定元素的哪些部分应该可见。未在裁剪区域内的部分将被隐藏。可以为使用绝对定位的元素提供裁剪。

属性值：

1. **基本形状**：可以使用 `inset()`、`circle()`、`ellipse()`、`polygon()` 等函数定义基本形状。
    
    - `inset()` ：用于创建矩形裁剪区域。
        
        ```css
        .clip-inset {
          clip-path: inset(20px 30px 40px 50px); /* top right bottom left */
        }
        ```
        
    - **`circle()`**: 用于创建圆形裁剪区域。
        
        ```css
        .clip-circle {
          clip-path: circle(50px at center);
        }
        ```
        
    - **`ellipse()`**: 用于创建椭圆形裁剪区域。
        
        ```css
        .clip-ellipse {
          clip-path: ellipse(50% 30% at 50% 50%);
        }
        ```
        
    - **`polygon()`**: 用于创建多边形裁剪区域。
        
        ```css
        .clip-polygon {
          clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
        }
        ```
        
2. **SVG 路径**：使用 `path()` 函数定义复杂路径。
    
3. **`url()`**：引用 SVG 文件中的 `<clipPath>` 定义。
    
4. **`none`**：默认值，不进行任何裁剪。
    

## 弹性布局（Flexbox）

- 弹性布局是一种常用的布局方式，主要用于一维布局（行或列方向上的排序）
- 在 Flexbox 中，布局是由父容器和子元素组成：
    - Flex 容器：通过 `display: flex;` 或 `display: inline-flex;` 设定的父元素
    - Flex 项目：Flex容器内的所有直接子元素

### Flex 容器的属性

1. **`display`**
    
    用于定义 Flex 容器：
    
    - `display: flex;`（块级元素，默认 `width: 100%`）
    - `display: inline-flex;`（行内元素，不独占一行）
2. **`flex-direction`**
    
    设置**主轴方向**，影响`justify-content`等属性：
    
    - `row`（默认）：从左到右排列（横向）
    - `row-reverse`：从右到左排列（横向反转）
    - `column`：从上到下排列（纵向）
    - `column-reverse`：从下到上排列（纵向反转）
3. **`flex-wrap`**
    
    默认情况下，Flex项目会**挤在同一行**，`flex-wrap`用于控制是否换行：
    
    - `nowrap`（默认）：不换行
    - `wrap`：换行（超出容器的内容换到下一行）
    - `wrap-reverse`：换行并反向排列
4. **`justify-content`**
    
    控制**主轴方向上的对齐方式**：
    
    - `flex-start`（默认）：左对齐
    - `flex-end`：右对齐
    - `center`：居中对齐
    - `space-between`：两端对齐（首尾贴边）
    - `space-around`：均匀分布（左右留空）
    - `space-evenly`：间距相等
5. **`align-items`**
    
    控制**交叉轴方向上的对齐方式**：
    
    - `stretch`（默认）：拉伸填充
    - `flex-start`：顶部对齐
    - `flex-end`：底部对齐
    - `center`：居中对齐
    - `baseline`：按照文本基线对齐
6. **`align-content`**
    
    当`flex-wrap: wrap;`时，**多行内容的对齐方式**：
    
    - `stretch`（默认）：拉伸填充
    - `flex-start`：靠上对齐
    - `flex-end`：靠下对齐
    - `center`：居中对齐
    - `space-between`：两端对齐
    - `space-around`：间距相等
7. **`gap`**
    
    设定**行内元素之间的间距**（不影响外边距 `margin`），是两个属性的简写属性：
    
    - `row-gap`：主轴方向间距
    - `column-gap`：交叉轴方向间距

### Flex 项目的属性

1. **`flex-grow`**
    
    定义**项目的扩展比例**，默认为 `0`（不扩展）。
    
    - `0`：不扩展
    - `1`：按比例扩展，剩余空间均分
    - 其他值：按照指定比例分配空间
2. **`flex-shrink`**
    
    定义**项目的缩小比例**，默认为 `1`（允许缩小）。
    
    - `0`：不缩小
    - 其他值：按照指定比例缩小
3. **`flex-basis`**
    
    定义**项目的初始大小**（主轴上的大小），可取：
    
    - `auto`（默认）：按内容或 `width/height` 设定
    - 具体值（`100px`、`50%`）：指定初始大小
4. **`flex`**
    
    简写形式：`flex: flex-grow flex-shrink flex-basis;`**常见写法：**
    
    - `flex: 1;`（等比例分配空间）
    - `flex: 1 0 auto;`（不缩小，不超出）
    - `flex: 0 0 100px;`（固定宽度100px）
5. **`align-self`**
    
    设置**单个Flex项目**在交叉轴方向上的对齐方式：
    
    - `auto`（默认）：继承 `align-items`
    - 其他值：`flex-start`、`flex-end`、`center`、`baseline`、`stretch`

## 网格布局（Grid）

- 网格布局是一种二维布局方式，与 Flexbox 相比，Grid 具有更高的控制力
- 网格布局的思路是将容器划分为一个网格，然后通过定义网格中的行和列来排列元素

### Grid 容器属性

1. **`display`**
    
    用于定义 Grid 容器：
    
    - `display: grid;`：生成一个块级元素的 Grid 容器。
    - `display: inline-grid;`：生成一个行内元素的 Grid 容器。
2. **`grid-template-columns`** 和 **`grid-template-rows`**
    
    这两个属性定义了 **网格容器的列和行** 的尺寸：
    
    - `grid-template-columns`：设置列的宽度。
    - `grid-template-rows`：设置行的高度。
3. **`grid-template-areas`**
    
    通过名称来为网格定义区域。可以给网格单元格命名，使得项目占据指定的区域。
    
4. **`grid-gap`** 或 **`gap`**
    
    用于定义网格项目之间的间距，`gap` 是 `row-gap` 和 `column-gap` 的简写：
    
    - `row-gap`：行间距
    - `column-gap`：列间距
    - `gap`：行列间距都设置
5. **`grid-auto-rows`** 和 **`grid-auto-columns`**
    
    定义当项目放置在网格外部（没有明确的位置）时，自动生成的行和列的大小
    

### Grid 项目属性

1. **`grid-column`** 和 **`grid-row`**
    
    - `grid-column`：控制 Grid 项目在列方向上的占据。
    - `grid-row`：控制 Grid 项目在行方向上的占据。
2. **`grid-column-start`** 和 **`grid-column-end`** / **`grid-row-start`** 和 **`grid-row-end`**
    
    分别设置 Grid 项目的开始和结束位置：
    
    - `grid-column-start` 和 `grid-column-end`：定义项目开始和结束的列。
    - `grid-row-start` 和 `grid-row-end`：定义项目开始和结束的行。
3. `grid-area`
    
    这是 `grid-column` 和 `grid-row` 的简写形式，可以指定项目所占据的区域。
    
4. **`justify-self`** 和 **`align-self`**
    
    这些属性控制单个 Grid 项目在**单元格内的对齐方式**：
    
    - `justify-self`：控制项目在水平方向上的对齐（与容器的行对齐方式类似）。
    - `align-self`：控制项目在垂直方向上的对齐（与容器的列对齐方式类似）。

## 多列布局

### **📌 `columns` 属性（CSS 多列布局）**

`columns` 属性用于创建 **多列布局（multi-column layout）**，可以让文本或块级元素**自动分成多个列**，类似于报纸或杂志的布局。

---

## **1️⃣ 语法**

```css
css
复制编辑
columns: column-width column-count;

```

**参数解释：**

- `column-width`（列宽）：指定每列的最小宽度，浏览器会自动调整列数以适应容器宽度。
- `column-count`（列数）：指定总列数，浏览器自动调整列宽以适应容器宽度。

---

## **2️⃣ 基本用法**

### **（1）指定列数 `column-count`**

```css
css
复制编辑
.container {
  column-count: 3; /* 让内容自动分为 3 列 */
}

```

```html
html
复制编辑
<div class="container">
  <p>这是一些示例文本。它会自动分为三列。</p>
</div>

```

✅ **效果**：文本会被自动拆分成 **3 列**，每列的宽度由浏览器自动计算。

---

### **（2）指定列宽 `column-width`**

```css
css
复制编辑
.container {
  column-width: 200px; /* 每列至少 200px 宽，浏览器自动计算列数 */
}

```

✅ **效果**：浏览器会尽可能把每列的宽度保持在 **200px**，并自动调整列数以适应父容器。

---

### **（3）同时指定列宽和列数**

```css
css
复制编辑
.container {
  columns: 200px 3; /* 尝试使用 3 列，每列至少 200px 宽 */
}

```

✅ **效果**：优先满足 `column-width`（200px），但最多分成 3 列，超出则调整列数。

---

## **3️⃣ 进阶属性**

### **（1）`column-gap`（列间距）**

```css
css
复制编辑
.container {
  column-count: 3;
  column-gap: 30px; /* 设置列与列之间的间距 */
}

```

✅ **效果**：每列之间的间距是 **30px**。

---

### **（2）`column-rule`（列间分割线）**

```css
css
复制编辑
.container {
  column-count: 3;
  column-rule: 2px solid black; /* 列之间添加 2px 的黑色分割线 */
}

```

✅ **效果**：列与列之间有 **2px 的黑色垂直分割线**。

---

### **（3）`column-span`（跨列）**

**用于让某个元素跨多列**，通常用于标题：

```css
css
复制编辑
h2 {
  column-span: all; /* 让 h2 跨所有列 */
}

```

✅ **效果**：`h2` **不会被拆分**，它会**跨越所有列**。

---

## **4️⃣ `columns` 与 `flex/grid` 的区别**

|**对比项**|**columns**|**flex/grid**|
|---|---|---|
|**适用于**|文本内容|盒子布局|
|**内容流动**|自动换列|手动调整|
|**支持列间距**|✅ `column-gap`|✅ `gap`|
|**子元素布局**|**无法控制**单独子元素布局|**完全控制**|

> 📌 columns 适用于 多栏文本，如果是复杂布局，请使用 grid 或 flexbox。

---

## **5️⃣ 兼容性**

✅ **现代浏览器（Chrome、Firefox、Edge、Safari）** 都支持 `columns`。

对于 **IE11 及更早版本**，可以加 `-webkit-` 或 `-moz-` 前缀：

```css
css
复制编辑
.container {
  -webkit-columns: 3;
  -moz-columns: 3;
  columns: 3;
}

```

---

## **6️⃣ 总结**

✅ `columns` 适用于 **文本多栏排版**，关键属性：

- `column-count` → **指定列数**
- `column-width` → **指定最小列宽**
- `column-gap` → **列间距**
- `column-rule` → **列间分割线**
- `column-span` → **跨列**

💡 **适用场景**：

- 文章排版（新闻、杂志风格）
- 目录、列表
- 响应式文本布局

# 响应式设计

# 过渡和动画

### `transition` 在 CSS 中的作用

`transition` 是一个 CSS 属性，它使得元素在其状态发生变化时，能够平滑地过渡到新的状态。通过 `transition`，我们可以为某些 CSS 属性的变化添加动画效果，例如颜色变化、位置变化、透明度变化等。`transition` 可以让网页中的交互更加流畅和自然。

### `transition` 属性的语法

```css
css
复制编辑
transition: [property] [duration] [timing-function] [delay];

```

### 1. **`property`**（过渡的属性）

- **指定要应用过渡效果的 CSS 属性**。
- 你可以设置一个具体的属性（如 `background-color`, `width`, `height` 等），也可以使用 `all` 来表示所有支持过渡的属性都进行过渡。

**常见的可过渡属性**：

- `background-color`、`color`、`height`、`width`、`opacity`、`transform` 等。

**示例**：

```css
css
复制编辑
transition: background-color 0.5s;

```

这表示只有 `background-color` 这个属性会应用过渡效果，过渡时间为 0.5 秒。

### 2. **`duration`**（过渡持续时间）

- **定义过渡效果从开始到结束所需的时间**，通常使用秒（s）或者毫秒（ms）作为单位。
- 例如，`1s` 表示过渡效果将持续 1 秒。

**示例**：

```css
css
复制编辑
transition: color 1s;

```

这表示颜色的变化将持续 1 秒。

### 3. **`timing-function`**（过渡的速度曲线）

- **定义过渡效果的速度变化方式**。它控制过渡效果的加速或减速模式。
- 常见的 `timing-function` 值有：
    - `ease`：默认值，缓慢开始，然后加速，最后减速。
    - `linear`：匀速，整个过程的速度一致。
    - `ease-in`：开始时慢，然后加速。
    - `ease-out`：开始时快，最后减速。
    - `ease-in-out`：开始和结束时慢，中间加速。
    - `cubic-bezier(n,n,n,n)`：自定义贝塞尔曲线，可以创建更加细致的加速与减速效果。

**示例**：

```css
css
复制编辑
transition: all 0.5s ease-in-out;

```

这表示所有的过渡属性都使用 0.5 秒的过渡时间，并且使用 `ease-in-out` 速度曲线。

### 4. **`delay`**（过渡的延迟时间）

- **指定过渡效果开始前的延迟时间**。同样使用秒（s）或毫秒（ms）为单位。
- 如果设置了延迟，过渡效果将不会立即开始，而是等待指定的时间后才开始。

**示例**：

```css
css
复制编辑
transition: opacity 1s 0.5s;

```

这表示元素的透明度变化将在 0.5 秒的延迟后开始，并且持续 1 秒。

### `transform` 属性在 CSS 中的作用

`transform` 属性允许你对元素进行 **2D 或 3D 转换**，包括旋转、缩放、平移、倾斜等操作。它是一个非常强大的属性，可以用于实现各种动画效果、布局调整以及复杂的视觉效果，而不会影响到元素的文档流（即不会占据额外的空间）。`transform` 不会改变元素的位置或布局，只会改变元素的外观。

### `transform` 的常见用法

使用相对单位，如%，是相对于元素本身进行平移的

### 1. **`translate()`** — 平移

- **作用**：移动元素，改变其位置。
    
- **语法**：`translate(x, y)`
    
    - `x`：水平平移，正值向右，负值向左。
    - `y`：垂直平移，正值向下，负值向上。
    
    **示例**：
    
    ```css
    css
    复制编辑
    .box {
      transform: translate(50px, 100px);
    }
    
    ```
    
    这会将 `.box` 元素向右平移 50px，向下平移 100px。
    

### 2. **`scale()`** — 缩放

- **作用**：改变元素的大小。
    
- **语法**：`scale(x, y)`
    
    - `x`：水平缩放，默认值是 `1`（不缩放），大于 `1` 表示放大，小于 `1` 表示缩小。
    - `y`：垂直缩放，默认为 `1`。如果只传一个值，水平和垂直方向都会按该值缩放。
    
    **示例**：
    
    ```css
    css
    复制编辑
    .box {
      transform: scale(1.5, 2); /* 水平方向放大1.5倍，垂直方向放大2倍 */
    }
    
    ```
    

### 3. **`rotate()`** — 旋转

- **作用**：旋转元素。
    
- **语法**：`rotate(angle)`
    
    - `angle`：旋转角度，可以使用 `deg`（度）作为单位。
    
    **示例**：
    
    ```css
    css
    复制编辑
    .box {
      transform: rotate(45deg); /* 将元素顺时针旋转45度 */
    }
    
    ```
    

### 4. **`skew()`** — 倾斜

- **作用**：使元素倾斜，即在水平方向或垂直方向上对元素进行倾斜。
    
- **语法**：`skew(x-angle, y-angle)`
    
    - `x-angle`：水平方向上的倾斜角度。
    - `y-angle`：垂直方向上的倾斜角度。
    
    **示例**：
    
    ```css
    css
    复制编辑
    .box {
      transform: skew(20deg, 10deg); /* 水平方向倾斜20度，垂直方向倾斜10度 */
    }
    
    ```
    

### 5. **`matrix()`** — 矩阵变换

- **作用**：基于 2D 矩阵进行变换，允许你同时进行旋转、缩放、平移和倾斜操作。它使用 6 个值来描述这些变换。
    
- **语法**：`matrix(a, b, c, d, e, f)`
    
    - `a, b, c, d` 控制缩放、旋转、倾斜。
    - `e, f` 控制平移。
    
    **示例**：
    
    ```css
    css
    复制编辑
    .box {
      transform: matrix(1, 0.5, -0.5, 1, 100, 100);
    }
    
    ```
    
    这种变换表示一个结合了平移、旋转、倾斜和缩放的效果，实际应用中较为复杂，一般较少直接使用。
    
    ### `opacity` 属性在 CSS 中的作用
    
    `opacity` 是一个 CSS 属性，用于设置元素的透明度。它决定了元素的不透明程度，其值范围从 `0` 到 `1`，其中：
    
    - `0` 表示完全透明，元素不可见。
    - `1` 表示完全不透明，元素完全可见。
    - 介于 `0` 和 `1` 之间的值表示半透明，元素会根据设置的透明度显示。
    
    ### `opacity` 的语法
    
    ```css
    css
    复制编辑
    opacity: value;
    
    ```
    
    - `value` 的取值范围是 `0` 到 `1`。
        - `0`：完全透明，元素不可见。
        - `1`：完全不透明，元素完全可见。
        - 例如，`0.5` 表示半透明。
    
    ### 示例
    
    ### 1. **完全透明**
    
    ```css
    css
    复制编辑
    .transparent {
      opacity: 0;
    }
    
    ```
    
    元素会变得完全透明，不可见。
    
    ### 2. **半透明**
    
    ```css
    css
    复制编辑
    .semi-transparent {
      opacity: 0.5;
    }
    
    ```
    
    元素会变成半透明，即它会显示 50% 透明度。
    
    ### 3. **完全不透明**
    
    ```css
    css
    复制编辑
    .opaque {
      opacity: 1;
    }
    
    ```
    
    元素会保持完全不透明。
    
    ### `opacity` 与 `transition` 配合使用
    
    `opacity` 常常与 `transition` 结合使用，用于创建平滑的过渡效果，比如鼠标悬停时元素逐渐变得透明或出现。通过过渡动画，可以让元素的透明度变化更加平滑，提升用户体验。
    
    ### 示例：
    
    ```css
    css
    复制编辑
    .box {
      opacity: 1;
      transition: opacity 0.5s ease;
    }
    
    .box:hover {
      opacity: 0.3;
    }
    
    ```
    
    **解释**：
    
    - 当鼠标悬停到 `.box` 元素上时，元素的透明度会在 0.5 秒内从完全不透明 (`1`) 渐变为半透明 (`0.3`)，实现平滑过渡效果。
    
    ### `opacity` 对后代元素的影响
    
    `opacity` 不仅会影响元素本身的透明度，还会影响该元素的所有后代元素的透明度。这意味着，当你设置一个父元素的 `opacity` 时，所有子元素都会继承该透明度效果。
    
    ### 示例：
    
    ```css
    css
    复制编辑
    .parent {
      opacity: 0.5;
    }
    
    .child {
      background-color: red;
    }
    
    ```
    
    **解释**：
    
    - `.parent` 元素及其 `.child` 元素都会变得半透明，因为 `opacity` 属性影响了所有后代元素。
    
    ### 与 `background-color` 结合使用
    
    有时，我们希望仅改变元素背景的透明度，而不影响整个元素的透明度。在这种情况下，`opacity` 可能不够合适，因为它会影响到元素的所有内容（包括文字、边框等）。如果只需要背景透明度，可以使用 `rgba` 或 `hsla` 颜色值来控制背景颜色的透明度。
    
    ### 示例：
    
    ```css
    css
    复制编辑
    .box {
      background-color: rgba(255, 0, 0, 0.5);  /* 背景色为红色，透明度为50% */
    }
    
    ```
    
    **解释**：
    
    - 这里，背景颜色采用 `rgba`（红色、绿色、蓝色、透明度）来设置透明度，而不使用 `opacity`，这可以确保文本和其他内容保持不透明。
    
    ### 使用 `opacity` 的常见场景
    
    1. **悬停效果**：
        - 在网页设计中，常见的使用场景之一是当鼠标悬停时，元素的透明度发生变化，例如按钮在悬停时逐渐变暗或变亮。
    2. **渐变效果**：
        - 透明度变化通常与过渡效果（`transition`）一起使用，用来创建元素的渐显或渐隐效果。
    3. **图像处理**：
        - 在某些情况下，设置图片的透明度可以使图片看起来更柔和，或与背景色融合。
    4. **模态框和弹出窗口**：
        - 在弹出窗口或模态框的背景上，设置半透明背景可以有效增强焦点效果，同时避免其他内容被干扰。
    
    ### `opacity` 的性能和注意事项
    
    - **性能问题**：
        - 使用 `opacity` 的元素会产生浏览器的重绘（repaint），但不会导致重排（reflow），因此性能比一些其他视觉效果（如 `width`、`height`）的变化要好。
        - 但是，如果透明度变化频繁，或者透明度应用在多个嵌套元素上，可能会影响渲染性能，尤其是在低性能设备上。
    - **透明度继承**：
        - 正如前面提到的，`opacity` 会影响所有后代元素，因此如果你只希望改变背景的透明度，可以使用 `rgba` 或 `hsla` 来避免影响子元素的透明度。

### `animation` 属性在 CSS 中的作用

CSS `animation` 属性使得元素能够进行动态效果的变化，包括平滑过渡、旋转、位移、缩放等。与 `transition` 不同，`animation` 能够在指定的时间段内多次播放、重复播放，并且可以定义多个关键帧，使得动画更加复杂和灵活。

### `animation` 属性的语法

```css
css
复制编辑
animation: name duration timing-function delay iteration-count direction fill-mode play-state;

```

- **`name`**：动画的名称，指向一个定义的 `@keyframes` 动画。
- **`duration`**：动画的持续时间，单位为秒（`s`）或毫秒（`ms`）。
- **`timing-function`**：控制动画速度的曲线，可以是 `linear`、`ease`、`ease-in`、`ease-out` 等。
- **`delay`**：动画开始前的延迟时间，单位为秒（`s`）或毫秒（`ms`）。
- **`iteration-count`**：动画的重复次数，可以是一个数字，表示重复的次数，也可以是 `infinite`，表示无限循环。
- **`direction`**：动画的方向，控制动画是否反向或正向播放。常见的取值有 `normal`（正向播放）、`reverse`（反向播放）、`alternate`（交替播放）。
- **`fill-mode`**：动画播放结束后元素的样式如何表现。可以是 `none`（不保留动画状态）、`forwards`（保留最后一帧样式）、`backwards`（动画开始前，保留初始样式）。
- **`play-state`**：控制动画的播放状态。可以是 `running`（播放中）或 `paused`（暂停）。

### 基本用法

### 1. **定义 `@keyframes` 动画**

`@keyframes` 用于创建一组关键帧，描述动画在不同时间点的状态。

```css
css
复制编辑
@keyframes slide {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(100px);
  }
}

```

在这个例子中，动画名称为 `slide`，它将元素从 `x` 轴上的初始位置平移到 `100px` 的位置。

### 2. **使用 `animation` 应用动画**

```css
css
复制编辑
.box {
  width: 100px;
  height: 100px;
  background-color: red;
  animation: slide 2s ease-in-out 0s infinite alternate;
}

```

**解释**：

- `slide` 是动画的名称，指向 `@keyframes` 定义的动画。
- `2s` 是动画的持续时间，表示动画需要 2 秒完成。
- `ease-in-out` 是 `timing-function`，表示动画速度在开始和结束时较慢，中间较快。
- `0s` 是动画的延迟时间，表示动画立即开始。
- `infinite` 表示动画无限循环。
- `alternate` 表示动画会交替方向播放，即第一次从 `0%` 到 `100%`，然后从 `100%` 到 `0%`，以此类推。

### `@keyframes` 的更复杂示例

你可以在 `@keyframes` 中定义多个关键帧，动画会在这些关键帧之间过渡。每个关键帧都可以包含不同的属性值。

### 示例：颜色渐变动画

```css
css
复制编辑
@keyframes colorChange {
  0% {
    background-color: red;
  }
  50% {
    background-color: yellow;
  }
  100% {
    background-color: blue;
  }
}

.box {
  width: 100px;
  height: 100px;
  animation: colorChange 3s infinite;
}

```

**解释**：

- 这个动画会将 `.box` 元素的背景颜色在 3 秒内从红色变成黄色，再变成蓝色，且会无限循环。

### `animation` 的常见用法

### 1. **逐渐淡入/淡出**

可以使用 `opacity` 和 `@keyframes` 来实现元素的淡入淡出效果。

```css
css
复制编辑
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

.box {
  width: 100px;
  height: 100px;
  background-color: red;
  animation: fadeIn 2s ease-in-out forwards;
}

```

**解释**：

- `.box` 元素会在 2 秒内逐渐由完全透明变为完全不透明。

### 2. **平移动画**

通过 `transform: translate()`，可以使元素沿着某个方向移动。

```css
css
复制编辑
@keyframes moveRight {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(300px);
  }
}

.box {
  width: 100px;
  height: 100px;
  background-color: blue;
  animation: moveRight 3s linear infinite;
}

```

**解释**：

- 这个动画使 `.box` 元素在 3 秒内向右平移 300px，且会无限循环。

### 3. **旋转动画**

旋转效果通常通过 `transform: rotate()` 来实现。

```css
css
复制编辑
@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.box {
  width: 100px;
  height: 100px;
  background-color: green;
  animation: rotate 4s linear infinite;
}

```

**解释**：

- 这个动画会让 `.box` 元素旋转 360 度，持续 4 秒，且会无限循环。

### 多个动画同时进行

你可以为一个元素定义多个动画，使它们在同一时间运行。

```css
css
复制编辑
@keyframes moveAndFade {
  0% {
    transform: translateX(0);
    opacity: 0;
  }
  100% {
    transform: translateX(200px);
    opacity: 1;
  }
}

.box {
  width: 100px;
  height: 100px;
  background-color: orange;
  animation: moveAndFade 3s ease-in-out infinite;
}

```

**解释**：

- `.box` 元素同时进行平移和渐变效果，元素将在 3 秒内同时从透明到不透明并向右移动 200px。

### `animation` 的其他常用属性

### 1. **`animation-timing-function`** (动画速度曲线)

- 用于指定动画的速度曲线，控制动画的加速和减速。常见的值有：
    - `linear`：匀速。
    - `ease`：先慢后快。
    - `ease-in`：由慢到快。
    - `ease-out`：由快到慢。
    - `ease-in-out`：先慢后快，再慢。

### 2. **`animation-delay`** (动画延迟)

- 指定动画开始之前的延迟时间，单位为秒（`s`）或毫秒（`ms`）。

### 3. **`animation-iteration-count`** (动画循环次数)

- 指定动画播放的次数。可以是数字，也可以是 `infinite`（无限循环）。

### 4. **`animation-direction`** (动画方向)

- 控制动画是否反向播放，常见的值：
    - `normal`：正向播放（默认）。
    - `reverse`：反向播放。
    - `alternate`：交替播放（正向和反向交替）。
    - `alternate-reverse`：交替反向播放。

### 5. **`animation-fill-mode`** (动画填充模式)

- 控制动画播放完毕后，元素的最终样式。
    - `none`（默认）：动画结束后恢复到原始状态。
    - `forwards`：保持最后一帧的样式。
    - `backwards`：动画开始前使用起始帧的样式。
    - `both`：结合 `forwards` 和 `backwards`。

# CSS高级

# 性能优化

## **1. CSS 基础**

### 1.1 CSS 概述

- CSS 的作用
- CSS 的基本语法
- CSS 的引入方式（内联、内部样式表、外部样式表）

### 1.2 选择器（Selectors）

- 基本选择器：元素选择器、类选择器、ID 选择器、通配符
- 组合选择器：后代选择器（`A B`）、子选择器（`A > B`）、相邻兄弟选择器（`A + B`）、通用兄弟选择器（`A ~ B`）
- 伪类选择器（`:hover`、`:focus`、`:nth-child()`等）
- 伪元素选择器（`::before`、`::after`等）
- 属性选择器（`[type="text"]`、`[class^="prefix"]`等）

### 1.3 继承与层叠（Cascade & Inheritance）

- 继承（哪些属性可以继承，哪些不可以）
- 层叠（层叠规则、优先级）
- `!important` 的使用
- `initial`、`inherit`、`unset`

---

## **2. 盒模型（Box Model）**

### 2.1 盒模型概述

- 标准盒模型（content-box）
- IE 盒模型（border-box）

### 2.2 盒子属性

- `width`、`height`
- `padding`、`margin`（外边距塌陷问题）
- `border`
- `outline`（与 `border` 的区别）

---

## **3. 布局（Layout）**

### 3.1 普通流（Normal Flow）

- 块级元素与行内元素
- `display` 属性
- `visibility` 与 `opacity` 的区别

### 3.2 浮动（Float）

- `float` 属性
- `clear` 及 BFC（块级格式化上下文）

### 3.3 定位（Position）

- `static`
- `relative`
- `absolute`
- `fixed`
- `sticky`
- `z-index`（层级关系）

### 3.4 Flexbox（弹性布局）

- `display: flex`
- 主轴与交叉轴
- `flex-direction`
- `justify-content`
- `align-items`
- `align-self`
- `flex-wrap`
- `flex-grow`、`flex-shrink`、`flex-basis`

### 3.5 Grid（网格布局）

- `display: grid`
- `grid-template-columns` / `grid-template-rows`
- `grid-template-areas`
- `grid-auto-flow`
- `align-content`、`align-items`、`justify-content`
- `grid-gap`（`gap`）

### 3.6 多列布局（Multi-Column Layout）

- `column-count`、`column-width`
- `column-gap`
- `column-rule`

### 3.7 CSS 现代布局技术

- `container queries`
- `subgrid`

---

## **4. 文本与字体**

### 4.1 字体相关属性

- `font-family`
- `font-size`（px、em、rem、vw、vh）
- `font-weight`
- `font-style`
- `line-height`
- `letter-spacing`
- `word-spacing`
- `text-transform`

### 4.2 文本相关属性

- `text-align`
- `text-decoration`
- `text-indent`
- `white-space`
- `text-overflow`

---

## **5. 颜色与背景**

### 5.1 颜色（Color）

- 颜色表示法（十六进制、rgb、hsl）
- 透明度（`opacity` vs `rgba()`）

### 5.2 背景（Background）

- `background-color`
- `background-image`
- `background-repeat`
- `background-position`
- `background-size`（cover、contain）
- `background-attachment`

---

## **6. 过渡、动画与变换**

### 6.1 过渡（Transitions）

- `transition-property`
- `transition-duration`
- `transition-timing-function`
- `transition-delay`

### 6.2 动画（Animations）

- `@keyframes`
- `animation-name`
- `animation-duration`
- `animation-timing-function`
- `animation-iteration-count`
- `animation-fill-mode`
- `animation-delay`

### 6.3 变换（Transforms）

- `translate`（移动）
- `scale`（缩放）
- `rotate`（旋转）
- `skew`（倾斜）

---

## **7. 响应式设计**

### 7.1 媒体查询（Media Queries）

- `@media` 规则（`min-width`、`max-width`）
- `@media screen and (orientation: landscape)`

### 7.2 相对单位与视口单位

- `em`、`rem`
- `vw`、`vh`
- `vmin`、`vmax`

### 7.3 CSS 现代特性

- `clamp()`
- `min()` / `max()`
- `aspect-ratio`

---

## **8. CSS 高级特性**

### 8.1 变量（CSS Variables）

- `-custom-property`
- `var()`

### 8.2 计算属性（calc()）

- `calc(100% - 50px)`

### 8.3 滤镜（Filters）

- `filter: blur(5px);`
- `filter: brightness(150%);`
- `filter: grayscale(100%);`

### 8.4 滚动（Scrolling）

- `scroll-behavior: smooth`
- `overflow`（hidden、scroll、auto）
- `overscroll-behavior`

### 8.5 自定义滚动条（::-webkit-scrollbar）

- `::-webkit-scrollbar`
- `::-webkit-scrollbar-thumb`

### 8.6 伪元素与内容生成

- `content: ""`
- `::before`、`::after`

---

## **9. CSS 预处理器（Sass/Less）**

### 9.1 Sass 基础

- 变量 `$`
- 嵌套
- `@mixin`、`@include`
- `@extend`
- `@function`

### 9.2 Less 基础

- 变量 `@`
- 嵌套
- `mixins`
- `extend`

---

## **10. 浏览器兼容性与性能优化**

### 10.1 兼容性问题

- `can i use` 查询支持情况
- `autoprefixer` 处理兼容性

### 10.2 CSS 优化

- 避免使用 `!important`
- 选择器优化（减少层级）
- 避免过度使用 `float`
- 使用合适的 `font-display`
- 采用 `lazy-loading` 方式减少渲染阻塞

---

## **11. CSS 最新特性**

### 11.1 子网格（Subgrid）

- `grid-template-columns: subgrid;`
- `grid-template-rows: subgrid;`

### 11.2 容器查询（Container Queries）

- `@container`
- `container-type: inline-size`

### 11.3 `:has()` 选择器

- `:has(.child)`




























