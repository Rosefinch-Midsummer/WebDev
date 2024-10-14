# Pinia 入门

<!-- toc -->

## 7.1. 核心

**Pinia** 是 Vue 的**存储库**，它允许您**跨组件 / 页面共享状态**。

Pinia 三个核心概念：

- State：表示 Pinia Store 内部保存的数据（data）
- Getter：可以认为是 Store 里面数据的计算属性（computed）
- Actions：是暴露修改数据的几种方式。

**虽然外部也可以直接读写 Pinia Store 中保存的 data，但是我们建议使用 Actions 暴露的方法操作数据更加安全**。

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1712928014360-66fb4f45-471c-443b-be5d-6ca43778f7fe.png)

## 7.2. 整合

```
npm install pinia
```

main.js

```js
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import {createPinia} from 'pinia'

const pinia = createPinia ();

createApp (App)
    .use (pinia)
    .mount ('#app')
```

## 7.3. 案例

`stores/money.js` 编写内容；

```js
import {defineStore} from 'pinia'

// 定义一个 money 存储单元
export const useMoneyStore = defineStore ('money', {
    state: () => ({money: 100}),
    getters: {
        rmb: (state) => state.money,
        usd: (state) => state.money * 0.14,
        eur: (state) => state.money * 0.13,
    },
    actions: {
        win (arg) {
            this.money+=arg;
        },
        pay (arg){
            this.money -= arg;
        }
    },
});
```

App.vue

```vue
<script setup>
import Wallet from "./components/Wallet.vue";
import Game from "./components/Game.vue";
</script>

<template>
  <Wallet></Wallet>
  <Game/>

</template>

<style scoped>

</style>
```


Wallet.vue

```vue
<script setup>
import  {useMoneyStore} from '../stores/money.js'
let moneyStore = useMoneyStore ();

</script>

<template>
<div>
  <h2>￥：{{moneyStore.rmb}}</h2>
  <h2>$：{{moneyStore.usd}}</h2>
  <h2>€：{{moneyStore.eur}}</h2>
</div>
</template>

<style scoped>
div {
  background-color: #f9f9f9;
}
</style>
```

  
Game.vue

```vue
<script setup>
import  {useMoneyStore} from '../stores/money.js'

let moneyStore = useMoneyStore ();
function guaguale (){
  moneyStore.win (100);
}

function bangbang (){
  moneyStore.pay (5)
}
</script>

<template>
  <button @click="guaguale"> 刮刮乐 </button>
  <button @click="bangbang"> 买棒棒糖 </button>
</template>

<style scoped>

</style>
```

## 7.4. setup 写法

```js
export const useMoneyStore = defineStore ('money', () => {
    const salary = ref (1000); //ref () 就是 state 属性
    const dollar = computed (() => salary.value * 0.14); //  computed () 就是 getters
    const eur = computed (() => salary.value * 0.13); //computed () 就是 getters

    //function () 就是 actions
    const pay = () => {
        salary.value -= 100;
    }

    const win = () => {
        salary.value += 1000;
    }

    // 重要：返回可用对象
    return {salary,dollar,eur,pay,win}
})
```

完整代码：

```js
import {defineStore} from 'pinia'

// 定义一个 money 存储单元
//export const useMoneyStore = defineStore ('money', {
//     state: () => ({money: 100}),
//     getters: {
//         rmb: (state) => state.money,
//         usd: (state) => state.money * 0.14,
//         eur: (state) => state.money * 0.13,
//     },
//     actions: {
//         win (arg) {
//             this.money+=arg;
//         },
//         pay (arg){
//             this.money -= arg;
//         }
//     },
// });
export const useMoneyStore = defineStore ('money',() => {
    const money = ref (100);
    const rmb = computed (() => money.value);
    const usd = computed (() => money.value * 0.14);
    const eur = computed (() => money.value * 0.13);
    const win = (arg) => {
        money.value += arg;
    };
    const pay = (arg) => {
        money.value -= arg;
    };
    return {
        money,
        rmb,
        usd,
        eur,
        win,
        pay
    };
});
```
