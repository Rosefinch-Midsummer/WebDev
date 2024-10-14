# Vue-Router 入门

<!-- toc -->

## 5.1. 理解路由

前端系统根据页面路径，跳转到指定组件，展示出指定效果

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1709801835501-9cce06a4-3956-4095-8cb3-8a792dd15026.png)

  

## 5.2. 路由入门

### 5.2.1. 项目创建

```
npm create vite
```

### 5.2.2. 整合 vue-router

参照官网即可

```
npm install vue-router@v4
```


index.ts

```ts
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import ('../views/Home.vue')
  },
  {
    path: '/login',
    name: 'Login',
    component: () => import ('../views/Login.vue')
  },
  {
    path: '/signup',
    name: 'Signup',
    component: () => import ('../views/Signup.vue')
  }
]

const router = createRouter ({
  history: createWebHashHistory (),
  routes
})

export default router
```

App.vue
```vue
<script setup>
import { useRouter } from 'vue-router'
</script>

<template>
  <router-link to="/">Home</router-link> 
  <router-link to="/signup">SignUp</router-link> 
  <router-link to="/login">Login</router-link> 
  <router-view />
</template>

<style scoped>

</style>


```

main.ts

```ts
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import router from './router'

const app = createApp (App)
app.use (router)
app.mount ('#app')


```


### 5.2.3. 运行流程

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1709864949734-bc64bfe7-4dec-4886-a0bb-6ebce5bb9369.png?x-oss-process=image%2Fformat%2Cwebp)

  

### 5.2.4. 路由配置

编写一个新的组件，测试路由功能；步骤如下：

1. 编写 `router/index.js` 文件
2. 配置路由表信息
3. 创建路由器并导出
4. 在 `main.js` 中使用路由
5. 在页面使用 `router-link``router-view` 完成路由功能

### 5.2.5. 路径参数

使用 `: 变量名` 接受动态参数；这个称为路径参数

```js
const routes = [
  // 匹配 /o/3549
  { path: '/o/:orderId' },
  // 匹配 /p/books
  { path: '/p/:productName' },
]
```


App.vue

```js
<script setup>
import { useRouter } from 'vue-router'
</script>

<template>
  <router-link to="/">Home</router-link> 
  <router-link to="/signup/123">SignUp</router-link> 
  <router-link to="/signup/456">SignUp</router-link> 
  <router-link to="/login">Login</router-link> 
  <router-view />
</template>

<style scoped>

</style>

```


index.ts

```ts
import { createRouter, createWebHashHistory } from 'vue-router'

const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import ('../views/Home.vue')
  },
  {
    path: '/login',
    name: 'Login',
    component: () => import ('../views/Login.vue')
  },
  {
    path: '/signup/:id',
    name: 'Signup',
    component: () => import ('../views/Signup.vue')
  }
]

const router = createRouter ({
  history: createWebHashHistory (),
  routes
})

export default router
```



## 5.3. 嵌套路由

![](https://cdn.nlark.com/yuque/0/2024/gif/1613913/1710212693785-79b447e1-b391-4ef5-8cf5-36637a9442c5.gif)

```js
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      {
        // 当 /user/:id/profile 匹配成功
        // UserProfile 将被渲染到 User 的 <router-view> 内部
        path: 'profile',
        component: UserProfile,
      },
      {
        // 当 /user/:id/posts 匹配成功
        // UserPosts 将被渲染到 User 的 <router-view> 内部
        path: 'posts',
        component: UserPosts,
      },
    ],
  },
]
```

  

## 5.4. 编程式

### 5.4.1. useRoute：路由数据

路由传参跳转到指定页面后，页面需要取到传递过来的值，可以使用 `useRoute` 方法；

拿到当前页路由数据；可以做

1. 获取到当前路径
2. 获取到组件名
3. 获取到参数
4. 获取到查询字符串

```js
import {useRoute} from 'vue-router'
const route = useRoute ()
// 打印 query 参数
console.log (route.query)
// 打印 params 参数
console.log (route.params)
```

### 5.4.2. useRouter：路由器

拿到路由器；可以控制跳转、回退等。

```js
import {useRoute, useRouter} from "vue-router";

const router = useRouter ();

// 字符串路径
router.push ('/users/eduardo')

// 带有路径的对象
router.push ({ path: '/users/eduardo' })

// 命名的路由，并加上参数，让路由建立 url
router.push ({ name: 'user', params: { username: 'eduardo' } })

// 带查询参数，结果是 /register?plan=private
router.push ({ path: '/register', query: { plan: 'private' } })

// 带 hash，结果是 /about#team
router.push ({ path: '/about', hash: '#team' })

// 注意： `params` 不能与 `path` 一起使用
router.push ({ path: '/user', params: { username } }) // 错误用法 -> /user
```

  

  

## 5.5. 路由传参

### 5.5.1. params 参数

```js
<!-- 跳转并携带 params 参数（to 的字符串写法） -->
<RouterLink :to="`/news/detail/001 / 新闻 001 / 内容 001`">{{news.title}}</RouterLink>

<!-- 跳转并携带 params 参数（to 的对象写法） -->
<RouterLink 
  :to="{
        name:'xiang', // 用 name 跳转，params 情况下，不可用 path
        params:{
          id:news.id,
          title:news.title,
          content:news.title
        }
  }"
  >
  {{news.title}}
</RouterLink>
```

  

### 5.5.2. query 参数

```js
<!-- 跳转并携带 query 参数（to 的字符串写法） -->
<router-link to="/news/detail?a=1&b=2&content = 欢迎你">
	跳转
</router-link>
				
<!-- 跳转并携带 query 参数（to 的对象写法） -->
<RouterLink 
  :to="{
    //name:'xiang', // 用 name 也可以跳转
    path:'/news/detail',
    query:{
      id:news.id,
      title:news.title,
      content:news.content
    }
  }"
>
  {{news.title}}
</RouterLink>
```

  

## 5.6. 导航守卫

我们只演示全局前置守卫。后置钩子等内容参照官方文档

```js
import {createRouter, createWebHistory} from 'vue-router'
import HomeView from '../views/HomeView.vue'

const router = createRouter ({
    history: createWebHistory (import.meta.env.BASE_URL),
    routes: [
        {
            path: '/',
            name: 'home',
            component: HomeView
        },
        {
            path: '/about',
            name: 'about',
            //route level code-splitting
            //this generates a separate chunk (About.[hash].js) for this route
            //which is lazy-loaded when the route is visited.
            component: () => import ('../views/AboutView.vue')
        },
        {
            path: '/user/:name',
            name: 'User',
            component: () => import ('@/views/user/UserInfo.vue'),
            children: [
                {
                    path: 'profile',
                    component: () => import ('@/views/user/Profile.vue')
                },
                {
                    path: 'posts',
                    component: () => import ('@/views/user/Posts.vue')
                }
            ]
        }
    ]
})

router.beforeEach (async (to, from) => {
    console.log ("守卫：to：", to)
    console.log ("守卫：from：", from)
    if (to.fullPath === '/about') {
       return "/"
    }
})

export default router
```

  

## 5.7. 总结

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1712842500600-cd5098c4-2c44-4932-867b-1db92c5aa2bb.png)
