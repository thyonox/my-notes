![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/0536d6a3-c650-4e09-9e75-6d2605917d10/a9b1673e-a0f2-47b5-92e3-5ed578678756/image.png)

# 概述

## 简介

- Electron 是一个用于构建跨平台桌面应用程序的开源框架。
- 基于 Chromium 和 Node.js，可以使用前端技术（HTML、CSS、JavaScript）开发应用。

## 需求

传统 Web 开发和桌面应用开发泾渭分明，它们使用不同技术栈，而且不同的平台也使用不同的技术栈，这就导致一款产品既需要 Web 开发，又需要单独开发不同平台对应的应用，在多平台版本的开发和维护中就产生了很大挑战。

Electron 备受青睐的主要原因在于，它将 Web开发与桌面应用开发的技术栈打通，可以使用同一套技术开发 Web 应用和桌面应用。而且构建的桌面应用程序具有跨平台的特性，这也是它流行的重要原因。

## 对比

同类的跨平台技术有很多，诸如：

- NW.js：同 Electron 类似，都是基于 Chromium 和 Node.js，主进程和渲染进程合二为一，开发灵活，社区不活跃，发展较慢。
- Flutter：使用 Dart 语言开发，使用自定义的渲染引擎渲染界面
- React Native for Windows/macOS：使用原生组件渲染界面，性能更好
- Tauri：界面与逻辑分离，逻辑使用 Rust 开发，占用内存和打包体积远远小于 Electron

# 开始

## 安装

创建一个 Electron 应用有以下几种方式：

- **手动搭建**
    
    - 初始化 Node.js 项目
        
        ```jsx
        npm init -y
        ```
        
    - 安装 Electron
        
        ```jsx
        npm install electron --save-dev
        ```
        
    - 创建项目文件
        
        - index.html（用于显示的 HTML 页面）
            
            ```jsx
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <title>Electron App</title>
            </head>
            <body>
            <h1>Hello, Electron!</h1>
            </body>
            </html>
            ```
            
        - main.js（Electron 主进程代码）
            
            ```jsx
            const { app, BrowserWindow } = require('electron')
            
            let win
            
            function createWindow() {
                win = new BrowserWindow({
                    width: 800,
                    height: 600,
                    webPreferences: {
                        nodeIntegration: true
                    }
                })
            
                win.loadFile('index.html')
            
                win.on('closed', () => {
                    win = null
                })
            }
            
            app.whenReady().then(createWindow)
            
            app.on('window-all-closed', () => {
                if (process.platform !== 'darwin') {
                    app.quit()
                }
            })
            ```
            
        - package.json（需要添加启动脚本）
            
            ```jsx
            {
              "name": "electron-try",
              "version": "1.0.0",
              "main": "main.js",
              "scripts": {
                "test": "echo \\"Error: no test specified\\" && exit 1",
                "start": "electron ."
              },
              "keywords": [],
              "author": "",
              "license": "ISC",
              "description": "",
              "devDependencies": {
                "electron": "^33.2.1"
              }
            }
            ```
            
    - 注意事项
        
        因为 Electron 的二进制文件比较大，且不依赖于 npm 的仓库地址，需要手动指定地址。
        
        在终端中输入 `npm config edit` 打开 npm 的配置文件，添加：
        
        ```jsx
        electron_builder_binaries_mirror=https://npmmirror.com/mirrors/electron-builder-binaries/
        electron_mirror=https://cdn.npmmirror.com/binaries/electron/
        ```
        
- Electron CLI
    
    - 安装 Electron CLI
        
        ```jsx
        npm install -g electron-forge
        ```
        
    

## 使用

# 概念

# 配置

# 附录

<aside> 🌐

**官网地址：** [https://www.electronjs.org/zh/](https://www.electronjs.org/zh/)

</aside>

<aside> 💾

**代码地址：** [https://github.com/electron/electron](https://github.com/electron/electron)

</aside>

<aside> 📓

**其它文档：**

</aside>