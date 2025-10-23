# 概述

## 简介

## 需求

## 对比

# 开始

## 安装

## 使用

# 概念

# 配置

# 附录

<aside> 🌐

**官网地址：** [https://nodejs.org/zh-cn](https://nodejs.org/zh-cn)

</aside>

<aside> 💾

**代码地址：** [https://github.com/nodejs/node](https://github.com/nodejs/node)

</aside>

<aside> 📓

**其它文档：** [https://nqdeng.github.io/7-days-nodejs/#1](https://nqdeng.github.io/7-days-nodejs/#1) [https://nodejs.cn/en](https://nodejs.cn/en)

</aside>

# 1.Node.js基础https://nqdeng.github.io/7-days-nodejs/#1

## 1.1 概述

JS是一种脚本语言，浏览器负责对JS进行解析和执行，也就是说浏览器就是JS的运行时环境，浏览器也提供了`document`之类的内置对象。 而node.js的作用是将JS从浏览器中脱离出来，对JS提供运行时环境，使得JS可以单独运行，不再需要依赖于浏览器。 node.js的设计初衷在于提供一种高效、可拓展的方式来构建应用程序，因为node.js单线程、非阻塞特性，使得它可以处理大量并行连接，而不用等待I/O操作。

## 1.2 安装运行

对于windows直接在官网下载msi后缀文件，进行安装，可以添加进环境变量 [Node.js](https://nodejs.org/en) 对于Linux可以使用编译安装：

1. 确保系统下`g++`版本在4.6以上，python版本在2.6以上。
2. 从官网下载`tar.gz`后缀的NodeJS最新版源代码包并解压到某个位置。
3. 进入解压到的目录，使用以下命令编译和安装。

```bash
$ ./configure
$ make
$ sudo make install
```

---

运行时打开终端，输入node进入命令交互模式，输入语句，回车执行：

```bash
$ node
> console.log('Hello World!');Hello World!
```

终端下也可以直接执行JS文件：

```bash
$ node hello.js
Hello World!
```

## 1.3 模块

程序开发中，我们根据功能不同编写不同的JS文件，而这些JS文件就称为模块，文件路径名就是模块名，后缀可以省略。 在这些模块中，有`require`、`exports`、`module`三个预先定义好的变量可供使用。

### `require`

`require`用于在当前模块加载和使用别的模块，模块名可以使用相对路径或者绝对路径，也可以使用这种方法加载JSON文件。

```jsx
var foo1 = require('./foo');var foo2 = require('./foo.js');var foo3 = require('/home/user/foo');var foo4 = require('/home/user/foo.js');var data = require('./data.json');
```

### `exports`

`exports`用于导出当前模块的公有方法和属性，别的模块通过`require`使用当前模块时，得到的就是当前模块的`exports`对象。

```jsx
exports.hello = function () {
    console.log('Hello World!');};
```

### `module`

`module`用于访问模块信息以及替换当前模块的导出信息。 例如模块的默认导出对象是一个普通对象，现需要换成函数：

```jsx
module.exports = function () {
    console.log('Hello World!');};
```

一个模块的JS代码仅在第一次被使用时执行一次，并初始化导出对象，而后导出对象将被缓存，以便重复利用

# 2.代码的组织和部署

## 2.1 模块路径解析规则

`require`导入模块时，支持`/`或者`C:`这样的绝对路径，也支持`./`这样的相对路径，但不管绝对路径或者相对路径，都会造成模块之间的耦合，因此`require`函数支持第三种导入规则，直接以名称进行导入。

1. 内置模块

如果传入`require`的名称是内置模块的名称，不做路径解析，直接返回内置模块的导出对象，例如：`require('fs')`

1. node_modules目录

NodeJS定义了一个特殊的目录`node_modules`目录用于存放模块，如果模块名称不是内置模块名称，则进入这个目录查找模块。 查找时，先在导入模块同级的node_modules目录中寻找，没有则进入上一级的node_modules目录中寻找，没有则进入再上一级..

```jsx
 /home/user/node_modules/foo/bar /home/node_modules/foo/bar /node_modules/foo/bar
```

1. NODE_PATH环境变量

NodeJS允许NODE_PATH指定额外的模块搜索路径，NODE_PATH包含一到多个目录路径，Linux使用`:`分割，win使用`;`分割

## 2.2 包（package）

现在JS模块的基本组成单位是单个JS文件，但有些复杂的JS模块往往由多个子模块组成，并把所有子模块放在同一个目录中，组成的这个大模块我们就称之为包。 在组成包的所有子模块中，需要有一个入口模块（主模块），入口模块的导出对象称为包的导出对象。 目录结构：

```jsx
- /home/user/lib/    - cat/        head.js        body.js        main.js
```

在其他模块里使用这个包时，需要加载入口模块，如：`require('/home/user/lib/cat/main')`但这样看起来像是加载了单个JS文件，所有我们需要做点额外的工作，让它看起来像一个整体。

### `index.js`

当模块的文件名时index.js时，加载模块时，可以使用模块所在目录的路径，代替模块文件的路径。 利用这个原理，只要将包中的入口模块的文件名定为index.js，就可以在加载包时，直接使用包目录的路径。 如：`require('/home/user/lib/cat')`

### `package.json`

如果想要自定义入口模块的文件名和存放位置，就需要在包目录下包含一个package.json文件，并在其中定义入口模块的文件名和位置，比如包cat的结构如下：

```jsx
- /home/user/lib/    - cat/        + doc/        - lib/            head.js            body.js            main.js        + tests/        package.json
```

其中package.json的内容为：

```jsx
{
    "name": "cat",    "main": "./lib/main.js"}
```

使用时就可以使用`require('/home/user/lib/cat')`加载模块，NodeJS会根据包中的package.js找到入口文件。

## 2.3 命令行程序

使用NodeJS编写的要么是一个包，要么是一个命令行程序，而包也是服务于命令行程序的，因此在部署代码时，需要一些技巧，让用户觉得是在使用一个命令行程序。 例如编写了一个命令行程序（把命令行参数原样打印）并部署在`/home/user/bin/node-echo.js`这个位置，为了在任何目录下都能运行这个程序，需要使用以下终端命令：

```jsx
$ node /home/user/bin/node-echo.js Hello World
Hello World
```

这样看起来并不像个命令行程序，下面才是我们希望的：

```jsx
$ node-echo Hello World
```

### `Linux`

在Linux系统下，我们可以把JS文件当作shell脚本来运行，而从达到上述目的。具体步骤：

1. 在shell脚本中，可以通过`#!`注释来指定当前脚本使用的解析器。首先在node-echo.js文件顶部增加一行注释，表明当前脚本使用NodeJS解析

```jsx
 #! /usr/bin/env node
```

1. 使用以下命令赋予node-echo.js文件执行权限

```jsx
 $ chmod +x /home/user/bin/node-echo.js
```

1. 最后在PATH环境变量中指定的某个目录下，例如`/usr/local/bin`下创建一个软链文件，文件名与终端命令同名。

```jsx
 $ sudo ln -s /home/user/bin/node-echo.js /usr/local/bin/node-echo
```

### `Windows`

在Windows系统下需要依靠.cmd文件来解决问题，假设node-echo.js存放在`C:\\Users\\user\\bin`目录，并且该目录添加到PATH环境变量中，接下来需要在该目录下新建一个node-echo.cmd的文件，文件内容：

```jsx
@node "C:\\User\\user\\bin\\node-echo.js" %*
```

这样处理后就可以使用`node-echo`命令了

## 2.4 工程目录

以命令行程序为例，工程目录应当包含命令行模式和API模式两种使用方式，并且会借助第三方包编写程序，还应当包含文档和测试用例，结构如下：

```jsx
- /home/user/workspace/node-echo/   # 工程目录
    - bin/                          # 存放命令行相关代码
        node-echo
    + doc/                          # 存放文档
    - lib/                          # 存放API相关代码
        echo.js    - node_modules/                 # 存放三方包
        + argv/    + tests/                        # 存放测试用例
    package.json                    # 元数据文件
    README.md                       # 说明文件
```

## 2.5 NPM

NPM是NodeJS的包管理器，使用场景有下面几种情况：

1. 允许用户使用NPM服务器下载别人编写的第三方包到本地使用
2. 允许用户使用NPM服务器下载并安装别人编写的命令行程序到本地使用
3. 允许用户将自己编写的包或者命令行程序上传到NPM服务器供别人使用

NPM建立起了NodeJS生态圈，NodeJS开发者可以通过NPM互通有无

### 下载三方包

使用第三方包是首先需要知道包名，可以在[npm](https://www.npmjs.com/)中进行搜索，然后在工程目录下直接打开终端，使用命令下载三方包

```jsx
$ npm install argv
...argv@0.0.2 node_modules\\argv
```

下载好后，argv包就放在工程目录下的node_modules目录中，因此代码中只需要通过`require('argv')`的方式就好，不需要指定路径 这种方式下载的三方包默认是最新版本，如果需要指定版本的话，需要在包名后加上`@<version>`

```jsx
$ npm install argv@0.0.1...argv@0.0.1 node_modules\\argv
```

如果使用的三方依赖太多，一条条的下载非常麻烦，所以NPM对package.json做了拓展，允许在其中声明三方包依赖，package.json：

```jsx
{
    "name": "node-echo",    "main": "./lib/echo.js",    "dependencies": {
        "argv": "0.0.2"    }
}
```

这样就可以在工程目录下直接使用`npm install`命令批量安装三方包，经过这样处理，当node-echo.js文件上传NPM服务器之后，别人下载这个包时，NPM会根据包中申明的三方包依赖自动下载进一步依赖的三方包，从而使用户不需要自己去解决包依赖问题。

### 安装命令行程序

安装命令行程序和下载三方包类似，比如有node-echo这个命令行程序，只需要node-echo配置好package.json，用户使用下面命令安装即可：

```jsx
$ npm install node-echo -g
```

- `g`表示全局安装，因此node-echo会默认安装到以下位置，并且NPM会自动创建好Linux下的软链文件或者Windows下的.cmd文件

```jsx
- /usr/local/               # Linux系统下    - lib/node_modules/        + node-echo/        ...    - bin/        node-echo
        ...    ...- %APPDATA%\\npm\\            # Windows系统下
    - node_modules\\
        + node-echo\\
        ...    node-echo.cmd    ...
```

### 发布代码

第一次发布代码需要注册账号，终端运行`npm adduser`根据提示即可，接下来需要编辑package.json文件，添加必要字段：

```jsx
{
    "name": "node-echo",           # 包名，在NPM服务器上须要保持唯一
    "version": "1.0.0",            # 当前版本号
    "dependencies": {              # 三方包依赖，需要指定包名和版本号
        "argv": "0.0.2"      },    "main": "./lib/echo.js",       # 入口模块位置
    "bin" : {
        "node-echo": "./bin/node-echo"      # 命令行程序名和主模块位置
    }
}
```

最后就可以在package.json文件目录下，运行`npm publish`发布代码

### 版本号

NPM使用语义版本号管理代码，语义版本号分为`X.Y.Z`三位，分别代表主版本号、次版本号和补丁版本号：

```jsx
+ 如果只是修复bug，需要更新Z位。
+ 如果是新增了功能，但是向下兼容，需要更新Y位。
+ 如果有大变动，向下不兼容，需要更新X位。
```

# 3.文件操作

## 3.1 文件拷贝

NodeJS提供了最基础的文件操作API，但是像文件拷贝这种高级功能没有提供，我们需要接受源文件路径与目标路径两个参数。

### 小文件拷贝

使用NodeJS内置的`fs`模块实现功能

```jsx
var fs = require('fs');function copy(src, dst) {
    fs.writeFileSync(dst, fs.readFileSync(src));}
function main(argv) {
    copy(argv[0], argv[1]);}
main(process.argv.slice(2));
```

以上程序使用`fs.readFileSync`从源路径读取文件内容，并使用`fs.writeFileSync`将文件内容写入目标路径。 process是一个全局变量，可通过`process.argv`获得命令行参数。由于`argv[0]`固定等于NodeJS执行程序的绝对路径，`argv[1]`固定等于主模块的绝对路径，因此第一个命令行参数从`argv[2]`这个位置开始。

### 大文件拷贝

上面拷贝程序适合小文件，对于大文件，一次性将文件内容拷贝到内存，内存会溢出，只能一点点进行拷贝，或者添加缓存

```jsx
var fs = require('fs');function copy(src, dst) {
    fs.createReadStream(src).pipe(fs.createWriteStream(dst));}
function main(argv) {
    copy(argv[0], argv[1]);}
main(process.argv.slice(2));
```

以上程序使用`fs.createReadStream`创建了一个源文件的只读数据流，并使用`fs.createWriteStream`创建了一个目标文件的只写数据流，并且用`pipe`方法把两个数据流连接了起来。

## 3.2 API走马观花

大致介绍NodeJS提供了哪些和文件操作有关的API

### buffer（数据块）

> 官方文档：buffer 缓冲区 | Node.js v18 文档

JS语言本身只有字符串数据类型，没有二进制数据类型，因此NodeJS提供了一个与String对等的全局构造函数Buffer来对二进制数据进行操作。 除了可以读取文件获得Buffer实例外，还可以直接构造：

```jsx
var bin = new Buffer([ 0x68, 0x65, 0x6c, 0x6c, 0x6f ]);
```

Buffer与字符串类似，除了可以使用`.length`属性获得字节长度外，还可以用`[index]`方式读取指定位置的字节：

```jsx
bin[0]; // => 0x68;
```

Buffer与字符串能够互相转化，例如可以使用指定编码将二进制数据转化为字符串：

```jsx
var str = bin.toString('utf-8'); // => "hello"
```

或者反过来，将字符串转换为指定编码下的二进制数据：

```jsx
var bin = new Buffer('hello', 'utf-8'); // => <Buffer 68 65 6c 6c 6f>
```

Buffer与字符串有一个重要区别。字符串是只读的，并且对字符串的任何修改得到的都是一个新字符串，原字符串保持不变。 至于Buffer，更像是可以做指针操作的C语言数组。例如，可以用[index]方式直接修改某个位置的字节。

```jsx
bin[0] = 0x48;
```

而.slice方法也不是返回一个新的Buffer，而更像是返回了指向原Buffer中间的某个位置的指针：

```jsx
[ 0x68, 0x65, 0x6c, 0x6c, 0x6f ]
    ^           ^    |           |   bin     bin.slice(2)
```

因此对.slice方法返回的Buffer的修改会作用于原Buffer

```jsx
var bin = new Buffer([ 0x68, 0x65, 0x6c, 0x6c, 0x6f ]);var sub = bin.slice(2);sub[0] = 0x65;console.log(bin); // => <Buffer 68 65 65 6c 6f>
```

也因此，如果想要拷贝一份Buffer，得首先创建一个新的Buffer，并通过`.copy`方法把原Buffer中的数据复制过去。这个类似于申请一块新的内存，并把已有内存中的数据复制过去。

```jsx
var bin = new Buffer([ 0x68, 0x65, 0x6c, 0x6c, 0x6f ]);var dup = new Buffer(bin.length);bin.copy(dup);dup[0] = 0x48;console.log(bin); // => <Buffer 68 65 6c 6c 6f>console.log(dup); // => <Buffer 48 65 65 6c 6f>
```

总之，Buffer将JS的数据处理能力从字符串扩展到了任意二进制数据。

### Stream（数据流）

> 官方网站：stream 流 | Node.js v18 文档

当内存中无法一次装下需要处理的数据时，或者一边读取一边处理更加高效时，我们就需要用到数据流，Stream提供对数据流的操作。 以上边的大文件拷贝程序为例，我们可以为数据来源创建一个只读数据流：

```jsx
var rs = fs.createReadStream(pathname);rs.on('data', function (chunk) {
    doSomething(chunk);});rs.on('end', function () {
    cleanUp();});
```

Stream基于事件机制工作，所有Stream的实例都继承于NodeJS提供的[EventEmitter](http://nodejs.org/api/events.html)。 上边的代码中data事件会源源不断地被触发，不管doSomething函数是否处理得过来。代码可以继续做如下改造：

```jsx
var rs = fs.createReadStream(src);rs.on('data', function (chunk) {
    rs.pause();    doSomething(chunk, function () {
        rs.resume();    });});rs.on('end', function () {
    cleanUp();});
```

以上代码给`doSomething`函数加上了回调，因此我们可以在处理数据前暂停数据读取，并在处理数据后继续读取数据。 此外，我们也可以为数据目标创建一个只写数据流：

```jsx
var rs = fs.createReadStream(src);var ws = fs.createWriteStream(dst);rs.on('data', function (chunk) {
    ws.write(chunk);});rs.on('end', function () {
    ws.end();});
```

但是上面程序还是存在提到过的问题，如果写入速度跟不上读取速度，内存可能会溢出，我们可以根据.write方法的返回值来判断传入的数据是写入目标了，还是临时放在了缓存了，并根据drain事件来判断什么时候只写数据流已经将缓存中的数据写入目标，可以传入下一个待写数据了

```jsx
var rs = fs.createReadStream(src);var ws = fs.createWriteStream(dst);rs.on('data', function (chunk) {
    if (ws.write(chunk) === false) {
        rs.pause();    }
});rs.on('end', function () {
    ws.end();});ws.on('drain', function () {
    rs.resume();});
```

因为这种对数据流的操作应用广泛，所以NodeJS提供了`.pipe`方法，内部机制与上述代码类似

### File System（文件系统）

> 官方文档：fs 文件系统 | Node.js v18 文档

NodeJS通过`fs`模块提供对文件的操作，`fs`模块提供的API基本上可以分为以下三类：

- 文件属性读写

其中常用的有`fs.stat`、`fs.chmod`、`fs.chown`等等。

- 文件内容读写

其中常用的有`fs.readFile`、`fs.readdir`、`fs.writeFile`、`fs.mkdir`等等。

- 底层文件操作

其中常用的有`fs.open`、`fs.read`、`fs.write`、`fs.close`等等。 NodeJS最精华的异步IO模型在fs模块里有着充分的体现，例如上边提到的这些API都通过回调函数传递结果。以fs.readFile为例：

```jsx
fs.readFile(pathname, function (err, data) {
    if (err) {
        // Deal with error.    } else {
        // Deal with data.    }
});
```

如上边代码所示，基本上所有fs模块API的回调参数都有两个。第一个参数在有错误发生时等于异常对象，第二个参数始终用于返回API方法执行结果。 `fs`模块的所有异步API都有对应的同步版本，用于无法使用异步操作时，或者同步操作更方便时的情况。 同步API除了方法名的末尾多了一个`Sync`之外，异常对象与执行结果的传递方式也有相应变化。同样以fs.readFileSync为例：

```jsx
try {
    var data = fs.readFileSync(pathname);    // Deal with data.} catch (err) {
    // Deal with error.}
```

### Path（路径）

> 官方文档：path 路径 | Node.js v18 文档

文件操作离不开路径，NodeJS提供了`path`模块来对简化路径相关操作。

- path.normalize

将传入的路径转换为标准路径，具体讲的话，除了解析路径中的.与..外，还能去掉多余的斜杠。 如果有程序需要使用路径作为某些数据的索引，但又允许用户随意输入路径时，就需要使用该方法保证路径的唯一性。

```jsx
  var cache = {};  function store(key, value) {
      cache[path.normalize(key)] = value;  }
  store('foo/bar', 1);  store('foo//baz//../bar', 2);  console.log(cache);  // => { "foo/bar": 2 }
```

标准化之后的路径里的斜杠在Windows系统下是`\\`，而在Linux系统下是`/`。如果想保证任何系统下都使用/作为路径分隔符的话，需要 用`.replace(/\\\\/g, '/')`再替换一下标准路径。

- path.join

将传入的多个路径拼接为标准路径。该方法可避免手工拼接路径字符串的繁琐，并且能在不同系统下正确使用相应的路径分隔符

```jsx
path.join('foo/', 'baz/', '../bar'); // => "foo/bar"
```

- path.extname

当我们需要根据不同文件扩展名做不同操作时，该方法就显得很好用

```jsx
path.extname('foo/bar.js'); // => ".js"
```

## 3.3 遍历目录

遍历目录是操作文件时的一个常见需求。比如写一个程序，需要找到并处理指定目录下的所有JS文件时，就需要遍历整个目录。

### 递归算法

递归算法与数学归纳法类似，通过不断缩小问题的规模来解决问题

```jsx
function factorial(n) {
    if (n === 1) {
        return 1;    } else {
        return n * factorial(n - 1);    }
}
```

上边的函数用于计算N的阶乘（N!）。可以看到，当N大于1时，问题简化为计算N乘以N-1的阶乘。当N等于1时，问题达到最小规模，不需要再简化，因此直接返回1。

### 遍历算法

目录是一个树状结构，在遍历时一般使用深度优先+先序遍历算法。深度优先，意味着到达一个节点后，首先接着遍历子节点而不是邻居节点。先序遍历，意味着首次到达了某节点就算遍历完成，而不是最后一次返回某节点才算数。因此使用这种遍历方式时，下边这棵树的遍历顺序是A > B > D > E > C > F。

```jsx
          A
         / \\
        B   C
       / \\   \\
      D   E   F
```

### 同步遍历




![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/128a96c7-b1b7-4395-91e6-76ce13b64bbb/image.png)

# 概述

## 简介

- NPM（Node Package Manager）是 JavaScript 的包管理工具。
- NPM 是 Node.js 默认的包管理器，随着 Node.js 的捆绑安装。

## 需求

## 对比

# 开始

## 安装

## 使用

# 概念

## 包

## 架构

## package.json

# 配置

# 附录

<aside> 🌐

**官网地址：** [https://www.npmjs.com/](https://www.npmjs.com/)

</aside>

<aside> 💾

**代码地址：** [https://github.com/npm](https://github.com/npm)

</aside>

<aside> 📓

**其它文档：** [https://npmtrends.com/](https://npmtrends.com/) [https://npm.nodejs.cn/](https://npm.nodejs.cn/)

</aside>

# NPM

|标签|前端|
|---|---|
|创建者|Maktub|
|创建时间|2024/05/17 23:45|
|状态||

代理：

```jsx
npm config set proxy http://proxy_address:port
```

https

```jsx
npm config set https-proxy https://proxy_address:port
```

查看代理

```jsx
npm config get proxy
npm config get https-proxy
```

取消代理

```jsx
npm config delete proxy
npm config delete https-proxy
```

设置镜像

```jsx
npm config set registry <镜像地址>
```

淘宝镜像

```jsx
npm config set registry <https://registry.npmmirror.com/>
```

官方

```jsx
npm config set registry <https://registry.npmjs.org/>
```

查看

```jsx
npm config get registry
```


**最终：**
**在环境变量中设置 http_proxy 和 https_proxy ，因为 CLI 工具在运行时会检测这两个环境变量，自动把 HTTP 或 HTTPS 请求走代理**