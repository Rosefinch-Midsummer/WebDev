# Ant Design Vue

<!-- toc -->

## 官网

官网：[https://www.antdv.com/](https://www.antdv.com/)

使用 `npm create vue@latest` 创建出项目脚手架，然后整合 `ant design vue`

## 9.1. 整合

安装依赖

```
npm i --save ant-design-vue@4.x
```


全局注册： 编写 `main.js`

```js
import { createApp } from 'vue';
import Antd from 'ant-design-vue';
import App from './App';
import 'ant-design-vue/dist/reset.css';

const app = createApp (App);

app.use (Antd).mount ('#app');
```

使用图标需要执行 `npm install --save @ant-design/icons-vue`

```js
<template>
  <a-menu v-model:selectedKeys="current" mode="horizontal" :items="items" />
</template>
<script setup>
import { h, ref } from 'vue';
import { MailOutlined, AppstoreOutlined, SettingOutlined } from '@ant-design/icons-vue';
const current = ref (['mail']);
const items = ref ([
  {
    key: 'mail',
    icon: () => h (MailOutlined),
    label: 'Navigation One',
    title: 'Navigation One',
  },
  {
    key: 'app',
    icon: () => h (AppstoreOutlined),
    label: 'Navigation Two',
    title: 'Navigation Two',
  },
  {
    key: 'sub1',
    icon: () => h (SettingOutlined),
    label: 'Navigation Three - Submenu',
    title: 'Navigation Three - Submenu',
    children: [
      {
        type: 'group',
        label: 'Item 1',
        children: [
          {
            label: 'Option 1',
            key: 'setting:1',
          },
          {
            label: 'Option 2',
            key: 'setting:2',
          },
        ],
      },
      {
        type: 'group',
        label: 'Item 2',
        children: [
          {
            label: 'Option 3',
            key: 'setting:3',
          },
          {
            label: 'Option 4',
            key: 'setting:4',
          },
        ],
      },
    ],
  },
  {
    key: 'alipay',
    label: h (
      'a',
      {
        href: 'https://antdv.com',
        target: '_blank',
      },
      'Navigation Four - Link',
    ),
    title: 'Navigation Four - Link',
  },
]);
</script>

<style scoped></style>

```


## 9.2. 布局

- **Layout：布局容器**，其下可**嵌套 Header Sider Content Footer 或 Layout 本身**，可以**放在任何父容器中**。

- Header：顶部布局，**自带默认样式**，其下可嵌套任何元素，只能放在 Layout 中。
- Sider：侧边栏，**自带默认样式及基本功能**，其下可嵌套任何元素，只能放在 Layout 中。
- Content：内容部分，**自带默认样式**，其下可嵌套任何元素，只能放在 Layout 中。
- Footer：底部布局，**自带默认样式**，其下可嵌套任何元素，只能放在 Layout 中。


一个典型的后台管理系统布局

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1710830318280-23e4cd6a-a516-4ac2-9711-3076987b3eaa.png)


```vue
<template>
  <a-layout id="components-layout-demo" style="height: 100vh">
    <a-layout-sider v-model:collapsed="collapsed" :trigger="null" collapsible>
      <div class="logo" />
      <a-menu v-model:selectedKeys="selectedKeys" theme="dark" mode="inline">
        <a-menu-item key="1">
          <user-outlined />
          <span>nav 1</span>
        </a-menu-item>
        <a-menu-item key="2">
          <video-camera-outlined />
          <span>nav 2</span>
        </a-menu-item>
        <a-menu-item key="3">
          <upload-outlined />
          <span>nav 3</span>
        </a-menu-item>
      </a-menu>
    </a-layout-sider>
    <a-layout>
      <a-layout-header style="background: #fff; padding: 0">
        <menu-unfold-outlined
            v-if="collapsed"
            class="trigger"
            @click="() => (collapsed = !collapsed)"
        />
        <menu-fold-outlined v-else class="trigger" @click="() => (collapsed = !collapsed)" />
      </a-layout-header>
      <a-layout-content
          :style="{ margin: '24px 16px', padding: '24px', background: '#fff', minHeight: '280px' }"
      >
        Content
      </a-layout-content>
    </a-layout>
  </a-layout>
</template>
<script setup>
import { ref } from 'vue';
const selectedKeys = ref (['1']);
const collapsed = ref (false);
</script>
<style>
#components-layout-demo .trigger {
  font-size: 18px;
  line-height: 64px;
  padding: 0 24px;
  cursor: pointer;
  transition: color 0.3s;
}

#components-layout-demo .trigger:hover {
  color: #1890ff;
}

#components-layout-demo .logo {
  height: 32px;
  background: rgba (255, 255, 255, 0.3);
  margin: 16px;
}

.site-layout .site-layout-background {
  background: #fff;
}
</style>
```


## 9.3. 组件

参照官网使用即可