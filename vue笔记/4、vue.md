## 1、Vue 基本概念

### 1、什么是 Vue？

**Vue 是渐进式 JavaScript  框架**，一套拥有自己规则的语法。

官网地址：https://cn.vuejs.org/  (作者：尤雨溪)



### 2、什么是渐进式？

**渐进式**就是逐渐使用，集成更多的功能。



### 3、什么是库和框架？

**库是方法的集合，而框架是一套拥有自己规则的语法。**



### 4、Vue 的学习方式

- **传统开发模式**：基于 html 文件开发 Vue
- **工程化开发方式**：在 webpack 环境中开发 Vue ，这是最推荐，企业常用的方式





## 2、@vue/cli 脚手架

### 1、@vue/cli 和脚手架介绍

- **@vue/cli** 是 vue官方提供的一个全局模块**包**（得到 vue 命令），此包用于**创建脚手架**项目
- **脚手架**是为了保证格式工厂过程顺序进行而搭设的工作平台

我们使用 vue 开发项目，**不需要**自己**配置 webpack** 。vue 官方提供了一个包叫  **@vue/cli ,可以创建脚手架**。



### 2、脚手架的好处

- 开箱即用
- **0 配置webpack**
  - babel  支持
  - css, less 支持
  - 开发服务器支持



### 3、全局安装 @vue/cli 模块包

1. 全局安装 @vue/cli 模块包   **yarn global  add  @vue/cli**   或  **npm  install  @vue/cli -g**

2. 检查是否**安装**成功，**vue  -V**  ,方可查看版本号

   **遇见的问题**：安装包之后，想查看版本号一直显示 vue不是内部或外部命令。

   **解决方法**：找到  vue.cmd 文件的路径，把该路径添加到 path 环境变量中，方可解决。



### 4、@vue/cli 创建项目

1. **创建项目**   **vue  create  vuecli-demo**        vuecli-demo 是项目名，项目名不能有大写字母，中文和特殊符号

2. **启动开发服务器**， cd 进入项目下， **cd  vuecli-demo**   ， 然后   **yarn  serve**（npm run serve）  **启动本地热更新开发服务器**

3. 然后再浏览器输入给出的  url  ,即可浏览项目页面

   **遇见的问题**： 打开服务器yarn serve 时显示错误：**ERROR**  Error: The project seems to require yarn but it's not installed.，把项目下的  **yarn.lock 删掉**，**再全局安装  npm install -g yarn** 即可。



### 5、项目文件夹和文件含义

- vuecli-demo			        # 项目目录
- node_modules                # 项目依赖的第三方包
- public                                # 静态文件目录
- src                                     # 业务文件夹
  - assets                        # 静态资源
  - App.vue                    #  Vue 页面入口

- components                    # 组件目录

  

### 6、vue 项目里3个文件之间的关系

**main.js  和  App.vue ,以及 index.html 作用和关系？**

1. main.js             项目打包主入口， vue 初始化
2. App.vue            vue 页面主入口
3. index.html       浏览器运行的文件
4. App.vue  =>  main.js  =>   index.html  （在 main.js  引入 App.vue ,然后渲染到 index.html）



### 7、@vue/cli  自定义配置

项目中没有  **webpack.config.js**  文件，因为  vue 脚手架项目用的是 **vue.config.js**  

**假如我们想要更改默认的端口号，改成3000：,改完之后重启 webpack 开发服务器**    **yarn  serve**

~~~javascript
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  devServer: {
    port: 3000,
    open: true  // 浏览器自动打开
  }
})
~~~





### 8、eslint 是什么？

**eslint** 是**代码检查工具**，违反规定就报错。比如你设置了变量但是没有保存，就会报错。

**那么如何关闭呢？**

在 vue.config.js  中**设置 lintOnSave 为 false** 重启服务器即可， yarn serve



### 9、单 vue 文件讲解

- **vue 推荐采用 .vue 文件来开发项目**
- template 里只能有一个根标签
- js 独立作用域互不影响
- style 配合 scoped 属性，保证样式只针对当前 template 内标签生效

**单 vue 文件好处？**

**独立作用域，不再担心变量重名问题。**





## 3、vue 基础语法

### 1、插值表达式

**插值表达式:**又叫声明式渲染，文本插值。**变量在 data 函数return 的对象里面，data跟return是固定写法**

作用：在dom 标签中，直接插入 vue 数据变量

- 语法：**{{表达式}}**

~~~vue
<template>
  <div>
    <!-- 2、把值赋予到标签 -->
    <h1>{{msg}}</h1>
    <h2>{{obj.name}}</h2>
    <h3>{{obj.age > 18 ? '成年':'未成年'}}</h3>
  </div>
</template>

<script>
export default {
  // 1、变量在 data 函数return 的对象里面，data跟return是固定写法
  data() {
    return {
      msg: "Hello Vue",
      obj: {
        name: "小vue",
        age: 10
      }
    };
  }
};
</script>

<style>
</style>

~~~





### 2、MVVM 设计模式

**目标：转变思维，用数据驱动视图改变，操作 dom 的事，vue 源码内干了**

**设计模式**：是一套被反复使用的，多数人知晓的，经过分类编目、代码设计经验的总结。（设计模式就是对代码分层，引入一种架构的概念）

**MVVM 是什么？**

MVVM （模型，视图，视图模型双向关联的一种设计模式）

好处：减少 DOM 操作，提高开发效率





### 3、vue指令  v-bind

- **v-bind  语法和简写**
  - 语法： v-bind:属性名 = "vue变量"
  - 简写：   ：属性名 = "vue变量"

**作用： 给标签属性设置 Vue 变量的值**

~~~vue
<template>
  <div>
    <!-- 2、把值赋值给原生标签属性上 -->
    <!-- 语法：v-bind:原生属性名="vue变量" 简写： ：原生属性名="vue变量"-->
    <a v-bind:href="url">点击去往百度</a>
  </div>
</template>

<script>
export default {
  // 1、准备变量
  data() {
    return {
      url: "http://www.baidu.com",
      imgUrl:
        "https://cdn3.zzzmh.cn/v2/crx/cjpalhdlnbpafiamejdnhcphjbkeiagm/logo/logo.png"
    };
  }
};
</script>

<style>
</style>
~~~





### 4、vue指令 v-on

**v-on 语法**

**方法要在 methods 选项定义**

- v-on:事件名 = "要执行的少量代码"

- v-on:事件名 = "methods 中的函数名"

- v-on:事件名 = "methods 中的函数名（实参）"

  **作用：为标签绑定事件**

  **简写：把 v-on 简写成@**

  - @事件名 = "methods 中的函数名（实参）" 

~~~vue
<template>
  <div>
    <p>你要购买商品的数量：{{count}}</p>
    <button v-on:click="count = count + 1">+1</button>
    <button v-on:click="addFn">+1</button>
    <button v-on:click="addCountFn(5)">+5</button>
    <!-- 以下是简写 -->
    <button @click="count = count - 1">-1</button>
    <button @click="subFn">-1</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 1
    };
  },
  methods: {
    addFn() {
      // this 指向的是 export default的 {}
      // data 函数会把对象挂到当前组件对象上
      this.count++;
    },
    addCountFn(num) {
      this.count = this.count + num;
    },
    subFn() {
      this.count--;
    }
  }
};
</script>
<style>
</style>
~~~





### 5、vue指令 v-on事件对象e

**语法：**

- 无传参，通过形参直接接受
- 传参，通过 **$event** 指代事件对象传给事件处理函数

~~~vue
<template>
  <div>
    <a @click="one" href="http://www.baidu.com">百度</a>
    <br>
    <a @click="two(10, $event)" href="http://taobao.com">淘宝</a>
  </div>
</template>

<script>
export default {
  methods: {
    // 1、事件触发，无传值，可以直接获取事件对象 e
    one(e) {
      e.preventDefault();
    },
    // 2、事件触发，传值，需要手动传入 $event
    two(num, e) {
      e.preventDefault();
    }
  }
};
</script>

<style>
</style>
~~~



### 6、vue指令 v-on 修饰符

语法：

**@事件名.修饰符 = "methods 里的函数"**

- **.stop**      -- 阻止事件冒泡
- **.prevent**   --  阻止默认行为
- **.once**       -- 程序运行期间，只触发一次事件处理函数（其实事件还在执行，只不过只触发一次）

~~~vue
<template>
  <div @click="fatherFn">
    <!-- stop 阻止事件冒泡，prevent 阻止事件默认行为, once 指触发一次事件处理函数 -->
    <p @click.stop="oneFn">.stop - 阻止事件冒泡</p>
    <a @click.prevent href="http://www.baidu.com">去百度</a>
    <p @click.once="twoFn">点击观察事件处理函数执行几次</p>
  </div>
</template>

<script>
export default {
  methods: {
    fatherFn() {
      console.log("farher -触发了点击事件");
    },
    oneFn() {
      console.log("p标签触发了点击事件");
    },
    twoFn() {
      console.log("事件执行次数");
    }
  }
};
</script>

<style>
</style>
~~~





### 7、vue指令 v-on 按键修饰符

语法(举例：)

- **@keydown.enter**      --  绑定鼠标按下事件，监听回车按键
- **@keydown.esc**          --  绑定鼠标按下事件，监听esc按键

更多修饰符：https://cn.vuejs.org/v2/guide/events.html

~~~vue
<template>
  <div>
    <!-- 1、绑定键盘按下事件，enter 监听回车键 -->
    <input type="text" @keydown.enter="enterFn">
    <!-- 2、绑定键盘按下事件，esc 监听esc键(在键盘左上方) -->
    <input type="text" @keydown.esc="escFn">
  </div>
</template>

<script>
export default {
  methods: {
    enterFn() {
      console.log("用户按下了回车键");
    },
    escFn() {
      console.log("用户按下了esc键");
    }
  }
};
</script>

<style>
</style>
~~~

