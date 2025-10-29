## DOM树
### 节点类型
- **文档节点（Document）**
	- **描述**：表示整个 HTML 或 XML 文档，是 DOM 树的根节点。
	- **接口**：`Document`
	- **特点**：
		- 只有一个文档节点，通常通过 `document` 对象访问。
		- 提供全局方法，如 `getElementById`、`querySelector`、`createElement`。
	- **示例**：
		```js
		console.log(document); // Document 对象
		console.log(document.nodeType); // 9 (DOCUMENT_NODE)
		```
- **元素节点（Element）**
	- **描述**：表示 HTML 或 XML 中的标签（如 `<div>`、`<p>`、`<span>`）。
	- **接口**：`Element`（继承自 `Node`）
	- **特点**：
		- 是 DOM 树的主要结构单元，可包含子节点（其他元素、文本等）。
		- 支持操作属性（如 `id`、`class`）和样式（如 `style`）。
	- **示例**：
		```js
		const div = document.createElement("div");
		console.log(div.nodeType); // 1 (ELEMENT_NODE)
		```
- **属性节点（Attr）**
	- **描述**：表示元素的属性（如 `id`、`class`、`data-*`）。
	- **接口**：`Attr`
	- **特点**：
		- 绑定到特定元素节点，通过 `getAttribute`、`setAttribute` 操作。
		- 现代 DOM 中较少直接操作属性节点，通常通过元素对象的属性访问（如 `element.id`）。
	- **示例**：
		```js
		const div = document.createElement("div");
		div.setAttribute("id", "myDiv");
		console.log(div.getAttributeNode("id").nodeType); // 2 (ATTRIBUTE_NODE)
		```
- **文本节点（Text）**
	- **描述**：表示元素或属性中的文本内容。
	- **接口**：`Text`
	- **特点**：
		- 通常是元素节点的子节点，包含纯文本。
		- 不包含子节点（叶子节点）。
		- 可通过 `textContent` 或 `nodeValue` 修改。
	- **示例**：
		```js
		const text = document.createTextNode("Hello, World!");
		console.log(text.nodeType); // 3 (TEXT_NODE)
		```
- **注释节点（Comment）**
	- **描述**：表示 HTML 或 XML 中的注释（如 `<!-- comment -->`）。
	- **接口**：`Comment`
	- **特点**：
		- 不影响页面渲染，但可用于调试或存储元数据。
		- 可通过 `document.createComment` 创建。
	- **示例**：
		```js
		const comment = document.createComment("This is a comment");
		console.log(comment.nodeType); // 8 (COMMENT_NODE)
		```
- **其他节点类型**
	- **文档类型节点（DocumentType）**：
		- 表示文档的 `<!DOCTYPE>` 声明。
		- 接口：`DocumentType`
		- 示例：`document.doctype`（nodeType: 10）。
	- **文档片段节点（DocumentFragment）**：
		- 轻量级容器，用于临时存储节点，优化批量 DOM 操作。
		- 接口：`DocumentFragment`
		- 示例：`document.createDocumentFragment()`（nodeType: 11）。
	- **CDATA 节点（CDATASection）**：
		- 用于 XML 文档中存储未解析的文本（如 `<![CDATA[...]]>`）。
		- 较少用于 HTML，nodeType 为 4。
	- **处理指令节点（ProcessingInstruction）**：
		- 用于 XML 处理指令（如 `<?xml-stylesheet ... ?>`）。
		- nodeType 为 7，HTML 中少见。
- **节点类型常量**
	- 每种节点类型对应一个 `nodeType` 常量：
		- `ELEMENT_NODE`: 1
		- `ATTRIBUTE_NODE`: 2
		- `TEXT_NODE`: 3
		- `CDATA_SECTION_NODE`: 4
		- `PROCESSING_INSTRUCTION_NODE`: 7
		- `COMMENT_NODE`: 8
		- `DOCUMENT_NODE`: 9
		- `DOCUMENT_TYPE_NODE`: 10
		- `DOCUMENT_FRAGMENT_NODE`: 11
	- **检查节点类型**：
		```js
		const node = document.querySelector("div");
		console.log(node.nodeType === Node.ELEMENT_NODE); // true
		```









### 节点关系
- **父节点**
	- **属性**：`node.parentNode`
	- **描述**：返回当前节点的父节点（通常是元素节点或文档节点）。
	- **特点**：
		- 文档节点的 `parentNode` 为 `null`。
		- 如果节点未插入 DOM 树，`parentNode` 也为 `null`。
	- **示例**：
		```js
		const div = document.querySelector("div");
		console.log(div.parentNode); // 通常是 <body> 或其他父元素
		```
- **子节点**
	- **属性**：
		- `node.childNodes`：返回所有子节点（包括元素、文本、注释）的 `NodeList`。
		- `node.children`：仅返回元素子节点的 `HTMLCollection`。
		- `node.firstChild`：第一个子节点（可能为文本或注释）。
		- `node.lastChild`：最后一个子节点。
		- `node.firstElementChild`：第一个元素子节点。
		- `node.lastElementChild`：最后一个元素子节点。
	- **特点**：
		- `childNodes` 包含所有节点类型，需注意空白文本节点。
		- `children` 更常用于只处理元素节点。
	- **示例**：
		```js
		const div = document.querySelector("div");
		console.log(div.childNodes); // NodeList [text, span, text]
		console.log(div.children); // HTMLCollection [span]
		```
- **兄弟节点**
	- **属性**：
		- `node.nextSibling`：下一个兄弟节点（包括文本、注释）。
		- `node.previousSibling`：上一个兄弟节点。
		- `node.nextElementSibling`：下一个元素兄弟节点。
		- `node.previousElementSibling`：上一个元素兄弟节点。
	- **特点**：
		- 空白文本节点可能影响 `nextSibling` 和 `previousSibling`。
		- `nextElementSibling` 和 `previousElementSibling` 忽略非元素节点。
	- **示例**：
		```js
		const span = document.querySelector("span");
		console.log(span.nextSibling); // 可能是文本节点
		console.log(span.nextElementSibling); // 下一个元素节点
		```
- **层次导航**
	- **属性**：`node.ownerDocument`
	- **描述**：返回节点所属的 Document 对象。
	- **用途**：从任何节点追溯到文档根节点。
	- **示例**：
		```js
		const span = document.querySelector("span");
		console.log(span.ownerDocument === document); // true
		```
### DOM树示例
- **HTML代码**
	```html
	<!DOCTYPE html>
	<html>
	  <head>
	    <title>My Page</title>
	  </head>
	  <body>
	    <div id="container">
	      <p>Hello <span>World</span>!</p>
	      <!-- Comment -->
	    </div>
	  </body>
	</html>
	```
- **DOM树**
	```text
	Document (nodeType: 9)
	├── DocumentType (<!DOCTYPE html>) (nodeType: 10)
	└── html (nodeType: 1)
	    ├── head (nodeType: 1)
	    │   └── title (nodeType: 1)
	    │       └── Text: "My Page" (nodeType: 3)
	    └── body (nodeType: 1)
	        └── div (id="container") (nodeType: 1)
	            ├── p (nodeType: 1)
	            │   ├── Text: "Hello " (nodeType: 3)
	            │   ├── span (nodeType: 1)
	            │   │   └── Text: "World" (nodeType: 3)
	            │   └── Text: "!" (nodeType: 3)
	            └── Comment: "Comment" (nodeType: 8)
	```
## 访问DOM元素
### 传统方法
- **通过 ID 获取元素**
	- **定义**：根据元素的 `id` 属性返回单一元素节点。如果未找到，返回 `null`。
	- **方法**：`document.getElementById(id)`
	- **特点**：
		- ID 在文档中应唯一，查询效率高。
		- 仅适用于 `document` 对象，不支持其他节点。
	- **示例**：
		```js
		const div = document.getElementById("myDiv");
		console.log(div); // <div id="myDiv">...</div>
		```
- **通过标签名获取元素**
	- **定义**：返回指定标签名的所有元素的 `HTMLCollection`（动态集合）。
	- **方法**：`document.getElementsByTagName(tagName)`
	- **特点**：
		- 可在 `document` 或元素节点上调用（如 `element.getElementsByTagName`）。
		- 返回空集合（`HTMLCollection`）如果无匹配元素。
		- 标签名大小写不敏感（如 `"div"` 或 `"DIV"`）。
	- **示例**：
		```js
		const divs = document.getElementsByTagName("div");
		console.log(divs); // HTMLCollection [div, div, ...]
		for (let div of divs) {
		  console.log(div.textContent);
		}
		```
- **通过类名获取元素**
	- **定义**：返回具有指定类名的所有元素的 `HTMLCollection`。
	- **方法**：`document.getElementsByClassName(className)`
	- **特点**：
		- 支持多个类名（空格分隔，如 `"class1 class2"`）。
		- 可在 `document` 或元素节点上调用。
		- 动态更新：DOM 变化会实时反映到集合中。
	- **示例**：
		```js
		const items = document.getElementsByClassName("item");
		console.log(items); // HTMLCollection [div.item, span.item, ...]
		```
- **通过 name 属性获取元素**
	- **定义**：返回具有指定 `name` 属性的所有元素的 `NodeList`。
	- **方法**：`document.getElementsByName(name)`
	- **特点**：
		- 常用于表单元素（如 `<input>`、`<select>`）。
		- 仅适用于 `document` 对象。
		- 返回 `NodeList`，非动态集合。
	- **示例**：
		```js
		const inputs = document.getElementsByName("username");
		console.log(inputs); // NodeList [input, input, ...]
		```
### 现代方法
- **querySelector**
	- **定义**：返回匹配指定 CSS 选择器的第一个元素，未找到返回 `null`。
	- **方法**：`document.querySelector(selector)`
	- **特点**：
		- 支持复杂的 CSS 选择器（如 `#id`, `.class`, `div > p`）。
		- 可在 `document` 或元素节点上调用。
		- 效率略低于 `getElementById`（因解析选择器）。
	- **示例**：
		```js
		const firstItem = document.querySelector(".item");
		console.log(firstItem); // <div class="item">...</div>
		const nested = document.querySelector("div > p:first-child");
		```
- **querySelectorAll**
	- **定义**：返回匹配指定 CSS 选择器的所有元素的 `NodeList`（静态集合）。
	- **方法**：`document.querySelectorAll(selector)`
	- **特点**：
		- 支持与 `querySelector` 相同的选择器语法。
		- 返回的 `NodeList` 可使用 `forEach`、`for...of` 遍历。
		- 非动态集合：DOM 变化不会更新结果。
	- **示例**：
		```js
		const items = document.querySelectorAll(".item");
		items.forEach(item => console.log(item.textContent));
		```
### 特殊方法
- `document.documentElement`：返回 `<html>` 元素节点。
- `document.body`：返回 `<body>` 元素节点。
- `document.head`：返回 `<head>` 元素节点。
- `document.forms`：返回文档中所有 `<form>` 元素的 `HTMLCollection`。
- `document.images`：返回文档中所有 `<img>` 元素的 `HTMLCollection`。
- `document.links`：所有 `<a>` 和 `<area>` 元素。
- `document.scripts`：所有 `<script>` 元素。
- `document.anchors`：具有 `name` 属性的 `<a>` 元素（较少使用）。
## NodeList 与 HTMLCollection
### HTMLCollaction
- **定义**：动态集合，DOM 变化会实时更新。
- **方法返回**：`getElementsByTagName`、`getElementsByClassName`、document.`forms` 等。
- **特点**：
	- 仅包含元素节点。
	- 支持索引访问（如 `collection[0]`）和 `namedItem`（如 `document.forms["myForm"]`）。
	- 不支持 `forEach`，需转换为数组或使用 `for...of`。
- **示例**：
	```js
	const divs = document.getElementsByTagName("div");
	console.log(divs instanceof HTMLCollection); // true
	
	const divs = document.getElementsByClassName("item");
	const divArray = Array.from(divs);
	divArray.map(div => div.textContent);
	// 或使用展开运算符
	const divArray2 = [...divs];
	```
### NodeList
- **定义**：静态集合（`querySelectorAll`）或动态集合（`childNodes`）。
- **方法返回**：`querySelectorAll`、`getElementsByName`、`childNodes` 等。
- **特点**：
	- 可包含任何节点类型（元素、文本、注释）。
	- `querySelectorAll` 返回的 `NodeList` 支持 `forEach`。
- **示例**：
	```js
	const nodes = document.querySelectorAll("div");
	console.log(nodes instanceof NodeList); // true
	nodes.forEach(node => console.log(node));
	```
## 操作DOM元素
### 创建DOM节点
- **创建元素节点**
	- **定义**：创建指定标签名的元素节点（如 `<div>`、`<p>`）。
	- **方法**：`document.createElement(tagName)`
	- **特点**：
		- 返回一个未插入 DOM 树的元素节点。
		- 可设置属性、内容或事件监听器后插入。
	- **示例**：
		```js
		const div = document.createElement("div");
		div.id = "myDiv";
		div.textContent = "New Div";
		```
- **创建文本节点**
	- **定义**：创建包含指定文本的文本节点。
	- **方法**：`document.createTextNode(text)`
	- **特点**：
		- 文本节点是叶子节点，不能包含子节点。
		- 常用于设置元素的内容。
	- **示例**：
		```js
		const text = document.createTextNode("Hello, World!");
		const p = document.createElement("p");
		p.appendChild(text);
		```
- **创建文档片段**
	- 定义：创建轻量级容器，用于临时存储多个节点，优化批量操作。
	- 方法：`document.createDocumentFragment()`
	- 特点：
		- 插入文档时，片段本身消失，仅其子节点被插入。
		- 减少 DOM 重排，提高性能。
	- 示例：
		```js
		const fragment = document.createDocumentFragment();
		for (let i = 0; i < 3; i++) {
		  const li = document.createElement("li");
		  li.textContent = `Item ${i}`;
		  fragment.appendChild(li);
		}
		document.querySelector("ul").appendChild(fragment);
		```
- **创建其它节点**
	- 注释节点：`document.createComment(text)`
		```js
		const comment = document.createComment("This is a comment");
		document.body.appendChild(comment);
		```
	- 少见节点：如 `document.createAttribute(name)` 创建属性节点（较少直接使用）。
### 修改DOM节点
- **插入节点**
	- **追加子节点**：
		- 定义：将节点追加为目标节点的最后一个子节点。
		- 方法：`node.appendChild(child)`
		- 注意：如果节点已存在于 DOM 树，会被移动。
		- 示例：
			```js
			const div = document.createElement("div");
			div.textContent = "New Div";
			document.body.appendChild(div);
			```
	- **插入到指定位置**：
		- 定义：将新节点插入到参考节点之前。
		- 方法：`node.insertBefore(newNode, referenceNode)`
		- 注意：`referenceNode` 为 `null` 时效果等同于 `appendChild`。
		- 示例：
			```js
			const newP = document.createElement("p");
			newP.textContent = "Inserted Paragraph";
			const ref = document.querySelector("p");
			document.body.insertBefore(newP, ref);
			```
	- **现代插入方法**：
		- `node.append(...nodes)`：追加多个节点或字符串。
		- `node.prepend(...nodes)`：插入到子节点列表开头。
		- `node.before(...nodes)`：插入到节点之前。
		- `node.after(...nodes)`：插入到节点之后。
		- 示例：
			```js
			const div = document.createElement("div");
			div.append("Text", document.createElement("span"));
			document.body.prepend(div);
			```
- **删除节点**
	- **移除子节点**：
		- 定义：从父节点中移除指定子节点，返回被移除的节点。
		- 方法：`node.removeChild(child)`
		- 示例：
			```js
			const span = document.querySelector("span");
			span.parentNode.removeChild(span);
			```
	- **移除自身**：
		- 定义：直接移除节点（现代浏览器支持）。
		- 方法：`node.remove()`
		- 示例：
			```js
			document.querySelector("span").remove();
			```
- **替换节点**
	- 定义：用新节点替换目标节点的子节点，返回被替换的节点。
	- 方法：`node.replaceChild(newNode, oldNode)`
	- 示例：
		```js
		const newSpan = document.createElement("span");
		newSpan.textContent = "New Span";
		const oldSpan = document.querySelector("span");
		oldSpan.parentNode.replaceChild(newSpan, oldSpan);
		```
- **克隆节点**
	- 定义：复制节点，`deep` 参数决定是否复制子节点。
	- 方法：`node.cloneNode(deep)`
	- 注意：
		- `deep: true` 复制整个子树。
		- `deep: false` 仅复制当前节点。
		- 克隆的节点不自动插入 DOM 树。
	- 示例：
		```js
		const div = document.querySelector("#myDiv");
		const clone = div.cloneNode(true);
		document.body.appendChild(clone);
		```
### 操作元素属性
- **获取和设置属性**
	- 方法：
		- `element.getAttribute(name)`：获取属性值。
		- `element.setAttribute(name, value)`：设置属性值。
	- 注意：
		- 适用于所有属性，包括自定义属性。
		- 属性值始终为字符串。
	- 示例：
		```js
		const div = document.getElementById("myDiv");
		div.setAttribute("data-type", "example");
		console.log(div.getAttribute("data-type")); // "example"
		```
- **删除属性**
	- 定义：移除指定属性。
	- 方法：`element.removeAttribute(name)`
	- 示例：
		```js
		div.removeAttribute("data-type");
		console.log(div.hasAttribute("data-type")); // false
		```
- **直接访问标准属性**
	- 定义：标准属性（如 `id`、`className`、`value`）可直接通过元素对象访问。
	- 示例：
		```js
		const input = document.querySelector("input");
		input.value = "New Value";
		console.log(input.id); // 获取 id 属性
		```
- **自定义属性**
	- 定义：访问或设置 `data-*` 属性的值，属性名自动转换为驼峰命名。
	- 方法：`element.dataset`
	- 示例：
		```js
		const div = document.createElement("div");
		div.dataset.type = "example";
		console.log(div.dataset.type); // "example"
		console.log(div.getAttribute("data-type")); // "example"
		```
### 操作类
- **className**
	- 定义：获取或设置元素的类名字符串。
	- 属性：`element.className`
	- 注意：
		- 设置时覆盖所有现有类。
		- 需要手动处理类名拼接。
	- 示例：
		```js
		const div = document.getElementById("myDiv");
		div.className = "active highlight";
		```
- **classList**
	- 定义：提供操作类名的方法，返回 `DOMTokenList`。
	- 属性：`element.classList`
	- 方法：
		- `add(class)`：添加类。
		- `remove(class)`：移除类。
		- `toggle(class)`：切换类（存在则移除，不存在则添加）。
		- `contains(class)`：检查是否包含类。
		- `replace(oldClass, newClass)`：替换类。
	- 示例：
		```js
		const div = document.getElementById("myDiv");
		div.classList.add("active");
		div.classList.toggle("highlight");
		console.log(div.classList.contains("active")); // true
		div.classList.replace("active", "inactive");
		```
### 操作样式
- **内联样式**
	- 定义：获取或设置元素的内联 CSS 样式（`style` 属性）。
	- 属性：`element.style`
	- 注意：
		- 属性名使用驼峰命名（如 `backgroundColor` 而非 `background-color`）。
		- 仅影响内联样式，不影响外部 CSS。
	- 示例：
		```js
		const div = document.getElementById("myDiv");
		div.style.backgroundColor = "blue";
		div.style.fontSize = "16px";
		```
- **获取计算属性**
	- 定义：返回元素的最终计算样式（包括外部 CSS 和浏览器默认样式）。
	- 方法：`window.getComputedStyle(element, pseudoElement)`
	- 注意：
		- 返回 `CSSStyleDeclaration` 对象，只读。
		- 可获取伪元素样式（如 `::before`）。
	- 示例：
		```js
		const div = document.getElementById("myDiv");
		const styles = window.getComputedStyle(div);
		console.log(styles.backgroundColor); // "rgb(0, 0, 255)"
		```
- **通过类修改样式**
	- 定义：使用 `classList` 修改类，结合外部 CSS 实现样式更新。
	- 优点：分离样式和逻辑，易于维护。
	- 示例：
		```js
		div.classList.add("highlight");
		// CSS: .highlight { background-color: yellow; }
		```
### 操作内容
- **textContent**
	- 定义：获取或设置元素的纯文本内容（包括所有子节点的文本）。
	- 属性：`element.textContent`
	- 注意：
		- 忽略 HTML 标签，防止 XSS。
		- 适用于文本节点和元素节点。
	- 示例：
		```js
		const div = document.getElementById("myDiv");
		div.textContent = "Safe text content";
		```
- **innerHTML**
	- 定义：获取或设置元素的 HTML 内容。
	- 属性：`element.innerHTML`
	- 注意：
		- 解析 HTML 字符串，可能引入脚本或标签。
		- 需注意 XSS 风险，避免直接插入用户输入。
	- 示例：
		```js
		div.innerHTML = "<p>Bold <b>text</b></p>";
		```
- **innerText**
	- 定义：获取或设置元素的可见文本内容，考虑 CSS 样式（如 `display: none`）。
	- 属性：`element.innerText`
	- 注意：
		- 性能较低（触发重排）。
		- 不推荐使用，优先 `textContent`。
	- 示例：
		```js
		div.innerText = "Visible text";
		```
## DOM事件
### 事件模型
- **DOMO事件**
	- 定义：早期事件绑定方式，直接在元素上设置事件处理属性。
	- 语法：`element.onevent = function() {}`
	- 特点：
		- 简单直观，但每个事件类型只能绑定一个处理函数。
		- 覆盖已有处理函数。
		- 不支持事件捕获。
	- 示例：
		```js
		document.getElementById("myButton").onclick = function() {
		  alert("Button clicked!");
		};
		```
- **DOM2事件**
	- 定义：通过 `addEventListener` 绑定事件，支持更灵活的处理。
	- 方法：
		- `element.addEventListener(event, handler, options)`
		- `element.removeEventListener(event, handler, options)`
	- 特点：
		- 支持为同一事件绑定多个处理函数。
		- 支持事件捕获和冒泡阶段。
		- 可通过 `options` 配置一次性事件或被动监听。
		- 移除事件时，必须引用相同的处理函数（不能使用匿名函数）。
	- 参数：
		- `capture: boolean：true` 为捕获阶段，`false` 为冒泡阶段（默认）。
		- `once: boolean：true` 表示事件触发一次后自动移除。
		- `passive: boolean：true` 表示不调用 `preventDefault`，优化性能（如滚动事件）。
	- 示例：
		```js
		const button = document.getElementById("myButton");
		button.addEventListener("click", () => {
		  console.log("Clicked!");
		}, { once: true });

		const handler = () => console.log("Clicked!");
		button.addEventListener("click", handler);
		button.removeEventListener("click", handler);
		```
### 事件流
- **定义**
	- 事件流描述了事件在 DOM 树中的传播路径，包括捕获、目标和冒泡阶段。
	- W3C 标准定义了事件传播的三个阶段：
		- **捕获阶段**：事件从 `document` 向下传播到目标元素。
		- **目标阶段**：事件到达目标元素，触发绑定的事件处理函数。
		- **冒泡阶段**：事件从目标元素向上传播到 `document`。
- **控制事件流**
	- **阻止默认行为**：
		- 定义：阻止事件的默认行为（如阻止表单提交或链接跳转）。
		- 方法：`event.preventDefault()`
		- 示例：
			```js
			document.querySelector("a").addEventListener("click", (event) => {
			  event.preventDefault();
			  console.log("Link click prevented");
			});
			```
	- **阻止冒泡**：
		- 定义：阻止事件继续向上冒泡（不影响默认行为）。
		- 方法：`event.stopPropagation()`
		- 示例：
			```js
			document.querySelector("div").addEventListener("click", (event) => {
			  event.stopPropagation();
			  console.log("Div click, no bubbling");
			});
			```
	- **立即停止**：
		- 定义：阻止冒泡并阻止同一元素上的其他同类型事件处理函数。
		- 方法：`event.stopImmediatePropagation()`
		- 示例：
			```js
			button.addEventListener("click", (event) => {
			  event.stopImmediatePropagation();
			  console.log("First handler");
			});
			button.addEventListener("click", () => {
			  console.log("This won't run");
			});
			```
- **事件捕获**
	- 定义：通过 `addEventListener` 的 `capture: true` 选项在捕获阶段处理事件。
	- 特点：适合在父元素上拦截事件（如日志记录）。
	- 示例：
		```js
		document.body.addEventListener("click", () => {
		  console.log("Captured in body");
		}, { capture: true });
		```
### 事件类型
- **鼠标事件**
	- 常见事件：
		- `click`：鼠标单击。
		- `dblclick`：鼠标双击。
		- `mousedown`：鼠标按下。
		- `mouseup`：鼠标释放。
		- `mouseover`：鼠标进入（包括子元素）。
		- `mouseout`：鼠标离开（包括子元素）。
		- `mouseenter`：鼠标进入（不包括子元素）。
		- `mouseleave`：鼠标离开（不包括子元素）。
		- `mousemove`：鼠标移动。
	- 示例：
		```js
		document.querySelector("div").addEventListener("mouseenter", () => {
		  console.log("Mouse entered div");
		});
		```
- **键盘事件**
	- 常见事件：
		- `keydown`：按下键盘按键。
		- `keyup`：释放键盘按键。
		- `keypress`：按下可生成字符的按键（已弃用）。
	- 属性：
		- `event.key`：按下的键值（如 `"Enter"`）。
		- `event.code`：物理键代码（如 `"KeyA"`）。
		- `event.shiftKey`、`event.ctrlKey`、`event.altKey`：修饰键状态。
	- 示例：
		```js
		document.addEventListener("keydown", (event) => {
		  if (event.key === "Enter") {
		    console.log("Enter key pressed");
		  }
		});
		```
- **表单事件**
	- 常见事件：
		- `submit`：表单提交。
		- `change`：表单控件值变化。
		- `input`：输入框内容变化（实时）。
		- `focus`：元素获得焦点。
		- `blur`：元素失去焦点。
	- 示例：
		```js
		document.querySelector("form").addEventListener("submit", (event) => {
		  event.preventDefault();
		  console.log("Form submitted");
		});
		```
- **窗口和文档事件**
	- 常见事件：
		- `load`：页面或资源加载完成。
		- `resize`：窗口大小变化。
		- `scroll`：页面或元素滚动。
		- `unload`：页面卸载（少用）。
	- 示例：
		```js
		window.addEventListener("resize", () => {
		  console.log("Window resized to", window.innerWidth);
		});
		```
- **触摸事件**
	- 常见事件：
		- `touchstart`：触摸开始。
		- `touchmove`：触摸移动。
		- `touchend`：触摸结束。
	- 属性：
		- `event.touches`：当前触摸点列表。
		- `event.targetTouches`：目标元素上的触摸点。
		- `event.changedTouches`：变化的触摸点。
	- 示例：
		```js
		document.querySelector("div").addEventListener("touchstart", (event) => {
		  console.log("Touch started", event.touches.length);
		});
		```
- **其它事件**
	- **拖放事件**：
		- `dragstart`, `drag`, `dragend`, `dragover`, `drop`
	- **媒体事件**
		- `play`, `pause`, `ended`（用于 `<video>`、`<audio>`）。
	- **自定义事件**
		- 定义：使用 `CustomEvent` 创建自定义事件。
		- 示例：
			```js
			const event = new CustomEvent("myEvent", { detail: { data: "example" } });
			document.dispatchEvent(event);
			document.addEventListener("myEvent", (e) => {
			  console.log(e.detail.data); // "example"
			});
			```
### 事件对象
- **定义**：事件触发时，浏览器会传递一个 `Event` 对象给处理函数，包含事件相关信息。
- **常见属性**：
	- `event.type`：事件类型（如 `"click"`）。
	- `event.target`：触发事件的元素。
	- `event.currentTarget`：绑定事件处理函数的元素。
	- `event.bubbles`：事件是否冒泡。
	- `event.cancelable`：是否可取消默认行为。
- **鼠标事件属性**：
	- `clientX`, `clientY`：相对于视口的坐标。
	- `pageX`, `pageY`：相对于文档的坐标。
	- `button`：按下的鼠标按钮（0=左键，1=中键，2=右键）。
- **键盘事件属性**：`key`, `code`, `shiftKey` 等。
- **示例**：
	```js
	document.querySelector("button").addEventListener("click", (event) => {
	  console.log(`Clicked at (${event.clientX}, ${event.clientY})`);
	});
	```
### 事件委托
- 定义：事件委托是将事件监听器绑定到父元素，利用事件冒泡处理子元素的事件。
- 特点：
	- 减少事件监听器数量，提高性能。
	- 支持动态添加的元素。
- 注意：
	- 检查 `event.target` 是否匹配目标元素。
	- 使用 `closest` 查找最近的匹配祖先。
- 示例：
	```js
	document.querySelector("ul").addEventListener("click", (event) => {
	  if (event.target.tagName === "li") {
	    console.log("LI clicked:", event.target.textContent);
	  }
	});
	//---
	if (event.target.closest(".item")) {
	  console.log("Item or its child clicked");
	}
	```
## 性能优化


