## 引入HTML
---
- **行内**：可以直接在 HTML 的事件属性中写 JavaScript 代码。
	```js
	<button onclick="alert('你好，Maktub')">点我</button>	
	```
- **内部脚本**：在 HTML 页面中通过 `<script>` 标签嵌入 JS 代码，写在 `<script>` 标签内部。
	```html
	<!DOCTYPE html>
	<html>
	<head>
		<title>内部 JS 示例</title>
	</head>
	<body>
		<h1>欢迎</h1>
	
		<script>
			console.log("欢迎你，Maktub！");
		</script>
	</body>
	</html>	
	```
- **外部脚本**：将 JS 写在独立文件中，通过 `<script src="xxx.js">` 引入。一旦使用 src 属性，标签内容将被忽略。
	```html
	<!DOCTYPE html>
	<html>
	<head>
		<title>外部 JS 示例</title>
	</head>
	<body>
		<h1>你好，Maktub！</h1>
	
		<script src="main.js"></script>
	</body>
	</html>	
	```


## Script标签属性
---
- `type`
	在 HTML4 标准中，要求 script 标签有 `type` 特性。通常是 `type="text/javascript"`，但现在已经不需要了，但此属性可用于 JavaScript 模块。
- `language`
	这个特性是为了显示脚本使用的语言，默认就是 JavaScript，不再使用。
- `defer`
	下载脚本的同时不阻塞 HTML 解析，HTML 解析完再执行。
- `async`
	脚本下载完成后立即执行，**不等待 HTML 解析**。
# HTML基础

## HTML简介与历史

HTML（Hyper Text Markup Language）代表**超文本标记语言**，是创建网页的标准标记语言，描述了网页的结构，由一系列**元素**构成，这些元素告诉浏览器该如何显示内容。

HTML起源于1989年，由英国物理学家**蒂姆·伯纳斯-李**（Tim Berners-Lee）发明。HTML历经多个版本演进，HTML4.01逐渐成为了一个广泛接受的标准，为Web开发提供了一致性的标准，随着互联网的发展，人们对更多功能和语义化的需要日益增加，这促使了HTML5的开发。2014年**HTML5**正式成为了**W3C**（World Wide Web Consortium）推荐标准，为Web开发者提供了更多新特性和功能。

## HTML文档结构

> HTML文档结构指的是HTML文档的基本组成部分和结构。

一个简单的HTML文档：

```html
<!DOCTYPE html><html lang="en"><head>  <meta charset="UTF-8">  <meta name="viewport" content="width=device-width, initial-scale=1.0">  <title>Document</title></head><body>  <h1>My first Heading</h1>  <p>My first paragraph</p></body></html>
```

- **`*<!DOCTYPE html>*`**
    
    文档类型声明，指定该文档的HTML版本（HTML5），使浏览器使用相应的HTML规范解析文档。
    
- **`*<html>*`**
    
    该元素是HTML页面的根元素，包含整个HTML文档的内容，HTML5中通常有一个`*lang*`属性指定文档的语言：`*en*`表示英语、`*ja*`表示日文、`*zh-cn*`表示中文。
    
- **`*<head>*`**
    
    该元素包含HTML页面的元数据和其它信息，如标题（`*<title>*`）、字符集（`*<meta>*`）、样式表（`*<style>*`）、脚本（`*<scrpt>*`）等。这些信息对于浏览器和搜索引擎很重要，但不会直接显示在页面上。
    
- **`*<body>*`**
    
    该元素定义文档的主体，是所有可见内容的容器。
    

## HTML元素、标签和属性

HTML**元素**是HTML文档的基本构建块，由开始标签、结束标签、内容和标签属性组成。开始标签和结束标签之间即为元素的内容。有些元素是自闭合的，没有结束标签，如`*<img>*`、`*<br>*` 等。

HTML**标签**定义HTML元素的名称，有尖括号包围。

HTML**属性**定义HTML元素的额外信息，属性总是位于开始标签中。属性通常以键值对的形式出现。始终推荐给属性值添加引号。

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/72605496-699b-4dd2-851d-c59eb9c9c97a/JaGrkzimageimage_NfWAJ-zjQz.png)

## HTML注释

HTML 注释以 **`<!--`** 开始，以 **`-->`** 结束，VSCode中使用**`Ctrl+/`** 快速添加注释

```html
<!--这是HTML的注释-->
```

# 文本标签

## 标题标签

从**`*<h1>*`**到**`*<h6>*`**元素定义HTML标题，**`*<h1>*`**定义最重要的标题，**`*<h6>*`**定义最不重要的标题。每页仅使用一个 **`*<h1>*`** - 这应该代表整个页面的主标题/主题。另外，不要跳过标题级别 - 从 **`*<h1>*`** 开始，然后使用 **`*<h2>*`** ，依此类推。

标题标签支持全局属性和事件属性。

```html
<body>  <h1>Heading 1</h1>  <h2>Heading 2</h2>  <h3>Heading 3</h3>  <h4>Heading 4</h4>  <h5>Heading 5</h5>  <h6>Heading 6</h6></body>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/c0141fa8-ac63-4b3c-aa48-ccc28750215b/JaGrkzimageimage_mciZv7AooJ.png)

## 段落和预格式化文本标签

**`*<p>*`** 元素定义一个段落，段落始终以新行开始，并且浏览器会自动在段落前后添加一个空行。**`*<p>*`** 元素**不会保存内容的空格和行**。

段落标签支持全局属性和事件属性。

```html
<body>  <p>    My Bonnie lies over the ocean.
    My Bonnie lies over the sea.
    My Bonnie lies over the ocean.
    Oh, bring back my Bonnie to me.
  </p></body>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/d85cd3e5-2686-4920-ba1f-c82ca4143592/JaGrkzimageimage_L5aGf8Syd4.png)

`<pre>` 元素定义预格式化文本，内容中的文本以固定宽度字体显示，并且文本**保留空格和换行符**。

```html
<body>  <pre>    My Bonnie lies over the ocean.
    My Bonnie lies over the sea.
  </pre>  <pre>    My Bonnie lies over the ocean.
    Oh, bring back my Bonnie to me.
  </pre></body>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/dcae2209-f8d8-4844-b0f2-3acd02916657/JaGrkzimageimage_FljFwwl0pW.png)

## 文本格式化标签

HTML定义具有**特殊含义**的文本的元素。

- **`*<b>*`**
    
    **粗体文本**。定义粗体文本，没有任何额外的重要性。
    
- **`*<strong>*`**
    
    **重要文本**。定义了非常重要的文本。里面的内容通常以粗体显示。
    
- **`*<i>*`**
    
    **斜体文本**。里面的内容通常以斜体显示，通常用于表示技术术语、另一种语言的短语、想法、船名等。
    
- **`*<em>*`**
    
    **强调文本**。定义强调文本，里面的内容通常以斜体显示，屏幕阅读器将重音强调元素内容中的文本。
    
- **`*<mark>*`**
    
    **标记文本**。定义应标记或突出显示的文本。
    
- **`*<small>*`**
    
    **较小的文本**。定义较小的文本。
    
- **`*<del>*`**
    
    **删除的文本**。定义已从文档中删除的文本。浏览器通常会在删除的文本中划一条线。
    
- **`*<ins>*`**
    
    **插入的文本**。定义已插入到文档中的文本。浏览器通常会给插入的文本添加下划线。
    
- **`*<sub>*`**
    
    **下标文本**。定义下标文本，下标文本显示在中线下方半个字符处，常用于数学公式。
    
- **`*<sup>*`**
    
    **上标文本**。定义上标文本，上标文本显示在中线上方半个字符处，常用于脚注。
    
- **`*<q>*`**
    
    **短引用**。定义一个简短的引用，浏览器通常在引文两边插入引号。
    
- **`*<blockquote>*`**
    
    **长引用**。定义引用自其他来源的一段内容。浏览器通常缩进元素内容。`cite`属性设置内容源。
    
- 代码示例
    
    ```html
    <body>  <b>This text is bold</b>  <strong>This text is important!</strong>  <i>This text is italic</i>  <em>This text is emphasized</em>  <small>This is some smaller text.</small>  <p>Do not forget to buy <mark>milk</mark> today.</p>  <p>My favorite color is <del>blue</del> red.</p>  <p>My favorite color is <del>blue</del> <ins>red</ins>.</p>  <p>This is <sub>subscripted</sub> text.</p>  <p>This is <sup>superscripted</sup> text.</p></body>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/c9a5fc11-0663-4b50-b1e6-24bd7ea271d7/JaGrkzimageimage_iS6LDC0E4O.png)
    

## 换行标签和水平线标签

**`*<br>*`** 元素定义换行符，如果只是想换行，而不是重新开始段落，可以使用换行标签。换行标签是一个空标签，所以没有结束标签。

**`*<hr>*`** 元素定义 HTML 页面中的主题分隔符，并且通常显示为水平线，用于分隔 HTML 页面中的内容。水平线标签也是一个空标签，所以没有结束标签。

```html
<body>  <p>This is<br>a paragraph<br>with line breaks.</p>  <h1>This is heading 1</h1>  <p>This is some text.</p>  <hr>  <h2>This is heading 2</h2>  <p>This is some other text.</p>  <hr></body>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/6e4d027c-98b4-41d3-a2ec-995edc41ad47/JaGrkzimageimage_vrQ1iahmqi.png)

# 链接与媒体

## 超链接标签

**`*<a>*`** 元素定义超链接，之所以叫超链接，是指在超文本中存在的链接，表示链接不一定是文本，可以是图像或者任何其它HTML元素。

`href` 属性**指示链接的目的地**，这个目的地可以是外部页面，也可以同一页面的书签处。对于链接外部页面可以使用**绝对URL**和**相对URL**。绝对URL是指文件的真正路径，是从硬盘或者项目的根目录出发，通过一级级目录指向文件，比如：`C:/Users/a1394/Desktop/OneDrive` 、`www.google.com/` 。相对URL是指从当前文件的位置出发指向目标文件，`../`表示当前文件所在的目录的上一级目录、`./` 表示当前文件所在的目录（可省略）、`/`表示当前站点的根目录(域名映射的硬盘目录)。对于很长的网页，可以使用**链接书签**，首先需要使用`id`属性创建书签，然后添加指向它的链接，单机链接时，页面将会滚动到书签处。当然链接也可以指向另一页面的书签。上面使用文本作为链接，但链接可以是任何HTML元素，只需要将HTML元素嵌入超链接元素中即可。在 `href` 属性内使用 `mailto:` 将创建一个用于打开**电子邮件**程序的链接。当使用**按钮作为链接**时，需要为其添加一个单击事件和一些JS代码。

```html
<body>  <h1>How to learning HTML</h1>  <h2>HTML Basic</h2>  <h2 id="c4">HTML Links</h2>  <!-- 链接同一页面的书签 -->  <a href="#c4">Jump to HTML Links</a>  <!-- 链接到不同页面的书签 -->  <a href="07-LinksTagMark.html#c1"> Jump to another page mark</a>  <!-- 使用图像作为链接 -->  <a href="www.google.com"><img src="<https://www.w3schools.com/html/smiley.gif>" alt="smiling face"></a>  <!-- 打开电子邮件的链接 -->  <a href="mailto:dongf7889@gmail.com">send email</a>  <!-- 使用按钮作为链接 -->  <button onclick="document.location='07-LinksTagMark.html'">Jump page</button></body>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/2c1bcddb-9a96-4e68-942e-563e8c257625/JaGrkzimageimage_sgUCKNpK2_.png)

---

`target` 属性**指定打开链接文档的位置**，默认情况下链接页面将显示在当前浏览器窗口中。

- `_self` ：默认，在同一窗口打开链接文档。
- `_blank` ：在新窗口打开链接文档。
- `_parent` ：在上一层框架或窗口中展示文档，没有就是当前页面。
- `_top` ：在最外层框架或窗口中展示文档，没有就是当前页面。
- `framename` ：如果这个指定名称或 id 的框架或者窗口不存在，浏览器将打开一个新的窗口，给这个窗口一个指定的标记，然后将新的文档载入那个窗口

```html
<body>  <h3>Table of Contents</h3>  <ul>    <li><a href="/example/html/pref.html" target="bodyFrame">Preface</a></li>    <li><a href="/example/html/chap1.html" target="view_window">Chapter 1</a></li>    <li><a href="/example/html/chap2.html" target="view_window">Chapter 2</a></li>    <li><a href="/example/html/chap3.html" target="view_window">Chapter 3</a></li>  </ul>  <iframe id="bodyFrame" name="bodyFrame" frameborder="0"></iframe></body>
```

---

`title` 属性**指定有关元素的额外信息**。当鼠标移动到元素上时，该信息通常显示为工具提示文本。

```html
<body>  <a href="wwww.google.com" title="谷歌搜索">谷歌</a></body>
```

## 图像标签

**`*<img>*`** 元素用于在网页中嵌入图像，本质上来说，图像并没插入网页，而是图像链接到网页，**`*<img>*`** 标签为引用的图像创建**保留空间**。**`*<img>*`** 是空元素，没有结束标签。

`src` 属性**指定图像的路径 (URL)**，如果浏览器找不到URL指向的图像，则会显示损坏的链接图标和`alt`文本。

`alt` 属性**为图像提供替代文本**，如果图像因为某种原因无法显示，则会显示`alt`属性提供的替代文本。

`style` 属性**可以来指定图像的宽度和高度**（需要指定单位），或者可以直接使用`width` 和`height` 来指定宽高（默认以像素px为单位），虽然`style` 和`width`、`height` 都可以指定图片宽高，但是建议使用`style` 属性，这样可以防止在样式表中更改图像大小而导致图像的改变。最好始终设置宽高，否则图像加载时，页面可能会闪烁。使用CSS的`float` 属性，可以**让图像浮动到文本左侧或者右侧**。

```html
<head>  <style>    img {
      width: 100%;    }
  </style></head><body>  <!--宽高的不同定义方式-->  <img src="html5.gif" alt="HTML5 Icon" width="128" height="128">  <img src="html5.gif" alt="HTML5 Icon" style="width:128px;height:128px;">  <!--图像浮动-->  <p><img src="smiley.gif" alt="Smiley face" style="float:right;width:42px;height:42px;">The image will float to the right of the text.</p>  <p><img src="smiley.gif" alt="Smiley face" style="float:left;width:42px;height:42px;">The image will float to the left of the text.</p></body>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/44b35126-4379-4c29-88b3-bc6680526658/JaGrkzimageimage_c62mE8PqO9.png)

---

`usemap` 属性**用来联系图像和图像映射之间的关系**。图像映射是指在图像上创建多个可点击区域，点击不同区域，跳转不同页面。`usemap` 值以哈希标记 `#` 开头，后跟图像映射的名称，用于创建图像和图像映射之间的关系，使用**`*<map>*`** 元素创建图像映射，并通过使用所需的 `name` 属性链接到图像，也就是说**`*<map>*`**元素的`name`属性要和**`*<img>*`**元素的`usemap`属性具有相同的值。然后添加可点击区域，这是由**`*<area>*`**元素定义的，而可点击区域的形状由**`*<area>*`**元素的`shape`属性定义，有如下几种属性值代表的预定义形状：

- `rect`：定义一个矩形区域。
- `circle`：定义一个圆形区域。
- `poly`：定义一个多边形区域。
- `default`：定义整个区域。

**`*<area>*`**元素的`coords`属性定义区域的边界点坐标，矩形需要左上和右下两个点坐标，圆形需要圆心坐标和半径，多边形需要每个顶点的坐标，坐标由代表距离左边距的X轴和代表距离上边距的Y轴所定义。

**`*<area>*`**元素的`href`属性则指向链接目的地。

```html
<body>  <img src="workplace.jpg" alt="Workplace" usemap="#workmap">  <map name="workmap">    <area shape="rect" coords="34,44,270,350" alt="Computer" href="computer.htm">    <area shape="rect" coords="290,172,333,250" alt="Phone" href="phone.htm">    <area shape="circle" coords="337,300,44" alt="Coffee" href="coffee.htm">  </map></body>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/64fddc5c-241c-4f4a-8d84-41ef89395e0a/JaGrkzimageimage_luugUDqjr1.png)

还可以将 `click` 事件添加到 **`*<area>*`** 元素以执行 JavaScript 函数。

```html
<body>  <map name="workmap">    <area shape="circle" coords="337,300,44" href="coffee.htm" onclick="myFunction()">  </map>  <script>    function myFunction() {
      alert("You clicked the coffee cup!");    }
  </script></body>
```

---

HTML元素添加**背景图片**时，使用HTML`style` 属性和CSS`background-image`属性。如果背景图像小于元素，图像将水平和垂直重复自身，直到到达元素的末尾，为了避免重复可以将`background-repeat` 属性设置为 `no-repeat`，如果要将背景图片覆盖整个元素，可以将 `background-size` 属性设置为 `cover`（图像不会拉伸，保持原始比例），如果要将背景拉伸以适合整个元素，可以将`background-size` 属性设置为 `100% 100%`，另外为了确保整个元素始终被覆盖，可以将`background-attachment` 属性设置为 `fixed`

```html
<style>body {
  background-image: url('img_girl.jpg');  background-repeat: no-repeat;  background-attachment: fixed;  background-size: cover;}
</style>
```

---

**`*<picture>*`** 元素**使不同屏幕尺寸显示不同的图片**。这个标签的意义在于适应不同尺寸屏幕，而且有些浏览器和设备不支持所有的图像格式，通过这个标签就可以添加所有格式的同一图像，确保所有浏览器和设备显示上的一致性。**`*<picture>*`**元素包含一个或多个**`*<source>*`**元素，每个元素通过`srcset`属性引用不同图像，通过`media`属性定义该图像适合的屏幕尺寸。浏览器会识别第一个符合条件的**`*<source>*`**元素，忽略以下元素，但为了保证图像的正确显示，应该始终将**`*<img>*`**元素作为**`*<picture>*`**元素的最后一个子元素，避免浏览器不支持**`*<picture>*`**或者在没有**`*<source>*`**元素匹配的情况下使用。

```html
<picture>  <source media="(min-width: 650px)" srcset="img_food.jpg">  <source media="(min-width: 465px)" srcset="img_car.jpg">  <img src="img_girl.jpg" alt="girl" style="width:auto;"></picture>
```

---

**添加网站图标**时，使用**`*<link>*`**元素，这个元素定义了当前文档和外部资源之间的关系。网站图标一般使用`.ico`格式，这是为了保证通用性，使用`rel`属性指定了当前文档和外部资源/连接文档的关系，使用`href`属性指定了外部资源的路径，使用`type`属性指定了外部资源的格式，网站图标一般命名为**`favicon.ico`**

```html
<head>  <title>My Page Title</title>  <link rel="icon" type="image/x-icon" href="/images/favicon.ico"></head><body><h1>This is a Heading</h1><p>This is a paragraph.</p></body>
```

`type`的属性表示图标为`.ico`格式，可以忽略，浏览器会根据`href`属性的路径找到文件进行判断。

## 音频与视频标签

HTML 标准仅支持 MP4、WebM 和 Ogg 视频，HTML 标准仅支持 MP3、WAV 和 Ogg 音频

**`*<video>*`** 元素**用于在网页上显示视频**。**`*<source>*`** 元素指定浏览器可以选择的替代视频文件，浏览器将使用第一个可以识别格式的视频。`controls`属性添加视频控件，例如播放、暂停和音量。`autoplay`属性使浏览器自动启动视频，不过Chromium 内核的浏览器大多数情况下不允许自动播放，但始终允许静音自动播放，可以在在 `autoplay` 属性之后添加 `muted` 属性，使视频静音自动播放。

```bash
<video width="320" height="240" controls autoplay muted>  <source src="movie.mp4" type="video/mp4">  <source src="movie.ogg" type="video/ogg">  Your browser does not support the video tag.
</video>
```

HTML DOM定义了**`*<video>*`**元素的方法、属性和时间，可以通过JS拿到**`*<video>*`**元素的属性，或者实现元素的方法。

---

**`*<audio>*`** 元素**用于在网页上播放音频**。**`*<source>*`** 元素指定浏览器可以选择的替代音频文件。浏览器将使用第一个可以识别格式的视频。`controls` 属性添加音频控件，例如播放、暂停和音量。`autoplay`属性使浏览器自动播放音频，不过Chromium 内核的浏览器大多数情况下不允许自动播放，但始终允许静音自动播放，可以在在 `autoplay` 属性之后添加 `muted` 属性，使静音静音自动播放。

```bash
<audio controls autoplay controls autoplay muted>  <source src="horse.ogg" type="audio/ogg">  <source src="horse.mp3" type="audio/mpeg">  Your browser does not support the audio element.
</audio>
```

HTML DOM定义了**`*<audio>*`**元素的方法、属性和时间，可以通过JS拿到**`*<audio>*`**元素的属性，或者实现元素的方法。

---

**`*<object>*`** 元素**定义 HTML 文档中的嵌入对象**。它旨在将插件（如 Java 小程序、PDF 阅读器和 Flash 播放器）嵌入网页中，但也可用于在 HTML 中包含 HTML或者媒体。事实上现在大多数浏览器已经不再支持Java Applet、ActiveX控件和Shockwave Flash，`data`属性指定嵌入对象的路径。

```bash
<object width="100%" height="500px" data="snippet.html"></object><object data="audi.jpeg"></object>
```

---

**`*<embed>*`** 元素**也定义 HTML 文档中的嵌入对象**。同样可用于在 HTML 中包含 HTML或者媒体，这是一个空标签，没有结束标签，`src`属性指定嵌入对象的路径。

```bash
<embed width="100%" height="500px" src="snippet.html"><embed src="audi.jpeg">
```

通常情况下，如果只需简单地嵌入单一的多媒体文件，可以考虑使用 **`*<embed>*`** 元素；而对于复杂的嵌入场景，需要更多灵活性和可访问性的情况下，则可以选择 **`*<object>*`** 元素。

---

**`*<iframe>*`** 元素**可以用来引用外部网站上的视频**。使用 `width` 和 `height` 属性指定播放器的尺寸，让 `src` 属性指向视频 URL，在视频网址后面添加 `autoplay=1` 和 `mute=1` ，让视频自动静音播放，添加 `loop=1`让视频循环播放，添加 `controls=1` 在视频播放器中显示控件。

```bash
<iframe width="420" height="315" src="<https://www.youtube.com/embed/tgbNymZ7vqY?autoplay=1&mute=1&controls=1>"></iframe><iframe width="420" height="315" src="<https://www.youtube.com/embed/tgbNymZ7vqY?playlist=tgbNymZ7vqY&loop=1>"></iframe>
```

## 嵌入其它内容的标签

**`*<iframe>*`** 元素**指定内联框架，用于在当前 HTML 文档中嵌入另一个文档**。`title` 属性用于对内嵌文档的描述，屏幕阅读器使用它来读出**`*<iframe>*`**元素的内容，`height` 和 `width` 属性指定**`*<iframe>*`**元素的大小，默认情况下，**`*<iframe>*`**元素周围有边框，要删除边框，可以添加 `style` 属性并使用 CSS `border` 属性，**`*<iframe>*`**元素还可以用作链接的目标框架，超链接的 `target` 属性必须引用**`*<iframe>*`**元素的 `name` 属性。

```bash
<iframe
  src="demo_iframe.htm"
  height="200"
  width="300"
  title="Iframe Example"  style="border:none;"></iframe><!--超链接到iframe--><iframe src="demo_iframe.htm" name="iframe_a" title="Iframe Example"></iframe><p><a href="<https://www.w3schools.com>" target="iframe_a">W3Schools.com</a></p>
```

---

**`*<object>*`** 标签**最初是为了嵌入浏览器插件而设计的**。插件包括java小程序、ActiveX 控件、Flash 影片等，但现在的浏览器已经不再支持这些插件，不过**`*<object>*`**还可以用来嵌入HTML、图像、视频等，但是建议使用专门的标签。**`*<object>*`**元素允许指定备用内容和参数，提供了更丰富的可访问性特性。`data`属性指定嵌入对象的路径`height` 和 `width` 属性指定**`*<object>*`**元素大小。

```bash
<object width="100%" height="500px" data="snippet.html"></object><object data="audi.jpeg"></object>
```

---

**`*<embed>*`** 元素**也定义 HTML 文档中的嵌入对象**。同样可用于在 HTML 中包含 HTML或者媒体，这是一个空标签，没有结束标签，`src`属性指定嵌入对象的路径。

```bash
<embed width="100%" height="500px" src="snippet.html"><embed src="audi.jpeg">
```

通常情况下，如果只需简单地嵌入单一的多媒体文件，可以考虑使用 **`*<embed>*`** 元素；而对于复杂的嵌入场景，需要更多灵活性和可访问性的情况下，则可以选择 **`*<object>*`** 元素。

# 列表与表格

## 有序列表标签

**`*<ol>*`**元素**定义有序列表（Ordered Lists）**，每个列表项均以 **`*<li>*`**标签开头。默认情况下，列表项将用数字顺序标记，列表项之间可以进行嵌套，**`*<li>*`**代表的列表项可以包含新列表和其他HTML元素。**`*<ol>*`** 元素的 `type` 属性定义列表项标记的类型：

- `type="1"`：以数字编号（默认）。
- `type="A"`：以大写字母标号。
- `type="a"`：以小写字母编号。
- `type="I"`：大写罗马数字编号。
- `type="i"`：小写罗马数字编号。

**`*<ol>*`**的`start`控制编号的起始数字，默认从1开始。

```html
<ol>
  <li>Coffee</li>
  <li>
    Tea
    <ol>
      <li>Black tea</li>
      <li>Green tea</li>
    </ol>
  </li>
  <li>Milk</li>
</ol>
```

HTML5中type属性已被废弃，推荐使用CSS的`list-style-type` 属性。

## 无序列表标签

**`*<ul>*`**元素**定义无序列表（Unordered Lists）**，每个列表项均以 **`*<li>*`**标签开头。默认情况下，列表项将用项目符号（小黑圆圈）标记，列表之间可以进行嵌套，**`*<li>*`**代表的列表项可以包含新列表和其他HTML元素。CSS `list-style-type` 属性用于定义列表项标记的样式：

- `disc`：项目符号（小黑圆点），默认。
- `circle`：空心圆形。
- `square`：实心正方形。
- `none`：不设置符号。

对于无序列表最为流行的一项使用是，水平设置列表样式，以**创建导航菜单**。

```html
<ul>
  <li>Coffee</li>
  <li>
    Tea
    <ul>
      <li>Black tea</li>
      <li>Green tea</li>
    </ul>
  </li>
  <li>Milk</li>
</ul>
```

HTML5中type属性已被废弃，推荐使用CSS的`list-style-type` 属性。

## 自定义列表标签

**`*<dl>*`** 元素**定义描述列表（Description Lists）**，**`*<dt>*`** 标签定义术语（名称），**`*<dd>*`** 标签描述每个术语。

```html
<dl>  <dt>Coffee</dt>  <dd>--black hot drink</dd>  <dt>Milk</dt>  <dd>--white cold drink</dd></dl>
```

## 表格标签

**`*<table>*`元素定义表格**，**`*<tr>*`**元素定义表格中的行，**`*<td>*`**元素定义每行的单元格，**`*<th>*`**定义表格的表头，内容将加粗居中显示，单元格中的内容可以任意元素。

```html
<table>
  <tr>
    <th>Emil</th>
    <th>Tobias</th>
    <th>Linus</th>
  </tr>
  <tr>
    <td>16</td>
    <td>14</td>
    <td>10</td>
  </tr>
</table>
```

---

默认情况下，边框情况样式为：

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/8542eba0-fd5a-4353-9e88-21be5fd44884/JaGrkzimageimage_yIrJ4lRzAo.png)

可以将CSS的`border` 属性添加在**`*<table>*`** 、 **`*<th>*`** 和 **`*<td>*`** 元素上使用：

```css
table, th, td {
  border: 1px solid black;}
```

上面为简写格式，分别代表`border-width`、`border-style`和`border-color`。

如果要将表格边框设置为**单边框**，可以将 CSS `border-collapse` 属性（独有属性）设置为 `collapse`：

```css
table, th, td {
  border: 1px solid black;  border-collapse: collapse;}
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/21c41d73-c26d-4952-b720-ce4a11430dc2/JaGrkzimageimage_Z8rELZrd_m.png)

CSS属性`empty-cells`用于控制空白单元格是否显示边框：hide（隐藏）、show（显示），前提是`border-collapse` 不能为“`collapse`”

可以给每个单元格设置背景色：

```css
table,
th,
td {
  border: 1px solid white;
  border-collapse: collapse;
}
th,
td {
  background-color: #96d4d4;
}
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/e4015592-4c70-47b1-9497-7817e9caa309/JaGrkzimageimage_NWts8ZUunW.png)

如果要将边框设置为圆角，可以设置CSS`border-radius` 属性：

```css
table, th, td {
  border: 1px solid black;  border-radius: 10px;}
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/03b4beec-d79b-48f4-9b5c-dbf64d04307a/JaGrkzimageimage_P2_XTh4DsK.png)

如果不需要整个表格外的圆角边框，可以将**`*table*`**标签取消选择。

`border-style` 属性，可以设置边框的外观样式：

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/c130c6a5-3780-44d1-8963-c50c33fca0df/JaGrkzimageimage_kt2ezd_3j_.png)

`border-color` 属性，可以设置边框的颜色。

---

使用`style` 属性和CSS的`width` 或 `height`属性指定表、行和列的大小。

对于表的大小可以在**`*<table>*`**标签上设置表格宽度：

```html
<table style="width:100%"></table>
```

这里的宽度百分比是相对于父元素的，也就是它的上一层元素。

默认情况下表格的实际宽度由它的内容决定，如果设置百分比宽度，如果实际宽度过大，则会超出父元素范围，如果设置绝对单位，也将不生效，如果想要表格的大小不被内容决定，则需要设置`table-layout` 的值为`fixed` ，默认情况下值为`auto` ，由浏览器自动计算，也可以继承（inherit）

对于行的大小可以在**`*<tr>*`**标签上设置高度：

```html
<table style="width:100%">
  <tr>
    <th>Firstname</th>
    <th>Lastname</th>
    <th>Age</th>
  </tr>
  <tr style="height:200px">
    <td>Jill</td>
    <td>Smith</td>
    <td>50</td>
  </tr>
</table>
```

对于列的大小可以在**`*<th>*`** 或 **`*<td>*`**标签上设置宽度：

```html
<table style="width:100%">  <tr>    <th style="width:70%">Firstname</th>    <th>Lastname</th>    <th>Age</th>  </tr>  <tr>    <td>Jill</td>    <td>Smith</td>    <td>50</td>  </tr></table>
```

---

表头由**`*<th>*`**元素定义，每个 **`*<th>*`** 元素代表一个表格单元格。对于水平表标题，在第一行中设置**`*<th>*`**元素，对于垂直表标题，在每行的第一个单元格中设置**`*<th>*`**元素。使用 **`*<caption>*`**元素添加表格标题。

```html
<table style="width:100%">
  <caption>
    Monthly savings
  </caption>
  <tr>
    <th>Month</th>
    <th>Savings</th>
  </tr>
  <tr>
    <td>January</td>
    <td>$100</td>
  </tr>
</table>
```

可以使用CSS属性`caption-side` 来指定表格标题的位置

```css
#example1 {
  caption-side: bottom;
}
```

---

使用CSS `padding`和`border-spacing`属性**设置表格的填充和间距**。表格填充是指单元格边缘和单元格内容之间的空间，默认情况下设置为0，也可以使用`padding-top`、`padding-bottom`、`padding-left`、`padding-right`单独设置单元格上下左右的填充。单元格间距是指每个单元格之间的空间，默认情况下，空间设置为2像素，更改时需要修改**`*<table>*`** 元素上的 CSS `border-spacing` 属性，并且单元格边框的CSS`border-collapse`属性不能设置为`collapse` ，设置为`separate`。

---

`colspan`和`rowspan`使单元格跨越多列和多行，属性值代表要跨越的列数和行数。设置`colspan`和`rowspan`时候需要调整单元格的个数，和跨越的单元格数相对应。

```html
<table>  <tr>    <th>Name</th>    <th>age</th>    <th>gender</th>    <th colspan="2">phone</th>  </tr>  <tr>    <th>张三</th>    <th rowspan="2">18</th>    <th>男</th>    <th>110</th>    <th>112</th>  </tr><tr>    <th>李四</th>    <th>女</th>    <th>113</th>    <th>114</th>  </tr></table>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/a6666f46-e8cd-4a60-9cae-1ece863ea4a9/JaGrkzimageimage_JOhbvD_27p.png)

---

### 表格样式

> nth-child 是 CSS 选择器中的一个伪类，用于选择元素的第 n 个子元素，n是个整数或者表达式，如 2n+1，表示选择奇数位置的子元素；或者使用关键词 odd 和 even，分别表示选择奇数位置和偶数位置的子元素，通过这个伪类可以给表格添加斑马样式。

**水平斑马纹**：

```css
tr:nth-child(even) {
  background-color: #D6EEEE;}
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/449d1e5f-95e2-46b0-baf5-5dbc971f9fcb/JaGrkzimageimage_ZkrSYfHeYv.png)

**垂直斑马纹**：

```css
td:nth-child(even), th:nth-child(even) {
  background-color: #D6EEEE;}
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/c770a49f-4b29-4b14-87a3-12e2ef2ea1d4/JaGrkzimageimage_ymNsGYbYsc.png)

**组合斑马纹**：

```css
tr:nth-child(even) {
  background-color: rgba(150, 212, 212, 0.4);}
th:nth-child(even),td:nth-child(even) {
  background-color: rgba(150, 212, 212, 0.4);}
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/8175f67a-10da-4a9e-a900-97744474b695/JaGrkzimageimage_LByDClFVxY.png)

如果使用透明颜色，将获得重叠效果，使用`rgba()`指定颜色透明度。

**水平分割线**：

```css
tr {
  border-bottom: 1px solid #ddd;}
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/10bdc004-04f7-49c5-a5e3-5e7fc154e5df/JaGrkzimageimage_eCkw0nPbQR.png)

**悬浮效果**：

```css
tr:hover {
  background-color: #D6EEEE;}
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/d13a25cf-9bc6-40c1-855c-3db569ca0463/JaGrkzimageimage_0216KxU3m-.png)

### 列组和行组

> 列祖和行组意思是将表的列和行分组，对于表格没有任何功能上的影响，但可以帮助增添CSS样式。

**`*<colgroup>*`**元素**用于设置表的特定列的样式**。**`*<colgroup>*`**元素为列组的容器，元素中每个**`*<col>*`**元素都是一个列组，多个列组根据前后顺序对表格列进行顺序分组。**`*<colgroup>*`** 标记必须是 **`*<table>*`** 元素的子元素，在表格的其它元素之前，但在 **`*<caption>*`** 元素之后。能在**`*<colgroup>*`**元素中使用的CSS非常有限：

- `width`：设置列的宽度。
- `visibility`：是否隐藏列，如果属性值为`collapse`表示隐藏列。
- `border`：列中单元格的边框样式。

```html
<table>
  <colgroup>
    <col span="2" style="background-color: #D6EEEE" />
    <col span="3" style="background-color: pink" />
  </colgroup>
  <tr>
    <th>MON</th>
    <th>TUE</th>
    <th>WED</th>
    <th>THU</th>
    ...
  </tr>
</table>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/38525d1e-bf0a-483a-8c9a-821cbc427e57/JaGrkzimageimage_eTwBBIOsnW.png)

---

**`*<thead>*`**、**`*<tbody>*`** 和 **`*<tfoot>*`** 三个元素将表格的行分为表头、表身和表尾。

```html
<head>
  <style>
    thead {color:green;}
    tbody {color:blue;}
    tfoot {color:red;}
    table, th, td {
      border: 1px solid black;}
  </style>
</head>
<body>
  <h1>The thead, tbody, and tfoot elements - Styled with CSS</h1>
  <table>
    <thead>
      <tr>
        <th>Month</th>
        <th>Savings</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>January</td>
        <td>$100</td>
      </tr>
      <tr>
        <td>February</td>
        <td>$80</td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td>Sum</td>
        <td>$180</td>
      </tr>
    </tfoot>
  </table>
</body>
```

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/17738a39-c8b3-46b7-900b-3ed1a486c4af/JaGrkzimageimage_FbkGte_oLk.png)

### 响应式表格

如果表格太长，屏幕无法显示完整内容，可以在元素周围添加一个带有`overflow-x:auto`的容器元素（如<div>）以使其响应：

```css
<div style="overflow-x:auto;">

<table>
... table content ...
</table>

</div>
```

# 表单与输入

## 表单标签

> HTML表单用于收集用户输入。用户输入通常被发送到服务器处理。

**`*<form>*`** 元素**用于创建用于用户输入的 HTML 表单，本身不可见，是不同类型输入元素的容器**，可以包含一个或多个[表单元素](https://www.wolai.com/mCWkR8jpFoj2afxj2Z6fpk#fW915DMpGJV1phLZASjvGZ)。

**`*<form>*`** 元素具有以下属性：

- `action`
    
    定义提交表单时要执行的操作，通常，当用户单击提交按钮时，表单数据会发送到服务器上的文件。如果省略该属性，则操作将设置为当前页面。
    
- `target`：
    
    指定在何处显示提交表单后收到的响应。和**`*<a>*`**元素中的`target`属性一样。
    
- `method`
    
    指定提交表单数据时要使用的 HTTP 方法。默认为`GET`方法。使用`GET`时表单数据以名称/值对的形式附加到 URL中，数据可见、不安全、URL长度有限。使用`POST`方法时表单数据将附加到HTTP请求体中，数据不可见、安全、没有大小限制，但是没有无法添加书签。
    
- `autocomplete`
    
    指定表单是否应打开或关闭自动完成功能，会有上次输入的记忆提示。
    
- `novalidate`
    
    是一个布尔属性，如果存在，它指定提交时不应验证表单数据（输入）。
    
- `enctype`
    
    指定提交表单数据时的编码方式，仅适用于`POST`方式。
    

```html
<form  action="/action_page.php"  target="_blank"  method="post"  autocomplete="on"  novalidate  enctype="multipart/form-data">...
</form>
```

## 表单元素

**`*<form>*`**元素是表单元素的容器，包含以下一个或多个表单元素：

- **`*<input>*`**
    
    取决于 `type` 属性，可以有多种显示方式，如：文本输入、密码输入、单选按钮、复选框、提交按钮、按钮等。会在[Input类型](https://www.wolai.com/mCWkR8jpFoj2afxj2Z6fpk#5WcB2fvi1LeGJRipjzbzxw)一节详细说明。
    
- **`*<label>*`**
    
    **定义多个表单元素的标签**。当用户聚焦于表单元素时，屏幕阅读器将会读出**`*<label>*`**元素的内容；当用户点击**`*<label>*`**元素内容时，会将屏幕聚焦到表单元素。**`*<label>*`** 元素的 `for` 属性应等于表单元素的 `id` 属性，以将它们绑定在一起。
    
    ```html
    <form>  <label for="fname">first form</label>  <input type="text" id="fname" name="fname"></form>
    ```
    
- **`*<select>*`&`*<option>*`&`*<optgroup>*`**
    
    **`*<select>*`元素定义一个下拉列表**。`size`属性指定可见值的数量，`multiple` 属性允许用户选择多个值。
    
    **`*<option>*`元素在下拉列表中定义选项**。布尔属性`selected`表示该选项为下拉列表的预选。
    
    **`*<optgroup>*`元素给下拉列表中的选项分组**。
    
    ```html
    <form action="">  <label for="cars">Choose a car:</label>  <select name="cars" id="cars" size="2" multiple>    <optgroup label="Swedish Cars">      <option value="volvo">Volvo</option>      <option value="saab">Saab</option>    </optgroup>    <optgroup label="German Cars">      <option value="mercedes" selected>Mercedes</option>      <option value="audi">Audi</option>    </optgroup>  </select>  <br><br>  <input type="submit" value="Submit"></form>
    ```
    
- **`*<textarea>*`**
    
    **定义一个多行输入字段（文本区域）**。`rows` 属性指定文本区域中的可见行数，`cols` 属性指定文本区域的可见宽度。
    
    ```html
    <form action="">  <label for="textarea">input text</label><br>  <textarea name="textarea" id="textarea " cols="30" rows="10">    input 1-99 number
      </textarea></form>
    ```
    
- **`*<button>*`**
    
    **定义了一个可点击的按钮**。最好始终为元素指定 `type` 属性，因为不同的浏览器可能对按钮元素使用不同的默认类型。
    
    ```html
    <form action="">  <button type="button" onclick="alert('Hello world')">Click me!</button></form>
    ```
    
- **`*<fieldset>*`&`*<legend>*`**
    
    **`*<fieldset>*`** 元素**用于对表单中的相关数据进行分组**，**`*<legend>*`** 元素**定义** **`*<fieldset>*`** **元素的标题**。
    
    ```html
    <form action="">  <fieldset>    <legend>Your Info:</legend>    <label for="name">name:</label><br>    <input type="text" id="name" name="name"><br>    <label for="age">age:</label><br>    <input type="text" id="age" name="age"><br>    <input type="submit" value="submit">  </fieldset></form>
    ```
    
    效果是这样的：
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/28c4f111-1c1e-4e7a-89a4-0ee564917fa5/JaGrkzimageimage_fmh37OIaKr.png)
    
- **`*<datalist>*`**
    
    指定 **`*<input>*`** 元素的预定义选项列表。用户输入数据时将看到预定义选项的下拉列表。**`*<input>*`** 元素的 `list` 属性必须引用 **`*<datalist>*`** 元素的 `id` 属性，**`*<input>*`**元素中不能存在`id`属性。子元素**`*<option>*`**可以是空标签，建议使用空标签形式。
    
    ```html
    <form action="">  <label for="addr">Your address:</label><br>  <input type="text" name="addr" list="addr">  <datalist id="addr">    <option value="lanzhou">兰州</option>    <option value="南京">    <option value="成都"></form>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/a5958cba-1ada-4610-abbe-ec5abae82754/JaGrkzimageimage_YBfhfcYPDI.png)
    
- **`*<output>*`**
    
    **在事件或者函数执行计算后，结果将展示在元素中。**
    
    ```html
    <form action="" oninput="x.value=parseInt(a.value)+parseInt(b.value)">  0
      <input type="range" name="a" id="a" value="50">  100+
      <input type="number" name="b" id="b" value="50">  =
      <output name="x" for="a b"></output></form>
    ```
    
    `oninput`是一个事件属性，适用于任何元素，表示当**`*<input>*`** 或 **`*<textarea>*`** 元素的**值更改**时，将触发该属性。**`*<output>*`**的`for`属性表示影响计算结果元素的id，可以有多个。
    

## Input属性

- `type`
    
    决定**`*<input>*`**元素的显示方式。具体介绍在[Input类型](https://www.wolai.com/mCWkR8jpFoj2afxj2Z6fpk#5WcB2fvi1LeGJRipjzbzxw)一节。
    
- `name`
    
    必须要有`name`属性，否则无法发送用户输入的数据。
    
- `value`
    
    指定字段的初始值。
    
- `checked`
    
    该属性是个布尔属性，类似于**`*<option>*`**元素的`selected`属性，表示页面预先选中的**`*<input>*`**元素，常用在复选框（`type="checkbox"`）和单选框（`type="radio"`）中。
    
- `readonly`
    
    指定输入字段是只读的，用户无法修改，可以单击，提交表单时也会发送只读字段的值。
    
- `disabled`
    
    指定输入字段是禁用的，用户无法修改，不能单击，提交表单时不会发送禁用字段的值。
    
- `size`
    
    指定输入字段的可见宽度（以字符为单位），默认值为20。
    
- `maxlength`
    
    指定输入字段中允许的最大字符数，超过时只会显示最大字符数，且没有提示。
    
- `min`* &**`max`
    
    指定输入字段的最小值和最大值，适用于`number`、`range`、`date`、`datetime-local`, `month`, `time` and `week`。
    
- `multiple`
    
    允许用户在输入字段输入多个值，适用于`emil`和`file`。
    
- `pattern`
    
    指定一个正则表达式，提交表单时将根据正则表达式检查输入字段的值。适用于`text`, `date`, `search`, `url`, `tel`, `email`, and `password`。
    
- `placeholder`
    
    指定输入字段规则的简短描述，将会显示在输入字段中，适用于`text`, `search`, `url`, `number`, `tel`, `email`, and `password`。
    
- `required`
    
    指定在提交表单前必须要输入的字段，如果为空提交时将会提示。
    
- `step`
    
    指定输入字段的数字间隔，理解为步长。
    
- `autofocus`
    
    指定输入字段在页面加载时，自动获取焦点。
    
- `height` **&** `width`
    
    指定`type`属性为`image`时元素的高度和宽度。始终指定宽高，这样浏览器就会在加载页面是保留所需空间，否则浏览器无法保留适合的空间，图片加载时将会发生变化。
    
- `list`
    
    用于绑定**`*<datalist>*`**元素的`id`，将为**`*<input>*`**元素提供预定义选项。
    
- `autocomplete`
    
    指定输入字段是否应打开或关闭自动完成功能，会有上次输入的记忆提示。**`*<form>*`**元素中也有此属性。
    
- `form`
    
    指定 **`*<input>*`** 元素所属的表单，绑定表单元素的`id`，使输入字段处于表单外部但仍是表单一部分。
    
- `formaction`
    
    常用于`submit`and `image`，将指定提交表单数据时处理程序的URL，将会覆盖**`*<form>*`** 元素的 `action` 属性。
    
- `formenctype`
    
    常用于`submit`and `image`，指定提交表单数据时如何编码（仅适用于`POST`方法），将会覆盖**`*<form>*`** 元素的 `enctype`属性。
    
- `formmethod`
    
    常用于`submit`and `image`，指定将表单数据发送到处理程序URL的HTTP方法，将会覆盖**`*<form>*`** 元素的 `method` 属性。
    
- `formtarget`
    
    常用于`submit`and `image`，指定在何处显示提交表单后收到的响应，将会覆盖 **`*<form>*`** 元素的 `target` 属性。
    
- `formnovalidate`
    
    常用于`submit`，指定提交表单数据时不进行验证，将会覆盖**`*<form>*`** 元素的 `novalidate` 属性，不同的是该属性不是布尔值，属性值等于该属性。
    

## Input类型

**`*<input>*`**元素根据`type`属性值的不同会有多种显示方式：

- `text`
    
    定义单行**文本输入**字段，`type` 属性的默认值。
    
- `button`
    
    定义一个**按钮**，用于用户点击执行事件或者函数。
    
- `checkbox`
    
    定义一个**复选框**。
    
- `color`
    
    定义一个**颜色选择器**。
    
- `date`
    
    定义一个**日期选择器**（年月日），可以使用 `min` 和 `max` 属性添加对日期的限制
    
- `datetime-local`
    
    定义一个**日期和时间选择器**，不包含时区。
    
- `email`
    
    定义**电子邮件**的输入字段，在提交时可以自动验证格式。
    
- `file`
    
    定义**文件选择和上传**的按钮。
    
- `hidden`
    
    定义**隐藏**的输入字段，用户不可见，但会在源码中显示，用于向服务器发送额外信息。
    
- `image`
    
    将**图像**定义为提交按钮，包含`src`、`alt`、`width`、`height`等属性。
    
- `month`
    
    定义一个月份选择器（年月）。
    
- `number`
    
    定义**数字**输入字段，可以使用 `min` 和 `max` 属性添加对数字的限制。
    
- `password`
    
    定义**密码**输入字段，密码将不以明文形式展示（星号或者圆圈）。
    
- `radio`
    
    定义一个**单选按钮**。
    
- `range`
    
    定义一个用于**输入数字的控件**，默认范围是 0 到 100，可以使用 `min` 、 `max` 和 `step` 属性对接收的数字进行限制。
    
- `reset`
    
    定义**重置**按钮，将所有表单值重置为默认值。
    
- `search`
    
    定义**搜索**输入字段，类似于文本输入字段。
    
- `submit`
    
    定义向表单处理程序**提交表单数据的按钮**，`value`属性值为按钮名称。
    
- `tel`
    
    定义**电话号码**的输入字段。
    
- `time`
    
    定义一个**时间选择器**（时分）。
    
- `url`
    
    定义**URL**地址的输入字段，提交时可以自动验证 URL字段。
    
- `week`
    
    定义一个**周数选择器**（年周）。
    

# 语义化标签和结构

## 语义化HTML的重要性

语义化HMTL是指使用合适的标签来正确地描述内容的结构和含义，其重要性主要有：

1. **提升可访问性：** 使用语义化的 HTML 结构可以提升网站的可访问性，使得页面内容对于残障用户和使用辅助技术（如屏幕阅读器）的用户更加友好。通过正确地使用语义化标签（如 **`*<header>*`**、**`*<nav>*`**、**`*<main>*`**、**`*<article>*`**、**`*<section>*`**、**`*<aside>*`**、**`*<footer>*`** 等），可以更好地组织页面内容，让用户能够更容易地理解页面结构和内容。
2. **优化搜索引擎排名：** 搜索引擎（如 Google、Bing 等）在抓取和索引网页时会分析页面的结构和内容，从而确定页面的相关性和排名。使用语义化的 HTML 结构可以帮助搜索引擎更好地理解页面内容，提升页面的搜索排名。
3. **增强可维护性：** 语义化的 HTML 结构能够使页面更易于阅读和理解，有助于开发者快速地理解页面的结构和内容，提高代码的可维护性。此外，语义化的 HTML 结构也使得代码更易于扩展和修改，降低了后续维护的成本。
4. **增强代码的可读性和可预测性：** 使用语义化的 HTML 结构可以使代码更具有可读性和可预测性，使得开发者能够更快地理解代码的意图和结构，降低了出错的概率，并且使得团队成员之间更容易进行沟通和合作。
5. **提升用户体验：** 语义化的 HTML 结构能够使页面更具有逻辑性和条理性，提升用户对页面内容的理解和感知，从而提升用户体验。清晰的页面结构和内容展示方式有助于用户更快地找到他们需要的信息，提高用户的满意度和留存率。

## 结构标签

结构标签是HTML5中新引入的一些标签，用于更加语义化的描述页面的结构和内容。

- **`*<header>*`**
    
    定义页面或章节的页眉，包含导航、网站标题和搜索框等。一个HTML文档中可以有多个**`*<header>*`**元素，但**`*<header>*`** 不能放置在*** ***_**`<footer>`**_ 、 **`*<address>*`** 或另一个 **`*<header>*`** 元素内。
    
    ```html
    <article>  <header>    <h1>What Does WWF Do?</h1>    <p>WWF's mission:</p>  </header>  <p>WWF's mission is to stop the degradation of our planet's natural environment,
      and build a future in which humans live in harmony with nature.</p></article>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/387a59b0-de70-4e80-85f6-2c556e997bad/JaGrkzimageimage_XElYMga6Ha.png)
    
- **`*<nav>*`**
    
    定义页面导航部分，通常放置在页眉或者页脚中。
    
    ```html
    <nav>  <a href="/html/">HTML</a> |
      <a href="/css/">CSS</a> |
      <a href="/js/">JavaScript</a> |
      <a href="/jquery/">jQuery</a></nav>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/9517dfc5-7ef3-4f89-a7f4-d1d6f7d75aff/JaGrkzimageimage_jet-1eBcF5.png)
    
- **`*<section>*`**
    
    定义文档中的一个部分。网页文档通常可以分为介绍部分、内容部分和联系信息部分。**`*<section>*`**元素可以包含**`*<article>*`**元素。
    
    ```html
    <section><h1>WWF</h1><p>The World Wide Fund for Nature (WWF) is an international organization working on issues regarding the conservation, research and restoration of the environment, formerly named the World Wildlife Fund. WWF was founded in 1961.</p></section><section><h1>WWF's Panda symbol</h1><p>The Panda has become the symbol of WWF. The well-known panda logo of WWF originated from a panda named Chi Chi that was transferred from the Beijing Zoo to the London Zoo in the same year of the establishment of WWF.</p></section>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/9e3ba620-5bda-4442-a9ab-61613ffe4cde/JaGrkzimageimage_C18oDkIBQR.png)
    
- **`*<article>*`**
    
    定义独立的、自包含的文档内容，每个文档都是独立的。**`*<article>*`**元素可以包含**`*<section>*`**元素。
    
    ```html
    <article><h2>Google Chrome</h2><p>Google Chrome is a web browser developed by Google, released in 2008. Chrome is the world's most popular web browser today!</p></article><article><h2>Mozilla Firefox</h2><p>Mozilla Firefox is an open-source web browser developed by Mozilla. Firefox has been the second most popular web browser since January, 2018.</p></article><article><h2>Microsoft Edge</h2><p>Microsoft Edge is a web browser developed by Microsoft, released in 2015. Microsoft Edge replaced Internet Explorer.</p></article>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/0b78fa44-fda8-44c4-b2ee-41298a703b54/JaGrkzimageimage_hONbotdLXs.png)
    
- **`*<aside>*`**
    
    定义内容之外的一些内容（如侧边栏），内容应该于周围内容间接相关。
    
    ```html
    <p>My family and I visited The Epcot center this summer. The weather was nice, and Epcot was amazing! I had a great summer together with my family!</p><aside><h4>Epcot Center</h4><p>Epcot is a theme park at Walt Disney World Resort featuring exciting attractions, international pavilions, award-winning fireworks and seasonal special events.</p></aside>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/efa69959-a470-47c9-adee-d07518396115/JaGrkzimageimage_697jBbn9e6.png)
    
- **`*<footer>*`**
    
    定义页面或部分的页脚，包含版权信息、联系方式、相关链接等。一个HTML文档中可以有多个**`*<footer>*`**元素，用于展示不同的页脚内容。
    
    ```html
    <footer>  <p>Author: Hege Refsnes</p>  <p><a href="mailto:hege@example.com">hege@example.com</a></p></footer>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/1e5b3b2a-9bb1-41be-83e4-b6c3e218201a/JaGrkzimageimage_a1cR5154P3.png)
    
- **`*<figure>*`&`*<figcaption>*`**
    
    **`*<figure>*`**元素指定独立的内容，例如插图、图表、照片、代码列表等。**`*<figcaption>*`**元素定义**`*<figcaption>*`**元素的标题，可以放在**`*<figure>*`**元素的第一个或者最后一个子元素。
    
    ```html
    <figure>  <img src="pic_trulli.jpg" alt="Trulli">  <figcaption>Fig1. - Trulli, Puglia, Italy.</figcaption></figure>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/3b27fd00-3f98-44ad-a469-30d55d353ae2/JaGrkzimageimage_cYuDiKzONp.png)
    
- **`*<details>*`&`*<summary>*`**
    
    **`*<details>*`**定义用户可以根据需要打开和关闭的附加详细信息，类似于折叠文档。**`*<summary>*`**定义 **`*<details>*`** 元素的标题。
    
    ```html
    <details>  <summary>Epcot Center</summary>  <p>Epcot is a theme park at Walt Disney World Resort featuring exciting attractions, international pavilions, award-winning fireworks and seasonal special events.</p></details>
    ```
    
    ![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/dcecb259-1390-48e0-a6fc-9fd0f1df0cef/JaGrkzimageimage_pSa2rtpOUk.png)
    
- **`*<main>*`**
    
    定义页面的主要内容，包含页面的核心区域，一个页面应该只有一个 **`*<main>*`** 元素，不得是**`*<article>*`**、**`*<aside>*`**、**`*<footer>*`**、**`*<header>*`**或**`*<nav>*`**元素的子元素。
    
- **`*<time>*`**
    
    定义特定时间（或日期时间），通过`datetime`属性将时间转变为机器可读格式，以获得更好的搜索引擎结果或自定义功能（如提醒）。没有任何样式上的特殊。
    
    ```html
    <p>Open from <time>10:00</time> to <time>21:00</time> every weekday.</p>
    ```
    

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/4c39ec52-aae0-4f90-a2c0-0dfef735319a/JaGrkzimageimage_1dyrMjps21.png)

## 块级元素和内联元素

每个HTML元素都有一个默认的显示方式，主要取决于元素类型。两个最常见的显示方式是块和内联。

块级元素在页面上以块的形式显示，占据可用的全部宽度，每个块级元素总是从新行开始，浏览器会自动在元素前后添加一些边距，保持和前后元素的距离。块级元素包含块级元素和内联元素。块级元素的主要作用有：创建布局、分组内容和设置样式。

- **常见的块级元素** | [<address>](https://www.w3schools.com/tags/tag_address.asp) | <article> | <aside> | <blockquote> | <canvas> | <dd> | | ———————————————————————— | ———- | ———– | ————- | ——————————————————————————— | ——— | | <div> | <dl> | <dt> | <fieldset> | [<figcaption>](https://www.w3schools.com/tags/tag_figcaption.asp) | <figure> | | <footer> | <form> | <h1>-<h6> | <header> | <hr> | <li> | | <main> | <nav> | <noscript> | <ol> | <p> | <pre> | | <section> | <table> | <tfoot> | <ul> | <video> | |

内联元素在页面上以行的形式显示，元素的大小由其包含的内容决定，每个内联元素不会从新行开始，会在同一行上按顺序排列。内联元素包含内联元素和文本标签。内联元素的主要作用有：文本修饰、创建链接、插入图片和设置样式。

- **常见的内联元素** | <a> | [<abbr>](https://www.w3schools.com/tags/tag_abbr.asp) | <acronym> | <b> | <bdo> | <big> | <br> | | —————————————————————— | ——————————————————————— | ———- | ——– | ——— | —————————————————— | —— | | <button> | <cite> | <code> | <dfn> | <em> | [<i>](https://www.w3schools.com/tags/tag_i.asp) | <img> | | [<input>](https://www.w3schools.com/tags/tag_input.asp) | <kbd> | <label> | <map> | <object> | <output> | <q> | | <samp> | [<script>](https://www.w3schools.com/tags/tag_script.asp) | <select> | <small> | <span> | <strong> | <sub> | | <sup> | <textarea> | <time> | <tt> | <var> | | |

最常用的块级元素和内联元素是<div>和<span>这两个标签分别定义了一个块和内联，但没有特殊语义，实际开发中应该根据实际情况使用具有合适语义的标签，对于自定义功能和样式，可以使用这两个标签。

# 样式与布局

## CSS简介

CSS（层叠样式表）用于格式化网页，使用CSS可以控制颜色、字体、文本大小、元素之间的间距、元素的定位和布局方式、要使用的背景图像或背景颜色、不同设备和屏幕尺寸的不同显示等等等等。如果HTML是一只光溜溜的鸟，那CSS就是它的羽毛。

## 内联样式和外部样式表

在HTML中使用CSS样式主要有三种方式：

- 内联：在HTML元素内使用style属性。
    
    ```html
    <h1 style="color:blue;">A Blue Heading</h1>
    ```
    
- 内部：在<header>元素内使用<style>元素。
    
    ```html
    <head><style>  body {background-color: powderblue;}
      h1 {color: blue;}
      p {color: red;}
    </style></head>
    ```
    
- 外部：通过<link>元素链接到外部CSS文件，可以是另外网站的CSS文件。
    
    ```html
    <head>  <link rel="stylesheet" href="styles.css">  <link rel="stylesheet" href="<https://www.w3schools.com/html/styles.css>"></head>
    ```
    

## CSS盒模型

CSS盒模型是指在网页布局中，每个HTML元素都可以看作一个矩形的盒子，这个盒子包含了元素的内容、内边距、边框和外边距。CSS盒模型对于网页布局和样式设计具有重要意义，它定义了元素在页面上所占的空间和排列方式。

![](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/bb2a6941-064b-472d-bd10-5445ee368c31/JaGrkzimageimage_HsJr-FbYJv.png)

- **Margin(外边距)** - 除了边框外的区域，外边距是透明的，用于控制元素与其他元素之间的距离。
- **Border(边框)** - 围绕在内边距和内容外的线条，用于给元素添加边框效果。
- **Padding(内边距)** - 内容区域与边框之间的空间，内边距是透明的，用于控制内容与边框之间的间距。
- **Content(内容)** - 元素内部包含的实际内容，如文本、图像、子元素等。

## 布局

有四种方式创建多列布局：

- CSS框架：如bootstrap
- 浮动布局：利用CSS的float属性
- flexbox（弹性盒）布局：根据屏幕尺寸显示内容
- grid（网格）布局：基于网格中的行和列布局

这些内容在CSS中展示。

# 高级标签和功能

## 文档引用标签

## 元数据标签

## 图像地图标签

## HTML5新增标签和功能

# 响应式设计与移动原理

## 响应式设计原理

响应式设计的原理是基于弹性网格布局、弹性图片和媒体查询等技术，以及流体布局、弹性图像和媒体查询等技术，根据用户设备的特性和屏幕尺寸，动态调整网站的布局、内容和样式，以确保在不同设备上都能提供良好的用户体验。以下是响应式设计的原理及其核心技术：

1. **弹性网格布局（Flexible Grid Layout）：**
    - 响应式设计使用弹性网格布局来创建网页布局，使得网页中的各个元素能够根据设备屏幕的宽度自动调整大小和位置。这样可以确保网页在不同屏幕尺寸下都能保持良好的比例和平衡。
2. **弹性图片（Flexible Images）：**
    - 使用弹性图片技术可以使得图片根据设备屏幕的大小自动调整大小，以适应不同屏幕分辨率和像素密度。这样可以确保图片在不同设备上显示清晰且不失真。
3. **媒体查询（Media Queries）：**
    - 媒体查询是响应式设计的关键技术之一，它允许根据用户设备的特性（如屏幕宽度、分辨率、方向等）应用不同的 CSS 样式。通过媒体查询，可以为不同的屏幕尺寸和设备类型提供不同的布局和样式，从而实现响应式的设计效果。
4. **流体布局（Fluid Layout）：**
    - 响应式设计采用流体布局来创建网页布局，使得网页中的元素可以根据视口（Viewport）的大小动态调整大小和位置。这样可以确保网页在不同屏幕尺寸下都能够自适应并充分利用可用空间。
5. **断点（Breakpoints）：**
    - 响应式设计通常会在特定的屏幕宽度或设备特性上设置断点（Breakpoints），在这些断点处应用不同的 CSS 样式，以实现网页布局和样式的切换。通过断点的设置，可以确保网页在不同屏幕尺寸下具有不同的布局和外观。
6. **渐进增强（Progressive Enhancement）：**
    - 响应式设计遵循渐进增强的原则，即从基本的功能和布局开始，逐步添加更丰富的样式和交互效果。这样可以确保网页在不支持响应式设计的旧设备或浏览器上仍能够正常显示和使用。

## 移动优化的基本技巧

移动优化是指针对移动设备（如智能手机和平板电脑）进行网站或应用程序的设计和开发，以确保在移动设备上获得良好的用户体验和性能。以下是一些移动优化的基本技巧：

1. **响应式设计（Responsive Design）：**
    - 使用响应式设计技术来创建适应不同屏幕尺寸的网站布局。这可以通过 CSS 媒体查询和流式布局来实现，确保网站在各种移动设备上都能正常显示，并提供良好的用户体验。
2. **移动友好的用户界面（Mobile-Friendly UI）：**
    - 设计简洁、易用的用户界面，适应触摸操作和小屏幕操作。使用大型按钮和易于点击的链接，以及最少的输入字段，简化用户与网站或应用的交互。
3. **优化图像和多媒体内容：**
    - 对图像进行压缩和优化，以减少页面加载时间和数据传输量。使用适当的图像格式（如 WebP、JPEG）和分辨率，并为移动设备提供适当大小的图像。
    - 尽量减少或延迟加载大型多媒体内容（如视频和音频），以避免过长的加载时间和数据消耗。
4. **快速加载速度（Fast Loading Speed）：**
    - 优化页面加载速度，减少页面的大小和请求次数，通过减少 HTTP 请求、使用浏览器缓存和压缩资源等技术来加快页面加载速度。
    - 使用图片懒加载技术，只在用户滚动到图片位置时加载可见区域内的图片，以减少初始加载时间。
5. **移动优化的内容布局（Mobile-Optimized Content Layout）：**
    - 确保内容能够适应移动设备的屏幕尺寸，避免水平滚动和缩放操作。优化页面的内容布局和排列顺序，以确保重要内容能够优先展示并容易访问。
6. **使用响应式图标和字体（Responsive Icons and Fonts）：**
    - 使用矢量图标和字体图标，以确保在不同屏幕分辨率下都能清晰显示。避免使用固定大小的图标和字体，而是使用相对大小和百分比来保持适应性。
7. **优化交互体验（Optimized Interaction Experience）：**
    - 简化用户操作，减少输入和页面跳转次数。尽量避免使用小型的链接和按钮，确保用户能够轻松点击并进行交互。
    - 提供与移动设备特性相匹配的交互体验，如通过使用 HTML5 的触摸事件和手势来实现更自然的用户交互。
8. **测试和优化（Testing and Optimization）：**
    - 在不同的移动设备和浏览器上进行测试，确保网站在各种环境下都能正常运行和显示。使用移动设备模拟器和调试工具进行测试和调整，及时发现并解决问题。

预加载、压缩

## 媒体查询

媒体查询（Media Queries）是一种 CSS 技术，用于根据设备特性（如屏幕尺寸、屏幕方向、分辨率、颜色能力等）来应用不同的样式规则，从而实现响应式设计。通过媒体查询，可以根据不同的设备和浏览器环境，为用户提供最佳的视觉和交互体验。

### 基本语法：

媒体查询使用 `@media` 关键字来定义，其基本语法如下：

```css
@media media-type and (media-feature) {
    /* CSS rules to apply */}
```

- `media-type` 指定了要应用样式的媒体类型，常见的媒体类型包括 `all`（所有媒体设备）、`screen`（屏幕设备，如电脑、平板、智能手机等）、`print`（打印设备）等。
- `media-feature` 指定了一个或多个媒体特性，如 `width`（宽度）、`max-width`（最大宽度）、`orientation`（方向）等。
- `CSS rules to apply` 是要应用的样式规则，只有在媒体查询条件满足时才会应用这些样式。

### 示例：

```css
/* 当屏幕宽度小于等于 600px 时应用样式 */@media screen and (max-width: 600px) {
    body {
        font-size: 14px;    }
}
```

### 常用媒体特性：

- `width`：设备宽度。
- `height`：设备高度。
- `min-width`、`max-width`：最小或最大设备宽度。
- `min-height`、`max-height`：最小或最大设备高度。
- `orientation`：设备方向（`portrait` 竖屏、`landscape` 横屏）。
- `aspect-ratio`：设备宽高比。
- `color`：设备色彩能力。
- `resolution`：设备分辨率。

### 多重媒体查询：

可以在一个 `@media` 规则中同时指定多个媒体查询条件，使用逻辑运算符 `and`、`not` 和 `only`。

```css
/* 当屏幕宽度小于等于 600px 且为横屏时应用样式 */@media screen and (max-width: 600px) and (orientation: landscape) {
    /* CSS rules to apply */}
```

### 响应式设计中的应用：

媒体查询是响应式设计的重要组成部分，通过媒体查询可以根据设备特性为不同的屏幕尺寸和设备类型提供不同的布局和样式。常见的应用包括：

- 调整网页布局和排列顺序，以适应不同尺寸的屏幕。
- 提供不同大小和分辨率的图像和多媒体内容。
- 调整字体大小和行距，以提高可读性。
- 隐藏或显示特定的元素或功能，以优化用户体验。

# SEO与语义化

## 搜索引擎优化基础

搜索引擎优化（SEO）是一种通过优化网站内容和结构，以提高网站在搜索引擎中的排名，从而增加网站流量和可见性的技术和策略。以下是搜索引擎优化的基础知识：

1. **关键词研究（Keyword Research）：**
    - 关键词是用户在搜索引擎中输入的词语或短语，因此了解用户搜索的关键词和短语非常重要。通过关键词研究，可以确定与网站主题相关且具有搜索量的关键词，以在网站内容中进行优化。
2. **内容优化（Content Optimization）：**
    - 在网站上创建高质量、有用且相关的内容是搜索引擎优化的核心。确保网站内容包含目标关键词，并提供有价值的信息给用户。优化标题、描述和标题标签，以及使用相关的头部和段落标签来组织内容。
3. **网站结构优化（Website Structure Optimization）：**
    - 优化网站结构有助于搜索引擎了解网站内容的层次结构和重要性。使用清晰的URL结构，创建易于导航的网站布局，设置内部链接，以提高页面之间的关联性。
4. **技术优化（Technical Optimization）：**
    - 确保网站能够被搜索引擎顺利访问和索引，提高网站的加载速度，优化页面代码，确保网站在移动设备上的兼容性，使用适当的标记语言和结构化数据等技术优化措施。
5. **外部链接（Backlinks）：**
    - 外部链接是其他网站指向你网站的链接，也称为反向链接。搜索引擎认为拥有高质量的外部链接是网站权威性和可信度的体现，因此积极获取有价值的外部链接是提高搜索排名的重要策略。
6. **用户体验优化（User Experience Optimization）：**
    - 优化用户体验有助于提高网站的跳出率、停留时间和页面浏览量，这些指标对搜索引擎排名有重要影响。通过提供清晰的导航、良好的页面布局和易于阅读的内容等方式来改善用户体验。
7. **监测和分析（Monitoring and Analysis）：**
    - 定期监测和分析网站的流量、排名和转化率等指标，以了解搜索引擎优化的效果，并根据数据调整和优化策略。

## 语义化HTML对SEO的影响

语义化 HTML 是指使用恰当的 HTML 标签来描述网页的内容结构和含义，而不仅仅是为了呈现样式。语义化 HTML 对 SEO（搜索引擎优化）有着重要的影响，主要体现在以下几个方面：

1. **搜索引擎理解网页内容：**
    - 搜索引擎会根据网页的 HTML 结构来理解网页的内容和主题。语义化 HTML 可以提供清晰的信息层次结构，让搜索引擎更容易抓取和解读网页内容，从而更准确地确定网页的主题和相关性。
2. **关键词权重和密度：**
    - 使用语义化 HTML 可以使关键词自然地出现在合适的标签中，而不是通过滥用 `<div>` 和 `<span>` 标签来强行添加关键词。搜索引擎更倾向于识别并赋予权重给包含关键词的语义化标签，从而提高网页在相关搜索结果中的排名。
3. **提升页面可访问性：**
    - 语义化 HTML 使得网页更具有结构性和可读性，这不仅有助于搜索引擎理解网页内容，也使得网页更易于被残障人士和屏幕阅读器等辅助工具解读和访问。提升页面可访问性有助于提高用户体验，从而间接地提高了网站的排名。
4. **丰富片段（Rich Snippets）显示：**
    - 语义化 HTML 可以为搜索引擎提供更多关于网页内容的信息，使得搜索引擎能够更精确地生成丰富片段（Rich Snippets），如星级评价、产品价格等，从而提高网页在搜索结果中的可见性和吸引力。
5. **良好的用户体验：**
    - 语义化 HTML 可以使网页更具有逻辑性和可读性，提高了用户的浏览体验。搜索引擎越来越注重用户体验作为排名因素，因此语义化 HTML 间接地对 SEO 产生了积极影响。

# Web标准与可访问性

## Web标准的重要性

Web标准指的是由万维网联盟（W3C）制定的一系列技术规范和指南，用于确保网页在不同浏览器和设备上具有一致的表现和行为。以下是Web标准的重要性：

1. **跨浏览器兼容性：**
    - 遵循Web标准可以确保网页在不同的浏览器和操作系统中呈现一致，并保持良好的兼容性。这意味着用户无论使用哪种浏览器或设备访问网页，都能够获得相似的用户体验，而不会出现布局错乱或功能失效的情况。
2. **提高可访问性：**
    - 符合Web标准的网页结构清晰、语义明确，可以提高页面的可访问性，使得残障人士和使用辅助技术的用户（如屏幕阅读器）能够更轻松地访问和理解网页内容。
3. **利于搜索引擎优化（SEO）：**
    - Web标准有助于搜索引擎更准确地理解网页内容和结构，从而提高网页在搜索结果中的排名。符合标准的网页结构更易于被搜索引擎爬取和索引，同时也能产生更多的丰富片段（Rich Snippets），增加网页的曝光度和点击率。
4. **节省开发和维护成本：**
    - 遵循Web标准可以提高网页的可维护性和可扩展性，降低开发和维护的成本。符合标准的代码结构清晰、逻辑性强，易于理解和修改，有利于团队协作和项目迭代。
5. **提高网站性能：**
    - 符合Web标准的网页通常具有更高的性能和更快的加载速度。优化的HTML结构、CSS样式和JavaScript代码可以减少网页的大小和请求次数，从而提高页面的加载速度和响应速度。
6. **促进开放标准和创新：**
    - Web标准的制定是基于公开和开放的原则，鼓励开发者共同参与和贡献。遵循Web标准有助于推动开放标准的发展，促进技术创新和行业发展，从而为用户提供更加丰富和优质的网络体验。

## 网页的可访问性基础

网页可访问性（Web Accessibility）是指网站、应用程序和工具的设计、开发和维护，以确保残障人士和其他特殊群体能够平等地获取和使用网络资源的能力。以下是网页可访问性的基础知识：

1. **语义化的 HTML 结构：**
    - 使用语义化的 HTML 结构，即使用恰当的标签来描述文档的结构和内容，例如使用 `<header>`、`<nav>`、`<main>`、`<footer>` 等标签来分隔页面的主要部分，并使用适当的标题标签 `<h1>` 到 `<h6>` 来标记标题级别。
2. **合适的图像标记和描述：**
    - 对于图像元素 `<img>`，应该为其提供合适的 `alt` 属性，描述图像的内容和作用。这样即使图像无法显示时，用户仍然可以通过替代文本了解图像的信息。
3. **表单和表格的可访问性：**
    - 对于表单元素，应该使用 `<label>` 元素来关联表单控件，并使用适当的文本描述表单控件的目的。对于复杂的表格，应该使用 `<th>` 元素定义表头，并使用 `<caption>` 元素提供表格的标题或描述。
4. **键盘导航和焦点管理：**
    - 确保网页可以使用键盘进行导航和操作，而不仅仅依赖于鼠标。使用合适的焦点管理技术，确保焦点顺序合理且可预测，使得用户能够通过键盘轻松地访问和操作页面内容。
5. **颜色对比度和可读性：**
    - 确保页面的颜色对比度足够高，以便残障人士和老年人能够清晰地区分文本和背景。避免使用仅依赖颜色来传达信息的设计模式，应该提供其他视觉和文本提示。
6. **多媒体内容的可访问性：**
    - 对于音频和视频内容，应该提供字幕、注释或其他替代文本，以确保听力或视力受限的用户也能够理解内容。对于交互式媒体内容，应该提供合适的键盘导航和焦点管理。
7. **页面结构的清晰性和简洁性：**
    - 确保页面结构清晰且简洁，避免过多的复杂布局和无关内容。提供清晰的导航和链接结构，使用户能够轻松地找到所需的信息。
8. **使用可访问性工具进行测试和评估：**
    - 使用各种可访问性工具和辅助技术，如屏幕阅读器、无障碍浏览器插件等，来测试和评估网页的可访问性。及时发现并解决可访问性问题，以确保网站对所有用户都是友好和可访问的。

# 其它

## HTML编码实践和优化技巧

HTML 编码实践与优化技巧旨在提高网页性能、可访问性和代码可维护性。以下是一些HTML编码实践和优化技巧：

1. **语义化 HTML 结构：**
    - 使用语义化的 HTML 标签来描述文档结构和内容，如 `<header>`、`<nav>`、`<main>`、`<footer>` 等，以提高网页的可读性和可访问性。
2. **简洁的代码和嵌套：**
    - 保持HTML代码简洁，尽量避免不必要的嵌套和冗余标记。使用合适的标签和属性来达到所需的效果，避免使用不必要的 `<div>` 或 `<span>` 标签。
3. **减少HTTP请求：**
    - 将多个CSS文件和JavaScript文件合并为较少的文件，以减少HTTP请求次数，从而加快页面加载速度。可以使用CSS和JavaScript打包工具进行合并和压缩。
4. **优化图像和多媒体资源：**
    - 使用合适的图像格式和压缩技术来优化图像资源，以减少文件大小。对于多媒体资源，提供适当的替代文本和描述，以提高可访问性。
5. **响应式设计：**
    - 使用响应式设计技术创建适应不同屏幕尺寸和设备类型的网页布局，以提供更好的用户体验。可以使用CSS媒体查询来适应不同的屏幕宽度和设备特性。
6. **使用CSS和JavaScript优化：**
    - 将CSS和JavaScript代码放置在外部文件中，并使用合适的压缩和缓存技术来优化文件大小和加载速度。避免使用内联样式和脚本，以提高代码的可维护性和可缓存性。
7. **合理使用标记和属性：**
    - 使用合适的HTML标记和属性来描述页面的内容和结构，避免使用过时或不推荐的标记和属性。确保标记和属性的用法符合HTML规范和最佳实践。
8. **提高可访问性：**
    - 为图像、表单、多媒体内容和交互元素提供合适的替代文本和描述，以确保网页对残障人士和辅助技术用户也是友好和可访问的。
9. **注重性能优化：**
    - 使用延迟加载和异步加载技术来优化页面加载速度，将关键资源放置在页面顶部以提高首次渲染速度。优化CSS和JavaScript代码以减少渲染时间和页面交互延迟。
10. **定期检查和优化：**
    - 定期审查和优化HTML代码，移除不必要的注释、空格和代码块，以减少文件大小并提高页面加载速度。使用工具进行代码分析和检查，及时发现和解决潜在问题。

## 常见的HTML错误与解决方法

常见的HTML错误可能导致网页显示异常、排版错乱、功能失效等问题，下面是一些常见的HTML错误以及相应的解决方法：

1. **缺少必要的标签闭合：**
    - 错误示例：`<div><p>Some text</div>`
    - 解决方法：确保所有的HTML标签都有正确的闭合，例如将示例中的 `</div>` 修改为 `</p>`。
2. **标签嵌套错误：**
    - 错误示例：`<strong><em>Some text</strong></em>`
    - 解决方法：确保标签的嵌套关系正确，例如将示例中的 `</strong></em>` 修改为 `</em></strong>`。
3. **缺少必要的属性：**
    - 错误示例：`<a>Link</a>`
    - 解决方法：对于需要属性的标签，确保添加了必要的属性，例如 `href` 属性：`<a href="#">Link</a>`。
4. **使用过时的标签或属性：**
    - 错误示例：`<font color="red">Some text</font>`
    - 解决方法：避免使用已经废弃的标签和属性，改用CSS来控制样式，例如`<span style="color:red;">Some text</span>`。
5. **大小写不一致：**
    - 错误示例：`<Div>Some text</div>`
    - 解决方法：HTML标签和属性应该使用小写字母，确保大小写一致。
6. **忽略转义字符：**
    - 错误示例：`<p>He said "Hello"</p>`
    - 解决方法：对于需要转义的特殊字符，使用相应的转义字符，例如：`<p>He said &quot;Hello&quot;</p>`。
7. **缺少DOCTYPE声明：**
    - 错误示例：`<html><head>...</head><body>...</body></html>`
    - 解决方法：确保在HTML文档的开头添加正确的DOCTYPE声明，以指定文档类型和版本，例如`<!DOCTYPE html>`。
8. **忽略空格和换行：**
    - 错误示例：`<p>This is a<p>paragraph</p>`
    - 解决方法：确保标签之间有适当的空格和换行，使代码更易读且结构更清晰。
9. **忽略URL链接协议：**
    - 错误示例：`<a href="www.example.com">Link</a>`
    - 解决方法：在URL链接中添加正确的协议，例如`<a href="<http://www.example.com>">Link</a>`。
10. **使用无效的字符或特殊字符：**
    - 错误示例：使用了无效的字符或特殊字符，如控制字符、不可见字符等。
    - 解决方法：删除或替换无效字符，确保所有字符都是有效的Unicode字符。

# 资料

- _**w3schools**_
    
    [https://www.w3schools.com/html/](https://www.w3schools.com/html/)]([https://www.w3schools.com/html/](https://www.w3schools.com/html/) ” HTML Tutorial W3Schools offers free online tutorials, references and exercises in all the major languages of the web. Covering popular subjects like HTML, CSS, JavaScript, Python, SQL, Java, and many, many more. [https://www.w3schools.com/html/“](https://www.w3schools.com/html/%E2%80%9C)
    
- _**MDN**_
    
    [ MDN Web DocsMDN Web DocsMandalaMandalaMDN logoMozilla logo The MDN Web Docs site provides information about Open Web technologies including HTML, CSS, and APIs for both Web sites and progressive web apps. [https://developer.mozilla.org/zh-CN/](https://developer.mozilla.org/zh-CN/)]([https://developer.mozilla.org/zh-CN/](https://developer.mozilla.org/zh-CN/) ” MDN Web DocsMDN Web DocsMandalaMandalaMDN logoMozilla logo The MDN Web Docs site provides information about Open Web technologies including HTML, CSS, and APIs for both Web sites and progressive web apps. [https://developer.mozilla.org/zh-CN/“](https://developer.mozilla.org/zh-CN/%E2%80%9C))