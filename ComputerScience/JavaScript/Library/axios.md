![[Axios.png]]
# 概述

## 简介

- Axios 是一个开源的基于 Promise 的 HTTP 库。
- 可以在浏览器和 Node.js 种使用。
- 设计目的是为了简化 AJAX 请求的使用，使得 HTTP 请求的处理更简洁、直观和高效。

## 需求

相比于传统的 `XMLHttpRequest` 和 Jquery 的 `$.ajax()`，Axios 提供了更简洁、直观且功能强大的接口，解决了许多在 HTTP 请求时的常见问题：

- **简化异步代码**：基于 Promise，支持 `async/await`，避免了回调地狱的问题。
- **简洁的 API**：减少了代码的复杂性，简化了 HTTP 请求的处理。
- **自动 JSON 解析**：自动将 JSON 响应解析为 JavaScript 对象，简化数据处理。
- **请求/响应拦截器**：允许统一处理请求和响应，例如自动添加 token、错误统一处理等。
- **取消请求**：可以取消未完成的请求，避免不必要的网络请求。
- **并发请求**：通过 `axios.all` 轻松处理并发请求。
- **浏览器兼容性**：自动处理浏览器兼容性问题，避免手动处理。
- **超时控制**：可以轻松设置请求超时，提升用户体验。

## 对比

前端 HTTP 请求库有很多，最常用的是 Axios 和 Fetch API。

|特性|Axios|Fetch|
|---|---|---|
|**原生支持**|需要安装（`npm install axios`）|浏览器原生支持，无需额外安装|
|**API 简洁性**|更简洁、易于使用，具有更多内建功能（如请求拦截器、取消请求等）|比较原始，需要手动实现某些功能（如请求头设置、错误处理等）|
|**JSON 自动解析**|自动将 JSON 响应数据解析为 JavaScript 对象|不自动解析，需要手动调用 `.json()` 方法|
|**请求拦截器**|支持请求拦截器和响应拦截器|需要手动实现|
|**错误处理**|更易于捕获错误并处理|错误处理相对复杂，未处理 4xx、5xx 状态会直接返回成功状态|
|**请求取消**|支持 `CancelToken` 来取消请求|需要手动实现取消功能（通过 `AbortController`）|
|**支持跨浏览器**|支持所有现代浏览器，并且有一些兼容性补充|兼容性较好，IE不支持，需要 polyfill|

# 开始

## 安装

- 安装
    
    ```bash
    npm install axios
    ```
    
- 创建实例（**`axios.create([config])`**）
    
    ```jsx
    import axios from "axios";
    
    /*创建 axios 实例*/
    const instance = axios.create({
        /*URL 前缀公共部分，可以从环境文件中加载*/
        baseURL: '/api',
        /*超时*/
        timeout: 5000,
    })
    
    export default instance
    ```
    
- 创建接口
    
    ```jsx
    import request from "@/utils/request.js";
    
    /*登陆方法*/
    export function login(username, password) {
        return request.post(
            '/admin/login',
            {
                username,
                password
            }
        )
    }
    ```
    

## 使用

因为 Axios 是基于 Promise 的，所以可以通过 `.then` 表示请求成功，`.catch` 表示请求失败。

```jsx
login(username,password)
  .then(res => {
      setToken(res.token)
      ...
  })
  .catch(err => {
      ...
  })
```

# 概念

## 请求方式

Axios 创建请求的方式有两种：

- 使用 axios
    
    - `axios(config)`
        
        ```jsx
        // 发送 POST 请求
        axios({
          method: 'post',
          url: '/user/12345',
          data: {
            firstName: 'Fred',
            lastName: 'Flintstone'
          }
        });
        ```
        
    - `axios(url[, config])`
        
        ```jsx
        // 发送 POST 请求
        axios('/user/12345',{
          method: 'post',
          data: {
            firstName: 'Fred',
            lastName: 'Flintstone'
          }
        });
        ```
        
- 使用 axios 的具体请求方法
    
    - `axios#post(url[, data[, config]])`
        
        ```jsx
        // 发送 POST 请求
        axios.post('/user/12345',{
        		firstName: 'Fred',
            lastName: 'Flintstone'
          },
          {
        	  ...//config
          }
        );
        ```
        
    - `axios#request(config)`
        
    - `axios#get(url[, config])`
        
    - `axios#delete(url[, config])`
        
    - `axios#head(url[, config])`
        
    - `axios#options(url[, config])`
        
    - `axios#put(url[, data[, config]])`
        
    - `axios#patch(url[, data[, config]])`
        

## 并发请求

当同时并发多个请求时，使用两个助手函数：

- **`axios.all(iterable)`**
- **`axios.spread(callback)`**

```jsx
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```

# 配置

## 请求配置

这些是创建请求或创建实例的可选配置，`url` 是必须的，method 默认是 `get` 方法：

```jsx
{
   // 用于请求的服务器 URL
  url: '/user',

  // 是创建请求时使用的方法
  method: 'get', // default

  // 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: '<https://some-domain.com/api/>',

  // 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data, headers) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  // 也可以写成这种：'/user?ID=12345'
  params: {
    ID: 12345
  },

   // 是一个负责 `params` 序列化的函数
  // (e.g. <https://www.npmjs.com/package/qs>, <http://api.jquery.com/jquery.param/>)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

  // 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

   // 表示跨域请求时是否需要使用凭证
  withCredentials: false, // default

  // 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

 // 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

   // 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // default

  // 指示用于解码响应的编码方式
  // Note: 对于 responseType 为 'stream' 或客户端请求，这个选项会被忽略
  responseEncoding: 'utf8', // default

   // 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // 是携带 XSRF 令牌值的 HTTP 头部名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

   // 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

   // 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。
  // 如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // default

  // 定义了一个将在 Node.js 中使用的 UNIX 套接字
  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  // 只能指定 socketPath 或 proxy 中的一个
  // 如果两者都被指定，则使用 socketPath
  socketPath: null, // default

  // 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

## 响应结构

某个请求的响应包含以下信息：

```jsx
{
  // 由服务器提供的响应
  data: {},

  // 来自服务器响应的 HTTP 状态码
  status: 200,

  // 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // 服务器响应的头
  headers: {},

   // 是为请求提供的配置信息
  config: {},

  // 是生成此响应的请求
  // 它是 Node.js 中最后一个 ClientRequest 实例（在重定向中）
  // 以及浏览器中的一个 XMLHttpRequest 实例
  request: {}
}
```

## 配置优先级

有四种使用的配置：

- 全局配置
    
    ```jsx
    axios.defaults.baseURL = '<https://api.example.com>';
    axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
    axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
    ```
    
- 实例配置
    
    ```jsx
    // Set config defaults when creating the instance
    const instance = axios.create({
      baseURL: '<https://api.example.com>'
    });
    
    // Alter defaults after instance has been created
    instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
    ```
    
- 请求配置
    
    ```jsx
    // 发送 POST 请求
    axios.post('/user/12345',{
    		firstName: 'Fred',
        lastName: 'Flintstone'
      },
      {
    	  ...//config
      }
    );
    ```
    
- 默认配置
    
    `lib/defaults.js` 中找到的库的默认值
    

在这四种配置中：请求配置>实例配置>全局配置>默认配置，并以优先级顺序进行合并。

## 拦截器

Axios 可以设置实例的请求和响应拦截器，在请求和响应之前使用自定义逻辑，并对失败做出处理。

```jsx
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```

如果希望某个请求不使用拦截器，可以将之移除：

```jsx
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

## 错误处理

```jsx
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // 请求已发出，服务器响应的状态码超出了 2xx 范围
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // 请求已发出，但未收到响应
      // 在浏览器中是 XMLHttpRequest 实例，在 Node.js 中是 http.ClientRequest 实例
      console.log(error.request);
    } else {
      // "在设置请求时发生了某些事情，导致触发了错误
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

对于响应码范围可以通过配置的 `validateStatus` 属性设置。

## 取消请求

某些情况下，可能需要对发送的请求进行取消。

`Axios` 提供了 `CancelToken.source()` 方法来创建取消令牌，它返回一个包含 `token` 和 `cancel` 方法的对象。`token` 用于绑定到请求，`cancel` 用于触发取消操作。

```jsx
// 引入 axios
const axios = require('axios');

// 创建取消令牌
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

// 发送请求
axios.get('<https://jsonplaceholder.typicode.com/todos/1>', {
  cancelToken: source.token  // 将取消令牌与请求绑定
})
.then(response => {
  console.log('Response:', response.data);
})
.catch(error => {
  // 判断是否是被取消的请求
  if (axios.isCancel(error)) {
    console.log('Request canceled', error.message);
  } else {
    console.error('Request failed', error);
  }
});

// 在某个时刻取消请求
source.cancel('Request canceled by the user');
```

取消多个请求时，创建多个取消 `source` 并与请求绑定即可。

# 附录
---
> [!website]
> http://www.axios-js.com/

> [!code]
> https://github.com/axios/axios

> [!doc]
> [2024 你会用到的10个前端请求库](https://juejin.cn/post/7389131639300014134)
