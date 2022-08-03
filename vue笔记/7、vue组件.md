## 1、vue 组件

### 1、为什么要使用组件？

遇到重复标签想复用就可以使用组件，这样更方便。相当于就是把一些需要重复使用的代码封装成另一个 vue 文件，想调用的时候，直接导入，再使用标签应用即可。

**组件的好处：**各自独立，便于复用

~~~vue
<template>
  <div id="app">
    <h3>案例：折叠面板</h3>
    <!-- 3、使用组件 -->
    <Pannel></Pannel>
    <Pannel></Pannel>
    <Pannel></Pannel>
  </div>
</template>

<script>
// 1、导入组件
import Pannel from "./components/Pannel";
export default {
  // 2、注册组件
  components: {
    Pannel: Pannel // 第一个Pannel 要与第四行调用的名字一致，第二个要与import 命名的名字一致
  }
};
</script>

<style lang="less">
body {
  background-color: #ccc;
  #app {
    width: 400px;
    margin: 20px auto;
    background-color: #fff;
    border: 4px solid blueviolet;
    border-radius: 1em;
    box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.5);
    padding: 1em 2em 2em;
    h3 {
      text-align: center;
    }
  }
}
</style>

~~~



### 2、组件概念

- 组件是可复用的 Vue 实例，封装标签，样式和 JS 代码
- 组件化：封装的思想，把页面上 ‘ **可重用的部分** ’ 封装为 ’ **组件** ‘ ，从而方便项目的开发和维护
- 一个页面，可以拆分成一个个组件，一个组件就是一个整体，每个组件可以有自己独立的结构样式和行为

**什么时候封装组件？**  ==>   遇到重复标签，可复用的时候

**组件的好处？**   ==>   各自独立，互不影响





### 3、组件的使用步骤及分类

使用步骤：

- 1、**创建组件**（其实就是创建 .vue 文件）

- 2、**注册组件**（分为全局注册和局部注册）

  - 全局注册  -- main.js 中   
  - 局部注册  --  某 .vue 文件内

  3、使用组件 （组件名用作标签）

- 

![image-20220517220521494](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220517220521494.png)





### 4、scoped 作用过程

**Vue 组件内样式，只针对当前组件内标签生效如何做？**

给 **style** 上添加 **scoped** 

**原理和过程是什么？**

会自动给标签添加 data-v-hash 值属性，所有选择都到属性选择





## 2、组件通信

### 1、组件通信 - 父传子

**目标：**父组件 -->  子组件 传值（**谁被引入的谁就是子组件**）

**步骤：**

- 引入组件
- 注册组件
- 使用组件
- 传值进去

**什么时候需要父传子技术？**  ==>   从一个 vue 组件里把值传给 另一个 vue 组件 （父 -> 子）

**父传子口诀（步骤）是什么？**  

- 子组件内，props 定义变量，在子组件使用变量
- 父组件内，使用子组件，属性方式给 props 变量传值

~~~vue
// 父组件：
<template>
  <div>
    <!-- 目标：父（App.vue）==> 子(MyProduct.vue) 分别传值进入
    需求：每次组件显示不同的数据信息
    步骤（口诀）
    1、在子组件里 props 定义变量（准备接收）
    2、在父组件里 传值进去-->

    <!-- 4、调用组件(传值进去) -->
    <Product title="好吃的口水鸡" price="50" intro="开业大酬宾，全场八折"></Product>
    <Product title="好吃的鸡" price="50" intro="开业大酬宾，全场八折"></Product>
    <Product title="好吃的口水" price="50" :intro="str"></Product>
  </div>
</template>

<script>
// 1、创建组件（.vue 文件）
// 2、引入组件
import Product from "./components/MyProduct";
export default {
  data() {
    return {
      str: "啥玩意，这么贵"
    };
  },

  // 3、注册组件
  components: {
    // Product: Product  key 和 value 变量名同名，可以简写成一个
    Product
  }
};
</script>

<style>
</style>

// 子组件：
<template>
  <div class="my-product">
    <h3>标题: {{title}}</h3>
    <p>价格: {{price}}</p>
    <p>{{intro}}</p>
  </div>
</template>

<script>
export default {
  props: ["title", "price", "intro"]
};
</script>

<style>
.my-product {
  width: 400px;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
~~~





### 2、父向子 - 配合循环

目标：父组件   ==> 子组件    循环使用-传值

**循环使用组件注意事项：  每次循环，变量和组件，都是独立的**

~~~vue
<template>
  <div>
    <!-- :key 是为了提高更新的性能，有 id 用 id ,无id 用索引 -->
    <MyProduct
      v-for="obj in list"
      :key="obj.id"
      :title="obj.proname"
      :price="obj.proprice"
      :intro="obj.info"
    ></MyProduct>
  </div>
</template>

<script>
// 目标：循环使用组件 - 分别传入数据
// 1、创建组件
// 2、引入组件
import MyProduct from "./components/MyProduct";
export default {
  data() {
    return {
      list: [
        {
          id: 1,
          proname: "超级好吃的棒棒糖",
          proprice: 18.8,
          info: "开业大酬宾, 全场8折"
        },
        {
          id: 2,
          proname: "超级好吃的大鸡腿",
          proprice: 34.2,
          info: "好吃不腻, 快来买啊"
        },
        {
          id: 3,
          proname: "超级无敌的冰激凌",
          proprice: 14.2,
          info: "炎热的夏天, 来个冰激凌了"
        }
      ]
    };
  },
  // 3、注册组件
  components: {
    MyProduct
  }
};
</script>

<style>
</style>

~~~





### 3、单向数据流

**目标：**从父到子的数据流向，叫**单向数据流**

**原因：**子组件修改，不通知父级，造成数据不一致性，**vue 规定 props 里的额变量，本身是只读的，不允许重新赋值**





### 4、组件通信 - 子向父

**什么时候使用子传父技术？** ==>   当子想要去改变父里面的数据时

**子传父如何实现？** 

- 父组件内，给组件 @自定义事件 = “ 父 methods 函数 ”
- 子组件内，恰当时机 this.$emit('自定义事件名'，值1，值2····)  （**当调用事件时，会把值传给父组件里的 @自定义事件 = “ 父 methods 函数** ）





### 5、组件通信 - 跨组件传值

什么时候使用 eventBus 技术？  ==>   当 2 个没有引用关系的组件之间要通信传值

eventBus 技术本质是什么？  ==>  空白 Vue 对象，只负责 $on(注册事件)和 $emit(触发事件)





## 3、todo 案例 

### 1、项目创建 - 静态页面准备

**涉及到了哪些技术点？**

- 组件创建  
- 组件引入
- 组件注册
- 组件使用

~~~vue
<template>
  <div class="todoapp">
    <TodoHeader></TodoHeader>
    <TodoMain></TodoMain>
    <TodoFooter></TodoFooter>
  </div>
</template>

<script>
// 1. 目标：创建工程 - 准备相关组件文件 - 标签 + 样式 （静态）
// 1.0 样式引入(样式引入不需要命名啥的)
import "./styles/base.css";
import "./styles/index.css";
// 1.1 组件引入
import TodoHeader from "./components/TodoHeader";
import TodoMain from "./components/TodoMain";
import TodoFooter from "./components/TodoFooter";

export default {
  // 1.2 组件注册
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter
  }
};
</script>

<style>
</style>
~~~



### 2、循环展示任务

- 需求1：把待办的任务，展示到页面 TodoMain.vue 组件上
- 需求2：关联选中状态，设置相关样式



**涉及到了哪些技术点？**

- 父传子技术
- v-for 循环
- v-model 绑定
- 动态class使用

![image-20220518213927804](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220518213927804.png)





### 3、添加功能

**需求：**输入任务敲击回车，新增待办任务

**分析：**

1. TodoHeader.vue  - 输入框 - 键盘事件 - 回车按键
2. 子传父，把待办任务 - App.vue 中 - 加入数组list 里
3. 原数组改变，所有用到的地方都会更新

**涉及到哪些技术点？**

- 键盘事件，enter 修饰符
- 子传父技术





### 4、删除功能

**需求：**点击任务后的 x ,删除当前这条任务

**分析思路：**

1. x 标签 - 点击事件 - 传入id 进行区分
2. 子传父，把 id 传回 - App.vue 中 - 删除数组 list 里某个对应的对象
3. 原数组改变，所有用到的地方都会更新 

**涉及到哪些技术点？**

- 点击事件（点击事件要把id 值传入函数）
- 子传父（把 id 传到父组件）
- 父组件根据 id 值来删除对应的某个元素， v-for 更新





### 5、底部统计

**需求：**统计当前任务的条数

**分析：**

1. App.vue 中 - 数组list - 传给 TodoFooter.vue 
2. **直接在标签上显示** / **定义计算属性用于显示都可以**
3. 原数组只要改变，所有用到次数组的地方都会更新

**涉及到了哪些技术点？**

- 父传子
- 计算属性
- 数组更新 - 所有地方受影响



### 6、数据切换(较重要)

**需求1**：点击底部切换 - 点谁谁有边框

**需求2**：对应切换不同数据显示

**分析：**

- TodoFooter.vue - **定义isSel - 值为 all , yes,no 其中一种**
- 多个 class 分别判断谁应该有类名 selected
- 点击修改 isSel 的值
- 子传父，把类型isSel 传到 App.vue



### 7、清空已完成

// 清空已完成(其实就是把没完成的筛选出来，覆盖上去)



### 8、数据缓存

**分析：**

1. App.vue  -  侦听 list 数组改变 - 深度
2. 覆盖式存入到本地  - 注意本地只能存入 JSON 字符串
3. 刷新页面 - list 一个默认从本地取值 - 要考虑无数据空数组

**涉及到了哪些技术点？**

- 深度侦听
- 数据缓存
- 序列化和反序列化



### 9、全选功能

**需求1**：点击全选 -小选框受到影响

**需求2**：小选框都选中 - 全选自动选中状态

**涉及到了哪些技术点？**

- 计算属性完整写法
- every 方法如果不遍历返回 treu







## 4、vue 的生命周期

### 1、什么是vue的生命周期？

从**创建**到**销毁**的整个过程就是  --  Vue实例的生命周期



### 2、钩子函数

**Vue 框架内置函数，随着组件的生命周期阶段，自动执行。**

**作用：**特定的时间点，执行特定的操作

**场景：**组件创建完毕后，可以在 created 生命周 期函数中发起 Ajax 请求，从而初始化 data 数据

**分类：4大阶段  8个方法**

| 阶段   | 方法名        | 方法名    |
| ------ | ------------- | --------- |
| 初始化 | beforeCreate  | created   |
| 挂载   | beforeMount   | mounted   |
| 更新   | beforeUpdate  | updated   |
| 销毁   | beforeDestroy | destroyed |



如何知道 Vue 生命周期到达了什么阶段？ => **使用钩子函数**





### 3、初始化阶段

初始化步骤：

1. new Vue（）  --  Vue实例化（组件也是一个小的Vue实例）

2. init Events  & Lifecycle  -- 初始化事件和生命周期函数

3. **beforeCreate**  -- 生命周期钩子函数被执行  **(此时还不能调用 data 和 merhods 的属性)**

4. init injection & reactivity  --  **Vue内部添加 data 和 methods等**

5. **created**  -- 生命周期钩子函数被执行，实例创建

6. 接下来是编译模板阶段  -- 开始分析

7. Has el option?  -- 是否有 el 选项 -- 检查要挂到哪里

   没有el ， 则调用$mount() 方法，有 ， 继续检查 template

**注意：**

- Vue 实例从**创建**到**编译模板**执行了哪些钩子函数？  =>  **beforeCreate / created**

- created 函数触发能获取 data？ =>  能获取 data, 不能获取真实 DOM

  

### 4、挂载阶段

1. template 选项检查

   有 ->  编译template返回render 渲染函数。  无 -> 编译 el 选项对应标签作为 template(要渲染的模板)

2. 虚拟 DOM 挂载为真实 DOM 之前

3. **beforeMount**  -- 生命周期钩子函数被执行（拿不到真实的 DOM，因为还没有挂载到真实的DOM）

4. ·······   把虚拟 DOM 和渲染的数据，一并挂到真实 DOM上

5. 真实 DOM 挂载完毕

6. **mounted**  -- 生命周期钩子函数被执行

   

### 5、更新阶段

1. 当 data 里数据**改变**，更新 DOM **之前**
2. **beforeUpdate**  -- 生命周期钩子函数被执行 **（拿到到更新后的真实 DOM）**
3. Virtual DOM ·····  --  虚拟 DOM重新渲染，打补丁到真实 DOM
4. **updated**  -- 生命周期钩子函数被执行
5. 当有 data 数据改变  -- 重复这个循环

**注意：**

- 什么时候执行 updated 钩子函数？  =>  当数据发送变化并更新页面后
- 在哪可以获取更新后的DOM ?    =>   在 updated 钩子函数里



### 6、销毁阶段

1. 当 $destroy() 被调用 -- 比如组件 DOM 被移除（例：v-if）
2. **beforeDestroy**  -- 生命周期钩子函数被执行
3. 拆除数据监视器、子组件和事件侦听器
4. 实例销毁后，最后触发一个钩子函数
5. **destroyed**  --  生命周期钩子函数被执行

**注意：**

- 一般在 beforeDestroy / destroyed 里做什么呢？ =>  手动消除计时器/ 定时器 / 全局事件





## 5、axios

### 1、axios 的使用

**axios 是一个专门用于发送 ajax 请求的库。**（基于原生 ajax + promise 技术封装通用于前后端的请求库）

官网：http://www.axios-js.com/

**特点：**

- 支持客户端发送 Ajax 请求
- 支持服务端 Node.js 发送请求
- 支持 Promise 相关用法
- 支持请求和响应的拦截器功能
- 自动转换 JSON 数据

- axios 底层还是原生 js 实现，内部通过Promise 封装的



### 2、axios 获取图书信息

~~~vue
<template>
  <div>
    <p>1、获取所有图书信息</p>
    <button @click="getAllFn">点击-查看控制台</button>
  </div>
</template>

<script>
// 目标：获取所有图书信息
// 1、下载axios    yarn add axios
// 2、引入axios
import axios from "axios";
export default {
  methods: {
    getAllFn() {
      // 3、发起axios 请求
      axios({
        url: "http://123.57.109.30:3006/api/getbooks",
        method: "GET" // 默认就是 GET，所以也可以省略不写
      }).then(res => {
        console.log(res);
      });
      //  axios()得到的就是一个Promise对象，可以使用then等方法
    }
  }
};
</script>

<style>
</style>

~~~

**注意：**

- axios 如何发起一次 get 请求？  =>  在 methods 选项配置为 true /  也可以默认不写
- axios 函数调用原地结果是什么？  =>  是一个**Promise 对象**
- 如何拿到 Promise 里 ajax 的成功或失败的结果？  =>   **axios({ }).then().catch()**



### 3、获取图书信息 - get 传参

**ajax 如何给后台传参呢？**

1. 在 url ？拼接 -查询字符串
2. 在 url 路径上 - 需要后端特殊处理
3. 在请求体 / 请求头 传参给后台

axios 哪个配置项会把参数自动写到 url ? 后面     =>   params

~~~vue
<script>
// 4、如何拿到用户在框里输入的值呢？使用 v-model 双向绑定即可拿到
    findFn() {
      axios({
        url: "http://123.57.109.30:3006/api/getbooks",
        method: "GET",
        // params 里的参数都会被axios拼接到url? 后面
        params: {
          bookname: this.bName
        }
      }).then(res => {
        console.log(res);
      });
    }
    </script>
~~~





### 4、添加书籍 - post传参

目的：新增图书信息

- post 请求方法，一般在哪里传递数据给后台？   =>  请求体中
- axios 哪个选项，可以把参数自动装入到请求体中？    =>  data 选项
- axios 默认发给后台请求体数据格式是？    =>  json 字符串格式

~~~vue
<script>
sendFn() {
      axios({
        url: "http://123.57.109.30:3006/api/addbook",
        method: "POST",
        data: {
          appkey: "7250d3eb-18e1-41bc-8bb2-11483665535a",
          ...this.bookObj // 展开运算符，等同于下面
          //   bookname: this.bookObj.bookname,
          //   author: this.bookObj.author,
          //   publisher: this.bookObj.publisher
        }
      });
    }
    </script>
~~~





### 5、axios 全局配置

**目的：为了不用每次请求都要写一长串的 url , 可以把基础地址全局配置，统一管理**

**axios.default.baseURL = "  里面填基地址 "**

- 修改请求url / 以后的请求都不要带前缀基地址了   --   **运行时， axios 的baseURL 会自动拼在前面**



## 6、$refs 和 $nextTick 的使用

### 1、ref 获取原生DOM元素

目的：通过 id 或 ref 属性获取原生的 DOM元素

- 在mounted 生命周期  --  2种方式获取原生 DOM 标签
- **方法1：在标签里添加 id  ,  然后  console.log(document.getElementById("XXX"))**
- **方法2：在标签里添加 ref  , 然后  console.log(this.$refs.XXX)**

~~~vue
<template>
  <div>
    <p>1、获取原生DOM元素</p>
    <h1 id="h" ref="myH">获取原生DOM的两种方法</h1>
  </div>
</template>

<script>
export default {
  mounted() {
    console.log(document.getElementById("h"));
    console.log(this.$refs.myH);
  }
};
</script>

<style>
</style>
~~~



### 2、获取组件对象

目的：通过 ref 属性获取组件对象

// 1、创建组件 -> 引入组件 -> 注册组件 -> 使用组件

// 2、组件起别名 ref

// 3、恰当时机，获取组件对象

**方法1：this.$refs.de.fn();**

**方法2：**

​    **let demoObj = this.$refs.de;**

​    **demoObj.fn();**

**注意：**

**如何获取组件对象呢？** 

- 目标组件添加 ref 属性
- this.$refs.名字      获取组件对象

**拿到组件对象能够做什么呢？**

- 调用组件里的属性和方法





### 3、nextTick基础使用

**目的：**等 DOM 更新后，触发次方法里函数体执行

**语法：this.$nextTick(函数体)**

其实就是实时的拿到 DOM 里的值，因为vue 更新 DOM 是异步

~~~vue
<script>
methods: {
    btn() {
      this.count++; // vue 检测数据更新,(开启一个DOM更新队列（异步任务）)
      console.log(this.$refs.myP.innerHTML); // 得到是上一步的值，
      // 原因：vue 更新 DOM 是异步
      // 解决：this.$nextTick()
      // 过程：DOM更新完会挨个触发$nextTick里的函数体
      this.$nextTick(() => {
        console.log(this.$refs.myP.innerHTML);
      });
    }
  }
    </script> 
~~~



