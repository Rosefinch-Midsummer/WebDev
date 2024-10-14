# Vue3 入门

<!-- toc -->

## vite 创建项目

我们使用 vite 创建 vue 项目脚手架，并测试 Vue 功能

```shell
npm create vite #根据向导选择技术栈
```

## 4.1. 组件化

组件系统是一个抽象的概念；

- 组件：小型、独立、可复用的单元
- 组合：通过组件之间的组合、包含关系构建出一个完整应用

几乎任意类型的应用界面都可以抽象为一个组件树；

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1708754538888-ced4be1b-71b9-4238-9398-372985b1b99a.png?x-oss-process=image%2Fformat%2Cwebp)

## 4.2. SFC

Vue 的**单文件组件** (即 \*.vue 文件，英文 Single-File Component，简称 **SFC**) 是一种特殊的文件格式，使我们能够将一个 Vue 组件的模板、逻辑与样式封装在单个文件中.

```vue
<script setup>
  // 编写脚本
</script>

<template>
  // 编写页面模板
</template>

<style scoped>
  // 编写样式
</style>
```

## 4.3. Vue 工程

### 4.3.1. 创建 & 运行

```
npm create vite  # 按照提示选择 Vue
npm run dev #项目运行命令
```

### 4.3.2. 运行原理

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1712477454104-1234f405-1fdf-457c-8b84-a4cae4f4645e.png)

## 应用实例 [​](https://cn.vuejs.org/guide/essentials/application.html#the-application-instance)

每个 Vue 应用都是通过 [`createApp`](https://cn.vuejs.org/api/application.html#createapp) 函数创建一个新的 **应用实例**：

```js
import { createApp } from 'vue'

const app = createApp ({
  /* 根组件选项 */
})
```

## 根组件 [​](https://cn.vuejs.org/guide/essentials/application.html#the-root-component)

我们传入 `createApp` 的对象实际上是一个组件，每个应用都需要一个 “根组件”，其他组件将作为其子组件。

如果你使用的是单文件组件，我们可以直接从另一个文件中导入根组件。

```js
import { createApp } from 'vue'
// 从一个单文件组件中导入根组件
import App from './App.vue'

const app = createApp (App)
```

虽然本指南中的许多示例只需要一个组件，但大多数真实的应用都是由一棵嵌套的、可重用的组件树组成的。例如，一个待办事项 (Todos) 应用的组件树可能是这样的：

```
App (root component)
├─ TodoList
│  └─ TodoItem
│     ├─ TodoDeleteButton
│     └─ TodoEditButton
└─ TodoFooter
   ├─ TodoClearButton
   └─ TodoStatistics
```

我们会在指南的后续章节中讨论如何定义和组合多个组件。在那之前，我们得先关注一个组件内到底发生了什么。

## 挂载应用 [​](https://cn.vuejs.org/guide/essentials/application.html#mounting-the-app)

应用实例必须在调用了 `.mount ()` 方法后才会渲染出来。该方法接收一个 “容器” 参数，可以是一个实际的 DOM 元素或是一个 CSS 选择器字符串：


```html
<div id="app"></div>
```


```js
app.mount ('#app')
```

应用根组件的内容将会被渲染在容器元素里面。容器元素自己将**不会**被视为应用的一部分。

`.mount ()` 方法应该始终在整个应用配置和资源注册完成后被调用。同时请注意，不同于其他资源注册方法，它的返回值是根组件实例而非应用实例。

### DOM 中的根组件模板 [​](https://cn.vuejs.org/guide/essentials/application.html#in-dom-root-component-template)

根组件的模板通常是组件本身的一部分，但也可以直接通过在挂载容器内编写模板来单独提供：


```html
<div id="app">
  <button @click="count++">{{ count }}</button>
</div>
```


```js
import { createApp } from 'vue'

const app = createApp ({
  data () {
    return {
      count: 0
    }
  }
})

app.mount ('#app')
```

当根组件没有设置 `template` 选项时，Vue 将自动使用容器的 `innerHTML` 作为模板。

DOM 内模板通常用于 [无构建步骤](https://cn.vuejs.org/guide/quick-start.html#using-vue-from-cdn) 的 Vue 应用程序。它们也可以与服务器端框架一起使用，其中根模板可能是由服务器动态生成的。

## 4.4. 基础使用

### 4.4.1. 插值

```vue
<script setup>
// 基本数据
let name = "张三"
let age = 18

// 对象数据
let car = {
  brand: "奔驰",
  price: 777
}
</script>

<template>
  <p> name: {{name}} </p>
  <p> age: {{age}} </p>
  <div style="border: 3px solid red">
    <p > 品牌：{{car.brand}}</p>
    <p > 价格：{{car.price}}</p>
  </div>
</template>

<style scoped>

</style>vue
```

注意：没有 setup 则不显示 name 等属性值

在 Vue 3 中，`<script setup>` 是一种新的语法糖，用于简化组件的编写和逻辑的组织。它可以让开发者在组件中使用函数式组件的形式编写组件逻辑，同时也能让开发者在组件中使用 Composition API 来编写逻辑。

通过 `<script setup>` 语法，开发者可以将组件中的 props、state 和方法等都放在一个 setup 函数中进行定义，从而避免了显式地在组件中引入这些内容。这样可以使组件更加清晰简洁，并且可以更好地组织和重用逻辑代码。

总的来说，`<script setup>` 的作用是简化组件的编写和逻辑的组织，让开发者更加方便地使用 Vue 的 Composition API 来编写逻辑。

### 4.4.2. 指令

#### 4.4.2.1. 事件绑定：v-on

使用 `v-on` 指令，可以为元素绑定事件。可以简写为 `@`

```vue
<script setup>
// 定义事件回调
function buy (){
  alert ("购买成功");
}
</script>
<template>
  <button v-on:click="buy"> 购买 </button>
  <button @click="buy"> 购买 </button>
</template>
<style scoped>
</style>
```

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1708763498360-bc7a713e-b02a-4b17-abd9-bb1a6371bf50.png)

Modifiers：修饰符详情

[https://cn.vuejs.org/guide/essentials/event-handling.html](https://cn.vuejs.org/guide/essentials/event-handling.html)

#### 4.4.2.2. 条件判断：v-if

  

#### 4.4.2.3. 循环：v-for

  

#### 4.4.2.4. 更多指令

[https://cn.vuejs.org/api/built-in-directives.html](https://cn.vuejs.org/api/built-in-directives.html)；后期我们都会用到。

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1709712996927-3af836de-8e74-45b9-8757-9fb7972c1963.png)

  

  

### 4.4.3. 属性绑定

- 使用 `v-bind: 属性 ='xx'` 语法，可以为标签的某个属性绑定值；
- 可以简写为 `: 属性 ='xx'`

```vue
<script setup>
  //
  let url = "http://www.baidu.com"
</script>

<template>
  <a v-bind:href="url">go</a>
  <a :href="url">go</a>
</template>

  <style scoped>

</style>
```

  

注意：此时如果我们想修改变量的值，属性值并不会跟着修改。可以测试下。因为 v-bind 默认设置还没有响应式特性

  

  

  

  

  

  

### 4.4.4. 响应式 - ref ()

数据的动态变化需要反馈到页面；

Vue 通过 `ref ()` 和 `reactive ()` 包装数据，将会生成一个数据的代理对象。vue 内部的 **基于依赖追踪的响应式系统** 就会**追踪**感知**数据变化**，并**触发页面**的重新**渲染**。

#### 4.4.4.1. 使用方式

使用步骤：

1. 使用 **ref ()** 包装**原始类型、对象类型数据**，生成 **代理对象**
2. **任何方法、js 代码中**，使用 `代理对象.value` 的形式读取和修改值
3. **页面组件中**，直接使用 `代理对象`

_**注意：**__推荐使用 const（常量） 声明代理对象。代表代理对象不可变，但是内部值变化会被追踪。_

```js
<script setup>
import { ref } from 'vue'

const count = ref (0)

function increment () {
  count.value++
}
</script>

<template>
  <button @click="increment">
    {{ count }}
  </button>
</template>
```

#### 4.4.4.2. 深层响应性

```js
import { ref } from 'vue'

const obj = ref ({
  nested: { count: 0 },
  arr: ['foo', 'bar']
})

function mutateDeeply () {
  // 以下都会按照期望工作
  obj.value.nested.count++
  obj.value.arr.push ('baz')
}
```

### 4.4.5. 响应式 - reactive ()

使用步骤：

1. 使用 **reactive ()** 包装**对象类型数据**，生成 **代理对象**
2. **任何方法、js 代码中**，使用 `代理对象。属性` 的形式读取和修改值
3. **页面组件中**，直接使用 `代理对象。属性`

```vue
import { reactive } from 'vue'

const state = reactive ({ count: 0 })

<button @click="state.count++">
{{ state.count }}
</button>
```

注意：可以修改 const 变量的内容

基本类型用 ref ()，对象类型用 reactive ()，`ref 要用 .value`，`reactive 直接 .` 。页面取值永不变。

也可以 ref 一把梭，大不了天天 .value


```vue
<script setup>

import { reactive, ref } from 'vue';

//let name = 'Base2'
let name = "张三"
let age = 18
let car = reactive ({ brand: 'BMW', model: 'X5', price: 400 })
let message = '<p style="color:red">Hello Vue3</p>'
function changeName () {
    name = "李四"
    alert ('changeName')
}

let fruits = ['apple', 'banana', 'orange']

let url = ref ('https://www.baidu.com')

function changeUrl () {
    url.value = 'https://www.google.com'
}

function addPrice () {
    car.price += 100
}

</script>
<template>

    <!-- <a :href="url" :abc="url"> 百度 </a><br> -->
    <a v-bind:href="url">GO {{ url }}</a>
    <button @click="changeUrl">Change Url</button>

    <h1>Hello {{ name }}</h1>
    <p> {{ age }} component </p>
    <p> {{ car.brand }} {{ car.model }} </p>

    <p > 价格：{{ car.price }}</p>

    <button @click="addPrice"> 加价 </button>

    <div v-html="message"></div>
    <div>{{ message }}</div>
    <div v-text="message"></div>


    <!-- <button @click="changeName">Change Name</button> -->
    <button v-on:click.once="changeName">Change Name</button>

    <p style="color:blue" v-if="car.price < 1000"> 很便宜 </p>

    <p style="color: red;" v-else > 太贵了 </p>

    <ul>
        <li v-for="(fruit, index) in fruits" :key="index">{{ fruit }}</li>
    </ul>

</template>
<style></style>
```

### 4.4.6. 表单绑定

```
//2. 指令：v-XXX：
// 基础指令：v-html、v-text
// 事件指令：v-on
// 判断指令：v-if
// 循环指令：v-for
/ 属性绑定：v-bind; 单向绑定，数据 -> 页面
// 表单绑定：v-model; 双向绑定，数据 <--> 页面
```

复制如下模板到组件，编写你的代码，实现表单数据绑定。

```js
<div style="display: flex;">
  <div style="border: 1px solid black;width: 300px">
    <form>
      <h1 > 表单绑定 </h1>
      <p style="background-color: azure"><label > 姓名 (文本框)：</label><input/></p>
      <p style="background-color: azure"><label > 同意协议 (checkbox)：</label>
        <input type="checkbox"/>
      </p>
      <p style="background-color: azure">
        <label > 兴趣 (多选框)：</label><br/>
        <label><input type="checkbox" value="足球"/> 足球 </label>
        <label><input type="checkbox" value="篮球"/> 篮球 </label>
        <label><input type="checkbox" value="羽毛球"/> 羽毛球 </label>
        <label><input type="checkbox" value="乒乓球"/> 乒乓球 </label>
      </p>
      <p style="background-color: azure">
        <label > 性别 (单选框)：</label>
        <label><input type="radio" name="sex" value="男"> 男 </label>
        <label><input type="radio" name="sex" value="女"> 女 </label>
      </p>
      <p style="background-color: azure">
        <label > 学历 (单选下拉列表)：</label>
        <select>
          <option disabled value=""> 选择学历 </option>
          <option > 小学 </option>
          <option > 初中 </option>
          <option > 高中 </option>
          <option > 大学 </option>
        </select>
      </p>
      <p style="background-color: azure">
        <label > 课程 (多选下拉列表)：</label>
        <br/>
        <select multiple>
          <option disabled value=""> 选择课程 </option>
          <option > 语文 </option>
          <option > 数学 </option>
          <option > 英语 </option>
          <option > 道法 </option>
        </select>
      </p>
    </form>
  </div>
  <div style="border: 1px solid blue;width: 200px">
    <h1 > 结果预览 </h1>
    <p style="background-color: azure"><label > 姓名：</label></p>
    <p style="background-color: azure"><label > 同意协议：</label>
    </p>
    <p style="background-color: azure">
      <label > 兴趣：</label>
    </p>
    <p style="background-color: azure">
      <label > 性别：</label>
    </p>
    <p style="background-color: azure">
      <label > 学历：</label>
    </p>
    <p style="background-color: azure">
      <label > 课程：</label>
    </p>
  </div>
</div>
```

完整代码如下所示：

```vue
<script setup>

import { reactive, ref } from 'vue';

const data = reactive ({
    name: '',
    agree: false,
    hobby: [],
    sex: '',
    education: '',
    course: []
});

</script>
<template>

<div style="display: flex;">
  <div style="border: 1px solid black;width: 300px">
    <form>
      <h1 > 表单绑定 </h1>
      <p style="background-color: azure"><label > 姓名 (文本框)：</label><input v-model="data.name"/></p>
      <p style="background-color: azure"><label > 同意协议 (checkbox)：</label>
        <input type="checkbox" v-model="data.agree"/>
      </p>
      <p style="background-color: azure">
        <label > 兴趣 (多选框)：</label><br/>
        <label><input type="checkbox" value="足球" v-model="data.hobby"/> 足球 </label>
        <label><input type="checkbox" value="篮球" v-model="data.hobby"/> 篮球 </label>
        <label><input type="checkbox" value="乒乓球" v-model="data.hobby"/> 乒乓球 </label>
      </p>
      <p style="background-color: azure">
        <label > 性别 (单选框)：</label>
        <label><input type="radio" name="sex" value="男" v-model="data.sex"> 男 </label>
        <label><input type="radio" name="sex" value="女" v-model="data.sex"> 女 </label>
      </p>
      <p style="background-color: azure">
        <label > 学历 (单选下拉列表)：</label>
        <select v-model="data.education">
          <option disabled value=""> 选择学历 </option>
          <option > 小学 </option>
          <option > 初中 </option>
          <option > 高中 </option>
          <option > 大学 </option>
        </select>
      </p>
      <p style="background-color: azure">
        <label > 课程 (多选下拉列表)：</label>
        <br/>
        <select multiple v-model="data.course">
          <option disabled value=""> 选择课程 </option>
          <option > 语文 </option>
          <option > 数学 </option>
          <option > 英语 </option>
          <option > 道法 </option>
        </select>
      </p>
    </form>
  </div>
  <div style="border: 1px solid blue;width: 200px">
    <h1 > 结果预览 </h1>
    <p style="background-color: azure"><label > 姓名：{{ data.name }}</label></p>
    <p style="background-color: azure"><label > 同意协议：{{ data.agree }}</label>
    </p>
    <p style="background-color: azure">
      <label > 兴趣：{{ data.hobby }}</label>
    </p>
    <p style="background-color: azure">
      <label > 性别：{{ data.sex }}</label>
    </p>
    <p style="background-color: azure">
      <label > 学历：{{ data.education }}</label>
    </p>
    <p style="background-color: azure">
      <label > 课程：{{ data.course }}</label>
    </p>
  </div>
</div>

</template>
<style></style>
```
### 4.4.7. 计算属性 - computed

计算属性：根据已有数据计算出新数据

```vue
<script setup>
import { ref, computed } from "vue";

// 省略基础代码

const car = ref ({
    brand: "奔驰",
    num: 1,
    price: 100
})

const totalPrice = computed (() => {
    return car.value.price * car.value.num
})

</script>

<template>
    <h2 > 基础信息 </h2>
    <h2 > 品牌：{{ car.brand }}</h2>
    <h2 > 数量：{{ car.num }}</h2>
    <h2 > 单价：{{ car.price }}</h2>
    <h1 > 总价：{{ totalPrice }}</h1>
    <button @click="car.num++"> 加量 </button>
    <button @click="car.price++"> 加价 </button>

</template>

<style scoped></style>
```

  

## 4.5. 进阶用法

### 4.5.1. 监听 - watch&watchEffect

```js
watch (num, (value, oldValue, onCleanup) => {
  console.log ("newValue：" + value + "；oldValue:" + oldValue)
  onCleanup (() => {
    console.log ("onCleanup....")
  })
})
```

```vue
<script setup>
import { ref, computed, watch } from "vue";

// 省略基础代码

const car = ref ({
    brand: "奔驰",
    num: 1,
    price: 100
})

const totalPrice = computed (() => {
    return car.value.price * car.value.num
})
watch (() => car.value.num, (value, oldValue) => {  
    console.log ("value:", value, "oldValue:", oldValue)  
    if (car.value.num > 3) {  
        alert ("数量不能超过 3")  
        car.value.num = 3  
    }  
})
//watch (() => car.value.num, (value, oldValue) => {
//     console.log ("value:", value, "oldValue:", oldValue)
//     if (car.num.value > 3) {
//         alert ("数量不能超过 3")
//         car.num.value = 3
//     }
// })

</script>

<template>
    <h2 > 基础信息 </h2>
    <h2 > 品牌：{{ car.brand }}</h2>
    <h2 > 数量：{{ car.num }}</h2>
    <h2 > 单价：{{ car.price }}</h2>
    <h1 > 总价：{{ totalPrice }}</h1>
    <button @click="car.num++"> 加量 </button>
    <button @click="car.price++"> 加价 </button>

</template>

<style scoped></style>
```

这两个 watch 函数的区别在于监听的属性不同。

第一个 watch 函数中的监听属性是 `car.value.num`，而第二个 watch 函数中的监听属性是 `car.num.value`。

在 Vue3 的 Composition API 中，`ref` 创建的响应式对象可以直接通过 `.value` 访问其值，而使用 `computed` 等函数创建的响应式对象则不能直接访问其值，需要通过 `.value` 来获取。因此，正确的监听属性应该是 `car.value.num`。

watchEffect 函数监听所有响应式数据

```vue
<script setup>
import { ref, computed, watch, watchEffect } from "vue";

// 省略基础代码

const car = ref ({
    brand: "奔驰",
    num: 1,
    price: 100
})

const totalPrice = computed (() => {
    return car.value.price * car.value.num
})

watchEffect (() => {
    if (car.value.num > 3){
        alert ("数量不能超过 3")
        car.value.num = 3
    }
    if (car.value.price > 10000){
        alert ("单价不能超过 10000")
        car.value.price = 10000
    }
})

</script>

<template>
    <h2 > 基础信息 </h2>
    <h2 > 品牌：{{ car.brand }}</h2>
    <h2 > 数量：{{ car.num }}</h2>
    <h2 > 单价：{{ car.price }}</h2>
    <h1 > 总价：{{ totalPrice }}</h1>
    <button @click="car.num++"> 加量 </button>
    <button @click="car.price++"> 加价 </button>

</template>

<style scoped></style>
```

### 4.5.2. 生命周期

每个 Vue 组件实例在创建时都需要经历一系列的初始化步骤，比如设置好数据侦听，编译模板，挂载实例到 DOM，以及在数据改变时更新 DOM。在此过程中，它也会运行被称为生命周期钩子的函数，让开发者有机会在特定阶段运行自己的代码。

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1712714322676-1b2be398-47b7-4901-ab8d-f5dff873a8ab.png)

生命周期整体分为四个阶段，分别是：`创建、挂载、更新、销毁`，每个阶段都有两个钩子，一前一后。

常用的钩子：

- **onMounted (挂载完毕)**
- **onUpdated (更新完毕)**
- **onBeforeUnmount (卸载之前)**

```vue
<script setup>
import {onBeforeMount, onBeforeUpdate, onMounted, onUpdated, ref} from "vue";
const  count = ref (0)
const btn01 = ref ()

// 生命周期钩子
onBeforeMount (()=>{
  console.log (' 挂载之前 ',count.value,document.getElementById ("btn01"))
})
onMounted (()=>{
  console.log (' 挂载完毕 ',count.value,document.getElementById ("btn01"))
})
onBeforeUpdate (()=>{
  console.log (' 更新之前 ',count.value,btn01.value.innerHTML)
})
onUpdated (()=>{
  console.log (' 更新完毕 ',count.value,btn01.value.innerHTML)
})
</script>

<template>
  <button ref="btn01" @click="count++"> {{count}} </button>
</template>

<style scoped>
</style>
```

输出结果：

```
baked beans
Life.vue:8 挂载之前 0 null
Life.vue:11 挂载完毕 0 null
Life.vue:14 更新之前 1 0
Life.vue:17 更新完毕 1 1
```

更新前内容未变，数据变了


### 4.5.3. 组件传值

#### 4.5.3.1. 父传子 - Props

**_父组件给子组件传递值；_**

```vue
<script setup>
import { ref, reactive } from 'vue';
import Son from './Son.vue'

let money = ref (1000)

const data = reactive ({
    money: money,
    name: 'Father',
    age: 30,
    hobby: 'coding'
})

</script>

<template>
    <div>Father</div>
    <Son v-bind="data" />
</template>

<style scoped></style>
```

```vue
<script setup>

let props = defineProps (['money', "name", "hobby"]);

</script>

<template>
    <div>Son 组件 </div>
    <p style="color: aqua;"> 父组件传递的 money 值：{{ props.money }}</p>
    <p style="color: aqua;"> 父组件传递的 name 值：{{ props.name }}</p>
    <p style="color: aqua;"> 父组件传递的 hobby 值：{{ props.hobby }}</p>
</template>

<style scoped></style>
```

**单向数据流**效果：

- 父组件修改值，子组件发生变化
- 子组件修改值，父组件不会感知到

```vue
// 父组件给子组件传递数据：使用属性绑定
<Son :books="data.books" :money="data.money"/>
  
// 子组件定义接受父组件的属性
let props = defineProps ({
  money: {
    type: Number,
    required: true,
    default: 200
  },
  books: Array
});
```


#### 4.5.3.2. 子传父 - Emit

props 用来父传子，emit 用来子传父

```vue
// 子组件定义发生的事件
let emits = defineEmits (['buy']);
function buy (){
  //props.money -= 5;
  emits ('buy',-5);
}

// 父组件感知事件和接受事件值
  <Son :books="data.books" :money="data.money"
       @buy="moneyMinis"/>
```

```js
<script setup>
import { ref, reactive } from 'vue';
import Son from './Son.vue'

let money = ref (1000)

const data = reactive ({
    money: money,
    name: 'Father',
    age: 30,
    hobby: 'coding'
})

function addMoney () {
    money.value += 100
}

function minusMoney (arg) {
    money.value += arg
}

</script>

<template>
    <div>Father</div>
    <div>Money: {{ money }}</div>
    <button @click="addMoney">Add Money</button>
    <div ><Son v-bind="data" style="color: red;" @buy="minusMoney" /></div>
</template>

<style scoped></style>
```

```js
<script setup>

let props = defineProps (['money', "name", "hobby"]);
let emits = defineEmits (['buy']);
function buy () {
    //props.money -= 5;
    emits ('buy', -5);
}


</script>

<template>
    <div>
        <p style="color: aqua;"> 父组件传递的 money 值：{{ props.money }}</p>
        <button @click="buy"> 购买 </button>

        <p style="color: aqua;"> 父组件传递的 name 值：{{ props.name }}</p>
        <p style="color: aqua;"> 父组件传递的 hobby 值：{{ props.hobby }}</p>
    </div>

</template>

<style scoped></style>
```


思考：兄弟组件如何传值？

利用父子传值即可

### 4.5.4. 插槽 - Slots

子组件可以使用插槽接受模板内容。

#### 4.5.4.1. 基本使用

```js
<!-- 组件定义 -->
<button class="fancy-btn">
  <slot></slot> <!-- 插槽出口 -->
</button>

<!-- 组件使用 -->
<FancyButton>
  Click me! <!-- 插槽内容 -->
</FancyButton>
```

  

#### 4.5.4.2. 默认内容

```js
<button type="submit">
  <slot>
    Submit <!-- 默认内容 -->
  </slot>
</button>
```

  

#### 4.5.4.3. 具名插槽


```js
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

  

使用： `v-slot` 可以简写为 `#`

```js
<BaseLayout>
  <template v-slot:header>
    <!-- header 插槽的内容放这里 -->
  </template>
</BaseLayout>
```

  

  

## 4.6. 总结

![](https://cdn.nlark.com/yuque/0/2024/png/1613913/1712737404128-2d94429f-b301-408e-ac3a-88e175b1d45f.png)

几个简写：

- `v-on`=`@`
- `v-bind`= `:`
- `v-slot`= `#`