# Axios 速成

<!-- toc -->

## 6.1. 简介

Axios 是一个基于 promise 的网络请求库，可以用于浏览器和 node.js

```
npm install axios
```

```js
import axios from "axios"
axios.get ('/user')
     .then (res => console.log (resp.data))
```

API 测试服务器： [http://43.139.239.29/](http://43.139.239.29/)

## 6.2. 请求

### 6.2.1. get 请求

```js
// 向给定 ID 的用户发起请求
axios.get ('/user?ID=12345')
  .then (function (response) {
    // 处理成功情况
    console.log (response);
  })
  .catch (function (error) {
    // 处理错误情况
    console.log (error);
  })
  .finally (function () {
    // 总是会执行
  });
```

携带请求参数

```js
// 上述请求也可以按以下方式完成（可选）
axios.get ('/user', {
    params: {
      ID: 12345
    }
  })
  .then (function (response) {
    console.log (response);
  })
  .catch (function (error) {
    console.log (error);
  })
  .finally (function () {
    // 总是会执行
  });  
```

### 6.2.2. post 请求

默认 `post 请求体` 中的数据将会以 `json` 方式提交

```js
axios.post ('/user', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
.then (function (response) {
  console.log (response);
})
.catch (function (error) {
  console.log (error);
});
```

  

## 6.3. 响应

响应的数据结构如下：

```js
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,
  
  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 是服务器响应头
  // 所有的 header 名称都是小写，而且可以使用方括号语法访问
  // 例如: `response.headers ['content-type']`
  headers: {},

  // `config` 是 `axios` 请求的配置信息
  config: {},

  // `request` 是生成此响应的请求
  // 在 node.js 中它是最后一个 ClientRequest 实例 (in redirects)，
  // 在浏览器中则是 XMLHttpRequest 实例
  request: {}
}
```

  

## 6.4. 配置

```js
const instance = axios.create ({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});

// 可用的配置项如下：
{
  // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // 默认值

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 它只能用于 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 数组中最后一个函数必须返回一个字符串， 一个 Buffer 实例，ArrayBuffer，FormData，或 Stream
  // 你可以修改请求头。
  transformRequest: [function (data, headers) {
    // 对发送的 data 进行任意转换处理

    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对接收的 data 进行任意转换处理

    return data;
  }],

  // 自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是与请求一起发送的 URL 参数
  // 必须是一个简单对象或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是可选方法，主要用于序列化 `params`
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function (params) {
    return Qs.stringify (params, {arrayFormat: 'brackets'})
  },

  // `data` 是作为请求体被发送的数据
  // 仅适用 'PUT', 'POST', 'DELETE 和 'PATCH' 请求方法
  // 在没有设置 `transformRequest` 时，则必须是以下类型之一:
  //- string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  //- 浏览器专属: FormData, File, Blob
  //- Node 专属: Stream, Buffer
  data: {
    firstName: 'Fred'
  },
  
  // 发送请求体数据的可选语法
  // 请求方式 post
  // 只有 value 会被发送，key 则不会
  data: 'Country=Brasil&City=Belo Horizonte',

  // `timeout` 指定请求超时的毫秒数。
  // 如果请求时间超过 `timeout` 的值，则请求会被中断
  timeout: 1000, // 默认值是 `0` (永不超时)

  // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, //default

  // `adapter` 允许自定义处理请求，这使测试更加容易。
  // 返回一个 promise 并提供一个有效的响应 （参见 lib/adapters/README.md）。
  adapter: function (config) {
    /* ... */
  },

  // `auth` HTTP Basic Auth
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` 表示浏览器将要响应的数据类型
  // 选项包括: 'arraybuffer', 'document', 'json', 'text', 'stream'
  // 浏览器专属：'blob'
  responseType: 'json', // 默认值

  // `responseEncoding` 表示用于解码响应的编码 (Node.js 专属)
  // 注意：忽略 `responseType` 的值为'stream'，或者是客户端请求
  // Note: Ignored for `responseType` of'stream' or client-side requests
  responseEncoding: 'utf8', // 默认值

  // `xsrfCookieName` 是 xsrf token 的值，被用作 cookie 的名称
  xsrfCookieName: 'XSRF-TOKEN', // 默认值

  // `xsrfHeaderName` 是带有 xsrf token 值的 http 请求头名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // 默认值

  // `onUploadProgress` 允许为上传处理进度事件
  // 浏览器专属
  onUploadProgress: function (progressEvent) {
    // 处理原生进度事件
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  // 浏览器专属
  onDownloadProgress: function (progressEvent) {
    // 处理原生进度事件
  },

  // `maxContentLength` 定义了 node.js 中允许的 HTTP 响应内容的最大字节数
  maxContentLength: 2000,

  // `maxBodyLength`（仅 Node）定义允许的 http 请求内容的最大字节数
  maxBodyLength: 2000,

  // `validateStatus` 定义了对于给定的 HTTP 状态码是 resolve 还是 reject promise。
  // 如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，
  // 则 promise 将会 resolved，否则是 rejected。
  validateStatus: function (status) {
    return status >= 200 && status < 300; // 默认值
  },

  // `maxRedirects` 定义了在 node.js 中要遵循的最大重定向数。
  // 如果设置为 0，则不会进行重定向
  maxRedirects: 5, // 默认值

  // `socketPath` 定义了在 node.js 中使用的 UNIX 套接字。
  //e.g. '/var/run/docker.sock' 发送请求到 docker 守护进程。
  // 只能指定 `socketPath` 或 `proxy` 。
  // 若都指定，这使用 `socketPath` 。
  socketPath: null, //default

  // `httpAgent` and `httpsAgent` define a custom agent to be used when performing http
  //and https requests, respectively, in node.js. This allows options to be added like
  // `keepAlive` that are not enabled by default.
  httpAgent: new http.Agent ({ keepAlive: true }),
  httpsAgent: new https.Agent ({ keepAlive: true }),

  // `proxy` 定义了代理服务器的主机名，端口和协议。
  // 您可以使用常规的 `http_proxy` 和 `https_proxy` 环境变量。
  // 使用 `false` 可以禁用代理功能，同时环境变量也会被忽略。
  // `auth` 表示应使用 HTTP Basic auth 连接到代理，并且提供凭据。
  // 这将设置一个 `Proxy-Authorization` 请求头，它会覆盖 `headers` 中已存在的自定义 `Proxy-Authorization` 请求头。
  // 如果代理服务器使用 HTTPS，则必须设置 protocol 为 `https`
  proxy: {
    protocol: 'https',
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  //see https://axios-http.com/zh/docs/cancellation
  cancelToken: new CancelToken (function (cancel) {
  }),

  // `decompress` indicates whether or not the response body should be decompressed 
  //automatically. If set to `true` will also remove the 'content-encoding' header 
  //from the responses objects of all decompressed responses
  //- Node only (XHR cannot turn off decompression)
  decompress: true // 默认值

}
```

## 6.5. 拦截器

```js
// 添加请求拦截器
axios.interceptors.request.use (function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject (error);
  });

// 添加响应拦截器
axios.interceptors.response.use (function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    return Promise.reject (error);
  });
```

## 实战

单文件写法

```js
<script setup>
import axios from 'axios'

const instance = axios.create ({
  baseURL: 'http://43.139.239.29',
  timeout: 1000,
  headers: {
    'Content-Type': 'application/json'
  }
})

function getInfo () {
  //const url = 'https://jsonplaceholder.typicode.com/todos/1'
  instance.get ('/get')
    .then (response => {
      console.log (response.data)
    })
    .catch (error => {
      console.log (error)
    }
    )
}
function getInfoWithParam () {
  //const url = 'https://jsonplaceholder.typicode.com/todos/1'
  instance.get ('/get', {
    params: {
      id: 1,
      name: 'John'
    }
  })
    .then (response => {
      console.log (response.data)
    })
    .catch (error => {
      console.log (error)
    }
    )
}

function postInfo () {
  //const url = 'https://jsonplaceholder.typicode.com/todos/1'
  instance.post ('/post')
    .then (response => {
      console.log (response)
    })
    .catch (error => {
      console.log (error)
    }
    )
}
function postInfoWithParam () {
  //const url = 'https://jsonplaceholder.typicode.com/todos/1'
  instance.post ('/post', {
    id: 1,
    name: 'John'
  })
    .then (response => {
      console.log (response)
    })
    .catch (error => {
      console.log (error)
    }
    )
}

</script>

<template>
  <button @click="getInfo ()">Get Info</button>
  <button @click="getInfoWithParam ()">Get Info With Param</button>
  <button @click="postInfo ()">Post Info</button>
  <button @click="postInfoWithParam ()">Post Info With Param</button>
</template>

<style scoped></style>

```


多文件，添加拦截器

```js
<script setup>

import instance from './utils/http/axiosInstance';
import { message } from 'ant-design-vue';
function getInfo () {
  //const url = 'https://jsonplaceholder.typicode.com/todos/1'
  instance.get ('/get')
    .then (response => {
      console.log (response.data)
    })
    .catch (error => {
      //console.log (error)
    }
    )
}
function getInfoWithParam () {
  //const url = 'https://jsonplaceholder.typicode.com/todos/1'
  instance.get ('/get', {
    params: {
      id: 1,
      name: 'John'
    }
  })
    .then (response => {
      console.log (response.data)
    })
    .catch (error => {
      //console.log (error)
    }
    )
}

function postInfo () {
  //const url = 'https://jsonplaceholder.typicode.com/todos/1'
  instance.post ('/post')
    .then (response => {
      console.log (response)
    })
    .catch (error => {
      //console.log (error)
    }
    )
}
function postInfoWithParam () {
  //const url = 'https://jsonplaceholder.typicode.com/todos/1'
  instance.post ('/post', {
    id: 1,
    name: 'John'
  })
    .then (response => {
      console.log (response)
    })
    .catch (error => {
      //console.log (error)
    }
    )
}

function delay2s () {
  instance.get ('/delay/2')
    .then (response => {
      console.log (response)
    })
    .catch (error => {
      //console.log (error)
      //message.error (error.message)
    }
  )
}

</script>

<template>
  <button @click="getInfo ()">Get Info</button>
  <button @click="getInfoWithParam ()">Get Info With Param</button>
  <button @click="postInfo ()">Post Info</button>
  <button @click="postInfoWithParam ()">Post Info With Param</button>
  <button @click="delay2s ()">Delay 2s</button>
</template>

<style scoped></style>

```

```js
import axios from 'axios'
import { message } from 'ant-design-vue';
const instance = axios.create ({
  baseURL: 'http://43.139.239.29',
  timeout: 1000,
  headers: {
    'Content-Type': 'application/json'
  }
})

// 添加请求拦截器
instance.interceptors.request.use (function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    console.log (error);
    message.error (' 请求超时，请检查网络连接或联系管理员！');
    return Promise.reject (error);
  });

// 添加响应拦截器
instance.interceptors.response.use (function (response) {
    // 2xx 范围内的状态码都会触发该函数。
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 超出 2xx 范围的状态码都会触发该函数。
    // 对响应错误做点什么
    console.log (error);
    message.error (' 请求失败，请检查网络连接或联系管理员！');
    return Promise.reject (error);
  });

export default instance
```