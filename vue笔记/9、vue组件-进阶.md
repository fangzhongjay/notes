## 1、动态组件

### 1、动态显示组件

1. 利用 vue 里的 <component> 组件，配合 is 属性来设置要显示哪个组件

就是当想在固定区域切换不同组件时，需要先把用到的组件引入，注册，并用name给其中一个赋值，再利用vue 里的 <component> 组件，配合 is 属性来设置要显示哪个组件。

~~~vue
<template>
  <div>
    <button @click="comName='UserName'">账号密码填写</button>
    <button @click="comName='UserInfo'">个人信息填写</button>

    <p>下面显示注册组件-动态切换：</p>
    <div style="border:1px solid red">
      <!-- 更换comName 的值，来进行切换组件 -->
      <component :is="comName"></component>
    </div>
  </div>
</template>

<script>
// 目标：动态组件 - 切换组件显示
// 场景：同一个挂载点要切换不同组件显示
// 1、引入组件 - 注册组件
// 2、变量-承载要显示的组件名
// 3、设置挂载点 <component :is="变量"></component>
// 4、点击按钮-切换comName 要显示的组件名

import UserName from "../components/01/UserName";
import UserInfo from "../components/01/UserInfo";
export default {
  data() {
    return {
      comName: "UserName"
    };
  },
  components: {
    UserName,
    UserInfo
  }
};
</script>

<style>
</style>

~~~



**小结：**

- 什么是动态组件？ => 在同一个挂载点，可以切换显示不同组件
- 如何使用？  =>  vue 内置的 <component> 组件，配合 is 属性
- 如何切换？  =>  改变 is 属性的值





### 2、组件缓存

频繁的切换组件会导致组件**频繁的创建和销毁。**

**解决：**用vue 内置的**keep-alive组件**，把包起来的组件缓存起来，这样切换组件的时候就不会频繁的创建和销毁。

~~~vue
<!-- 使用vue 内置的keep-alive组件，把包起来的组件缓存起来 -->
      <keep-alive>
        <!-- 使用vue 内置的component组件， 更换comName 的值，来进行切换组件 -->
        <component :is="comName"></component>
      </keep-alive>
~~~

**组件缓存的好处：**不会频繁的创建和销毁组件，页面更快呈现。



### 3、组件的激活和非激活

那么在组件缓存下我们怎么知道组件被切回来了呢？  ==>   扩展 2 个新的生命周期方法

- **activated** - **激活**时触发（切换回来）
- **deactivated** - **失去激活**状态触发（切换走）

~~~javascript
 // 组件缓存下，多了两个钩子函数
  activated() {
    console.log("02-UserName组件激活");
  },
  deactivated() {
    console.log("02-UserName组件失去激活");
  }
~~~





## 2、组件插槽

### 1、组件插槽

**目标：**组件插槽使用 - 为了让封装的组件显示不同的标签结构（灵活）

其实就是在被封装的组件，使用 <slot></slot> 占位，在调用组件时，在组件内把想要的样式写在里面，它就会把样式填充在 <slot></slot> 占的位置上。

![image-20220530210913885](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220530210913885.png)

当组件内某一部分标签不确定怎么办？  ==>   **用插槽技术**



### 2、插槽默认内容

目标：如果调用组件的时候，不传标签，那么slot内的内容就会作为默认显示内容。

~~~javascript
<slot>
        <h1>默认内容</h1>
 </slot>
~~~



### 3、具名插槽

**场景：**一个组件内有 2 处以上需要外部传入标签的地方

语法：

1. 给slot 添加一个 **name 属性**
2. 调用组件时，使用 **template 标签**配合 **v-slot: name名**，来分发对应标签在哪个位置

- **注意：v-slot:       可以简化为 #** （即#name名）

~~~vue
<Pannel>
        <template v-slot:title>
          <h4>氓</h4>
        </template>
        <template #content>
          <img src="../assets/mm.gif" alt>
          <span>我是内容</span>
        </template>
</Pannel>

-----------------------------------------------------------
<div>
    <!-- 按钮标题 -->
    <div class="title">

      <slot name="title"></slot>

      <span class="btn" @click="isShow = !isShow">{{ isShow ? "收起" : "展开" }}</span>
    </div>
    <!-- 下拉内容 -->
    <div class="container" v-show="isShow">

      <slot name="content">
        <h1>默认内容</h1>
      </slot>
      
    </div>
  </div>
~~~





### 4、作用域插槽

场景：使用插槽时，想使用子组件内变量

口诀：

1. 子组件，在 solt 上绑定属性和子组件内的值
2. 使用组件，传入自定义标签，用 template 和 v-slot = "自定义变量名"  （注意：这个是v-slot=   不是v-slot:  ）
3. 自定义变量名会自动绑定slot上所有属性和值

~~~vue
<Pannel>
        <!-- 需求：插槽时，使用组件内变量 -->
        <!-- scope变量：{{row：defaultObj}} --> 
        <template v-slot="scope">
          <p>{{scope.row.defaultTwo}}</p>
        </template>
      </Pannel>
      --------------------------------
       <slot :row="defaultObj">{{defaultObj.defaultOne}}</slot>
       
 export default {
  data() {
    return {
      isShow: false,
      defaultObj: {
        defaultOne: "无名氏",
        defaultTwo: "小传同学"
      }
    };
  }
};
~~~

作用域插槽什么时候使用？  ==>  使用组件插槽技术时，需要用到子组件内变量





## 3、自定义指令

### 1、自定义指令

目标：获取标签，扩展额外的功能。

自定义指令的注册分为 **全局注册** 和 **局部注册**。

~~~javascript
// 下面举例全局注册名为 gfocus的自定义指令，为其扩展获得焦点功能
Vue.directive("gfocus
gfocus的", {
  inserted(el) {
    el.focus()  // 触发标签的事件方法
  }
})

// 下面举例局部注册名为 focus的自定义指令，为其扩展获得焦点功能
export default {
  // 局部自定义指令,inserted是配置项方法
  // 注意：inserted方法 - 指令所在标签，被插入到网页上触发
  directives: {
    focus: {
      inserted(el) {
        el.focus();
      }
    }
  }
};

// 使用自定义指令
<input type="text" v-gfocus>
<input type="text" v-focus>
~~~

我们为什么要自定义指令？  =>  在 Vue 内置指令满足不了需求时，可以自己定义使用。



### 2、自定义指令-传值

目标：定义一个color指令，可以在使用自定义指令给其传值，让标签文字变色

 **// inserted方法 - 指令所在标签，被插入到网页上触发(触发一次)**
  **// update方法 - 指令对应数据/标签更新时，此方法被触发**

~~~javascript
// 目标：自定义指令传值
Vue.directive('color', { // 可以使用第二个参数（名字随意取）的value属性拿到自定义指令的传值
  inserted(el, clor) {
    el.style.color = clor.value 
  },
  update(el, clor) {
    el.style.color = clor.value
  }
})

// 使用自定义组件并传值
<p v-color="colorStr">修改文字颜色</p>

export default {
  // 局部自定义指令,inserted是配置项方法
  // 注意：
  // inserted方法 - 指令所在标签，被插入到网页上触发(触发一次)
  // update方法 - 指令对应数据/标签更新时，此方法被触发
  data() {
    return {
      colorStr: "res"
    };
  },

~~~

**总结：**

- 指令如何传值？   v-指令名 = “值”

- 指令值变化触发什么方法？   自定义指令的 update 方法， uodate方法是数据改变就会执行，而inserted则第一次使用才会执行。

  