# Vue.js学习

## 学习前准备

1. 安装Node.js，且版本大于15.0.0
   1. 下载安装包：https://nodejs.org/zh-cn/download/
   2. 查看版本命令：`node -v` 
   3. 升级npm命令：`npm install -g npm`
2. 初始化环境
   1. 在命令提示符中，cd到指定目录内
   2. 输入命令：`npm init vue@latest`，此命令将会下载 `create-vue` 包并开始部署环境
   3. 按照提示输入项目名称、版本、描述、作者、许可证等信息，完成项目初始化
      1. 请输入项目名称：`Vue.js学习`【注意：名字中不能存在大写】
      2. 是否使用TypeScript语法？`No`
      3. 是否启用jsx支持？`No`
      4. 是否引入 Vue Router 进行单页面应用开发？`No`
      5. 是否引入 Pinia 用于状态管理？`No`
      6. 是否引入 Vitest 用于单元测试？`No`
      7. 是否要引入一款端到端（End to End）测试工具？`No`
      8. 是否引入 ESLint 用于代码质量检测？`No`
      9. 是否引入 Vue DevTools 7 扩展用于调试? (试验阶段)`No`
      ``` bash
      正在初始化项目 F:\Programing-Programs\SortByProjects\VueStudy01\SourceCode\vue-study-001...

      项目初始化完成，可执行以下命令：

      cd vue-study-001
      npm install
      npm run dev
      ```
3. VSCode安装插件
   1. 安装Vue插件：`vue - official`
   
## Vue项目目录结构

安装完成后可以看见以下目录结构：

```
├── .vscode   ---(VSCode相关配置)
│   └── extensions.json
├── node_modules  ---(Vue项目的运行依赖文件)
│   └── ...
├── public  ---(静态资源)
│   ├── favicon.ico
│   └── index.html
├── src  ---(源码)
│   ├── assets
│   ├── components
│   ├── App.vue
│   └── main.js
├── .gitignore    ---(git忽略文件)
├── index.html    ---(入口文件)
├── jsconfig.json    ---(TypeScript配置)
├── package-lock.json   ---(依赖包锁定文件)
├── package.json  ---(信息描述文件)
├── README.md  ---(项目说明文件)
└── vite.config.js   ---(Vue项目的配置文件)
```

## Vue基础语法

### 1 Vue模板语法

#### 1.1 直接插入数据

``` js
<template>
    <h3>模板语法</h3>
    <p>{{ msg }}</p>
</template>

<script>
export default {
    data() {
        return {
            msg: "Hello World!"
        }
    }
}
</script>
```

#### 1.2 使用JavaScript表达式

每个绑定仅支持单一表达式，不能包含多个表达式。
也就是一段能够被求值的JavaScript代码。
（一个简单的判定方法就是，能不能被写在return后面）

``` js
<template>
    <h3>JavaScript表达式</h3>
    <p>{{ 1 + 2 }}</p>
    <p>{{ true && false }}</p>
    <p>{{ message.split('').reverse().join('') }}</p>
</template>


<script>
export default {
    data() {
        return {
            message: "Hello World!"
        }
    }
}
</script>
```

无效内容：

``` js
<p>{{ var a = 1 }}</p>
<p>{{ if(true) "true" else "false" }}</p>
```

#### 1.3 插入纯HTML

双大括号将会将数据插值为纯文本，而不是HTML。
插入纯HTML代码，需要使用`v-html`指令。

``` js
<template>
    <h3>模板语法</h3>
    <p>{{ binglink }}</p>
    <p v-html="binglink"></p>

</template>


<script>
export default {
    data() {
        return {
            binglink: "<a href='https://www.bing.com/'>Bing</a>"
        }
    }
}
</script>
```

### 2 属性绑定

#### 2.1 `v-bind`指令

双大括号不能在HTML attribute中使用，想要响应式地绑定属性，需要使用`v-bind`指令。

``` js
<template>
    <h3>属性绑定</h3>
    <div v-bind:id="dynamicId" v-bind:class="dynamicClass">测试属性绑定</div>
    /* 等价于以下 */ 
    /* <div id="test" class="active">测试属性绑定</div> */
</template>


<script>
export default {
  data() {
    return {
      dynamicClass: 'active',
      dynamicId: 'test'
    }
  }
}
</script>
```

#### 2.2 简化

##### 2.2.1 布尔值Attribute

若定义的值为 null 或 undefined，则该 attribute 不会被渲染。

以上例子中，若 `dynamicId` 为 null 或 undefined，则 `id` attribute 不会被渲染，等价于：

``` html
<div class="active">测试属性绑定</div>
```

##### 2.2.2 冒号简写

又由于 `v-bind` 指令非常常用，所以 Vue 提供了一种简写方式：

``` html
<div :id="dynamicId" :class="dynamicClass">测试属性绑定</div>
```

等价于：

``` html
<div v-bind:id="dynamicId" v-bind:class="dynamicClass">测试属性绑定</div>
```

##### 2.2.3 动态绑定多个值

``` js
<template>
    <h3>属性绑定</h3>
    <div v-bind="objectOfAttr">测试属性绑定</div>
    /* 等价于以下 */ 
    /* <div v-bind:id="dynamicId" v-bind:class="dynamicClass">测试属性绑定</div> */
</template>


<script>
export default {
  data() {
    return {
      dynamicClass: 'active',
      dynamicId: 'test',
      objectOfAttr: {
        id: 'test',
        class: 'active'
      }
    }
  }
}
</script>
```

### 3 条件渲染

在Vue中，提供了条件渲染，这类似于Javascript中的条件语句：

- `v-if`：条件为真时渲染，条件为假时销毁
- `v-else`：`v-if`的反向条件渲染
- `v-else-if`：多重条件渲染
- `v-show`：条件为真时渲染，条件为假时隐藏

#### 3.1 `v-if` `v-else` `v-else-if`

`v-if`指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回值为真时被渲染。

``` js
<template>
    <h3>条件渲染</h3>
    <div v-if="flag">你能看见我么</div>
    <div v-else>那你还是看我吧</div>

    <div v-if="type === 'A'">A</div>
    <div v-else-if="type === 'B'">B</div>
    <div v-else-if="type === 'C'">C</div>
    <div v-else>其他字母</div>
</template>

<script>

export default {
    data(){
        return {
            flag: false,
            type: "x"
        }
    }
}

</script>
```

#### 3.2 `v-show`

对比 `v-if` 和 `v-show`：
* `v-if` 是“真实的”按条件渲染，因为它确保了在切换时，条件区块内的事件监听器和子组件都会被销毁与重建。
* `v-if` 也是惰性的：如果初次在渲染时条件值为 false，则不会做任何事。条件区块只有当条件首次变为 true 时才被渲染。
* 相比之下，`v-show` 简单许多，元素无论初始条件如何，始终会被渲染，只有基于CSS `display` 属性会被切换。（当其属性为 `none` 时，才会被隐藏）

总的来说，`v-if` 有更高的切换开下，而 `v-show` 有更高的初始渲染开销。因此，如果需要频繁切换，则使用 `v-show` 更好；如果在运行时绑定条件很少改变，则使用 `v-if` 更合适


### 4 列表渲染

我们可以使用 `v-for` 指令基于一个数组来渲染一个列表。
`v-for` 指令的值需要使用 `item in items` 形式的特殊语法，其中 `items` 是源数据的数组，而 `item` 是迭代项的**别名**

``` js
<template>
    <h3>列表渲染</h3>
    <p v-for="item in names">{{ item }}</p>
</template>

<script>

    export default{
        data(){
            return{
                names:["张三","李四","王五"]
            }
        }
    }

</script>
```
渲染后效果：
``` html
<h3>列表渲染</h3>
<p>张三</p>
<p>李四</p>
<p>王五</p>
```


`v-for` 也支持使用可选的第二个参数表示当前项的位置索引
``` js
<template>
    <h3>列表渲染</h3>
    <p v-for="(item, index) in names">{{ item }}:{{ index }}</p>
</template>

<script>

    export default{
        data(){
            return{
                names:["张三","李四","王五"]
            }
        }
    }

</script>
```
渲染后效果：
``` html
<h3>列表渲染</h3>
<p>张三:0</p>
<p>李四:1</p>
<p>王五:2</p>
```

一般喜欢使用三个参数的形式，因为它可以让你同时获得当前项的元素、当前项的键名和当前项的索引：`v-for="(item, key, index) in items"`。【顺序不能变：值、键名、索引】

而 `in` 可以与 `of` 互换

### 5 通过Key管理状态

Vue默认按照“就地更新”的策略来更新通过 `v-for` 渲染的元素列表。
当数据项的顺序改变时，Vue 不会随之移动 DOM 元素的顺序，二十就地更新每个元素，确保它们在原本指定的索引位置上渲染。

为了给 Vue 一个提示，以便它可以跟踪每个节点的标识，从而重用和重新排序现有元素，而不是重新渲染整个列表，我们需要为每项提供一个唯一的 `key` attribute。

``` js
<template>
    <h3>Key属性添加到v-for循环中</h3>
    <p v-for="(item, index) in names" :key="index">{{ item }}</p>
    <!-- 在这里是通过 v-bind 绑定的特殊 attribute -->
    <!-- 推荐在任何可行的时候为 v-fpr 提供一个 `key` attribute -->
    <!-- key 的绑定的期望值是一个基础类型的值，例如字符串或 number 类型 -->
</template>

<script>
    export default{
        data(){
            return{
                names:["张三","李四","王五"]
            }
        }
    }
</script>
```

### 6 事件处理

我们可以使用 `v-on` 指令（简称 `@` 符号）来监听 DOM 事件，并在触发时运行对应的 JavsScript。用法：`v-on:事件名="方法名"` 或 `@事件名="方法名"`。

事件处理器的值可以是
* 内联事件处理器：直接在元素上绑定 JavaScript 函数
* 方法事件处理器：在组件的 methods 选项中定义一个方法，并在元素上绑定 `v-on` 指令，值为方法的名称

#### 6.1 内联事件处理器

``` vue
<template>
    <h3>内联事件处理器</h3>
    <button @click="counter++">Add</button>
    <p>Counter: {{counter}}</p>
</template>

<script>
export default{
    data(){
        return{
            counter:0
        }
    }
}
</script>
```

#### 6.2 方法事件处理器

``` vue
<template>
    <h3>方法事件处理器</h3>
    <button @click="AddCounter">Add</button>
    <p>Counter: {{counter}}</p>
</template>

<script>
export default{
    data(){
        return{
            counter:0
        }
    },

    // 所有方法或者函数都放在这里
    methods:{
        AddCounter(){
            // 读取到data里面的数据的方案：this.counter
            this.counter++
        }
    }
}
</script>
```

#### 6.3 事件传参

``` vue
<template>
    <h3>事件传参</h3>
    <p @click="getNameHandler(item)" v-for="(item, index) of names" :key="index">{{ item }}</p>
</template>

<script>
export default{
    data(){
        return{
            names:['张三','李四','王五']
        }
    },

    methods:{
        getNameHandler(name){
            console.log(name);
        }
    }
}
</script>
```

##### 6.3.1 在传参的过程中获取事件对象

``` vue
<template>
    <h3>事件传参</h3>
    <p @click="getNameHandler(item, $event)" v-for="(item, index) of names" :key="index">{{ item }}</p>
</template>


<script>
export default{
    data(){
        return{
        }
    },

    methods:{
        getNameHandler(name, event){
            console.log(name);
            console.log(event);
        }
    }
}
</script>
```

#### 6.4 事件修饰符

在处理事件时调用 `event.preventDefault()` 或 `event.stopPropagation()` 是很常见的。尽管我们可以直接在方法内调用，但如果方法能更专注于数据逻辑而不用去处理DOM事件的细节会更好

为解决这一问题，Vue 为 `v-on` 提供了**事件修饰符**，常用有以下几个：

* `.stop`：阻止事件冒泡
* `.prevent`：阻止默认行为
* `.once`：只触发一次
* `.enter`：只触发 `enter` 键
* `.tab`：只触发 `tab` 键

##### 6.4.1 阻止默认事件

``` vue
<template>
    <h3>事件修饰符 - 阻止默认事件</h3>
    <a @click.prevent="clickhandle" href="http://bing.com">Bing搜索</a>
</template>

<script>
export default{
    data(){
        return{
            names:['张三','李四','王五']
        }
    },
    methods:{
        clickhandle(e){
            // 阻止默认事件
            // e.preventDefault()
            console.log("点击了")
        }
    }
}
</script>
```

### 7 数组变化侦听

#### 7.1 变更方法

Vue 能够侦听响应式数组的变更方法，并在它们被调用时触发相关的更新。这些方法包括：
* `push()`
* `pop()`
* `shift()`
* `unshift()`
* `splice()`
* `sort()`
* `reverse()`
  ...

#### 7.2 替换数组方法

变更方法顾名思义，就是会对调用他们的原始数组进行变更。相应的，也有一些不可变的方法，例如 `filter()`、`concat()`、`slice()` ，这些都不会更改原始数组，而总是**返回一个新的数组**。当遇到是非变更方法时，我们需要将旧的数组替换为新的
``` vue
<template>
    <h3>数组变化侦听</h3>
    <button @click="addListData">添加数据</button>
    <ul>
        <li v-for="(item, index) in names" key="index">{{ item}}</li>
    </ul>
</template>

<script>
    export default{
        data(){
            return{
                names:["张三","李四","王五"]
            }
        },
        methods:{
            addListData(){
                // 引起UI更新
                this.names.push('新数据')

                // 不引起UI更新
                this.names.concat['新数据2']
            }
        }
    }
</script>
```

### 8 计算属性

计算属性是一种特殊的属性，它基于它的依赖进行缓存，只有当它的依赖值发生变化时，才会重新求值。
``` vue
<template>
    <h3>{{ itbaizhan.name }}</h3>
    <p>{{ itbaizhanContent }}</p>
</template>
<script>
export default{
    data(){
        return{
            itbaizhan:{
                name:"百战程序员",
                content:["前端","Java","python"]
            }
        }
    },
// 计算属性
    computed:{
        itbaizhanContent(){
            return this.itbaizhan.content.length > 0 ? 'Yes' : "No"
        }
    }
}
</script>
```


### 9 Class绑定

Vue专门为`class`的`v-bid`用法提供了特殊的功能增强。除了字符串外，表达式的值也可以是对象或数组

#### 9.1 对象语法

``` vue
<template>
  <p :class="{'active': isActive, 'text-danger': hasError}">Class样式绑定1</p>
  <p :class="classObject">Class样式绑定2</p>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: true,
      classObject: {
        'active': true,
        'text-danger': true
      }
    }
  }
}
</script>

<style>
.active {
  font-size: 30px;
  color: red;
}
</style>
```

#### 9.2 数组语法

``` vue
<template>
  <div :class="[activeClass, errorClass]">isActive</div>
</template>

<script>
export default {
  data() {
    return {
      activeClass: 'active',
      errorClass: 'text-danger'
    }
  }
}
</script>
```