## 1、vue指令 

### 1、vue指令 v-moel

**语法：v-model = "vue数据变量"**

**作用：实现数据双向绑定**。实现把**vue 的数据变量**和**表单的 value 属性**双向绑定在一起。目前暂时只能用在表单标签上。

~~~vue
<template>
  <div>
    <!-- v-model 实现双向绑定，value属性和vue变量 -->
    <div>
      <span>用户名：</span>
      <input type="text" v-model="username">
    </div>
    <div>
      <span>密码：</span>
      <input type="password" v-model="pass">
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: "",
      pass: ""
    };
  }
};
</script>
 
<style>
</style>
~~~





### 2、v-model 绑定不同的表单标签

- 下拉菜单时，v-model 写在 **select 标签**里，value 写在 option上
- 复选框时，要注意的**vue变量**分为数组和**非数组**两种类型。
  - 非数组        --  关联的是 checked 属性
  - 数组            --  关联的是 value 属性

~~~vue
<template>
  <div>
    <div>
      <span>您来自于：</span>
      <!-- 下拉菜单要绑定在 select 上 -->
      <select v-model="from">
        <option value="北京市">北京</option>
        <option value="南京市">南京</option>
        <option value="天津市">天津</option>
      </select>
    </div>

    <!-- (重要)v-model 遇到复选框的时候，变量值分为
          非数组：关联的复选框的 checked 属性（true/false）
          数组：关联的是复选框的 value 属性
    -->
    <div>
      <span>爱好：</span>
      <input type="checkbox" v-model="hobby" value="抽烟"> 抽烟
      <input type="checkbox" v-model="hobby" value="喝酒"> 喝酒
      <input type="checkbox" v-model="hobby" value="打豆豆"> 打豆豆
    </div>

    <div>
      <span>性别：</span>
      <input type="radio" value="男" name="sex" v-model="gender"> 男
      <input type="radio" value="女" name="sex" v-model="gender"> 女
    </div>

    <div>
      <span>自我介绍：</span>
      <textarea v-model="intro"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      from: "",
      // hobby: "", true
      hobby: [],
      gender: "",
      intro: ""
    };
  }
};
</script>

<style>
</style>
~~~





### 3、v-model 的其他修饰符

**目标：让 v-model 拥有更强大的功能**

语法： **v-model.修饰符 = "vue 数据变量"**

- **.number**        以 parseFloat 转换为数字类型

- **.trim**              去除首尾空白字符

- **.lazy**               当失去焦点且内容发送改变时，才把值赋予给 vue 数据变量

  ~~~vue
  <template>
    <div>
      <!-- .number 修饰符：将值转换为数字类型 -->
      <div>
        <span>年龄：</span>
        <input type="number" v-model.number="age">
      </div>
  
      <!-- .trim 修饰符：去除首尾空白字符 -->
      <div>
        <span>人生格言：</span>
        <input type="text" v-model.trim="motto">
      </div>
  
      <!-- .lazy 修饰符：当失去焦点且内容发送改变时，才触发v-model -->
      <div>
        <span>个人简介：</span>
        <textarea v-model.lazy="intro"></textarea>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        age: 0,
        motto: "",
        intro: ""
      };
    }
  };
  </script>
  
  <style>
  </style>
  ~~~

  



### 4、v-text 和 v-html

**目前：更新 DOM 对象的 innerText / innerHTML**

语法：

- **v-text = "vue 数据变量"**
- **v-html= "vue 数据变量"**

**注意：会覆盖插值表达式！！！**

~~~vue
<template>
  <div>
    <p v-text="str"></p>
    <p v-html="str">里面的内容会被覆盖</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      str: "<span>我是一个span标签</span>"
    };
  }
};
</script>

<style>
</style>
~~~





### 5、v-show 和 v-if

**目标：控制标签的隐藏或显示。**

语法：

- **v-show = "vue 变量"**
- **v-if = "vue 变量"**

**区别：**

**v-show:   采用的是 display:none 来显示隐藏  // 频繁切换**

**v-if:         采用的是从 DOM 树直接移除           // 移除**

~~~vue
<template>
  <div>
    <!-- 
      v-show:采用的是 display:none 来显示隐藏  // 频繁切换
      v-if:采用的是从 DOM 树直接移除           // 移除
    -->
    <h1 v-show="isShow">我是h1标签</h1>
    <h2 v-if="isIf">我是h2标签</h2>
    <!-- v-if 和 v-else 使用 -->
    <p v-if="age > 18">成人</p>
    <p v-else>小孩</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: true, // true 表示显示，false表示隐藏
      isIf: false,
      age: 15 // 小孩
    };
  }
};
</script>

<style>
</style>
~~~





### 6、v-for 

目标：列表渲染，所在标签结构，按照数据数量，循环生成。

语法：**（谁循环写到谁身上）**

- **v-for = "(变量名，索引名)  in  目标结构 "**

- **v-for = "变量名 in  目标结构 "**

  **目标结构可以是数组 / 对象 / 数字**

**注意：**

1. **v-for 的临时变量名不能用到 v-for 范围外**
2. 表达式中的   in  两边必须要有空格





### 7、v-for 更新监测

**目标：目标结构变化，触发 v-for 的更新**

**案例：**

- 情况1：数组翻转
- 情况2：数组截取
- 情况3：更新值

**口诀：**

- **数组变更方法**，就会导致 v-for 更新，页面刷新。
- 数组非变更方法，返回新数组，不会动原数组，就不会导致 v-for 更新，可采用覆盖数组或 this.$set()

常见的**数组变更方法**有：

- shift ()                  -- 头部删除数组元素
- pop()                   --   尾部删除数组元素
- unshift()             --   头部添加数组元素
- push()                --   尾部添加数组元素
- splice()               --   删除某一个或几个数组元素
- sort()                  --   排序数组元素
- reserve()            --   翻转数组

~~~vue
<template>
  <div>
    <ul>
      <li v-for="(val,index) in arr" :key="index">{{val}}</li>
      <!-- :key="index" 可以使波浪线消失-->
      <button @click="revBtn">数组翻转</button>
      <button @click="sliceBtn">截取前3个</button>
      <button @click="updateBtn">更新第一个值</button>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [5, 3, 9, 2, 1]
    };
  },
  methods: {
    revBtn() {
      // 1、数组翻转可以使 v-for 更新
      this.arr.reverse();
    },
    sliceBtn() {
      // 2、数组 slice 不会使 v-for 更新
      // slice(0, 3) 截取数组的前三个，但不包括参数为3的数据
      let re = this.arr.slice(0, 3);
      console.log(re);
    },
    updateBtn() {
      // 3、更新某个值的时候，v-for 是监测不到的
      // this.arr[0] = 100;

      // 使用 this.$set(要更新的目标结构，要更新元素的索引，要更改成的值)
      this.$set(this.arr, 0, 100);
    }
  }
};
</script>

<style>
</style>
~~~





### 8、v-for 更新性能

v-for 更新时是如何操作 DOM 的？

循环出新的**虚拟 DOM 结构**，和旧的虚拟 DOM 结构对比，尝试复用标签就地更新内容。

<img src="C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220512182555147.png" alt="image-20220512182555147" style="zoom:50%;" />

~~~vue
<template>
  <div>
    <li v-for="(val,ind) in arr" :key="ind">{{val}}</li>
    <button @click="btn">在下标为1的位置插入新数据</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: ["老大", "老二", "老三"]
    };
  },
  methods: {
    btn() {
      // splice(要删除的位置,删除个数，要添加的元素)
      // 当有第三个参数时，第一个参数删除改为添加
      this.arr.splice(1, 0, "老几");
    }
  }
};
</script>

<style>
</style>
~~~







### 9、虚拟 DOM

**真实 DOM：**在 document 对象上，渲染到浏览器上显示的标签。**（真实DOM 的属性过多，遍历耗时）**

**虚拟 DOM：**存在于内存中的一个JS 对象，本质是保存节点信息，属性和内容的一个 JS 对象。



- **虚拟 DOM 好处？**

提高 DOM 更新的性能，不频繁操作真实 DOM，在内存中找到变化部分，再更新真实 DOM（打补丁）





### 10、diff 算法

diff 算法如何比较新旧虚拟 DOM ？   ==>    **同级比较**

根元素变化？    ==>   **删除重新建立整个 DOM树**

根元素未变，属性改变？    ==>    **DOM 复用，只更新属性**





### 11、Key 的作用

**子元素或者内容改变会分 diff 哪 2 种情况比较？**

- 无 key ,就地更新
- 有 key ,按照 key 比较

**key 值要求是？**

- 唯一不重复的字符串或者数值

**key 应该怎么用？**

- 有 id 用  id ,无 id 用索引

**key 的好处？**

可以配合虚拟 DOM 提高更新的性能





### 12、阶段小结

- v-for  什么时候更新页面呢？  ==>    数组采用更新方法，才会导致  v-for  更新页面
- vue 是如何提高更新性能的?   ==>     采用虚拟 DOM + diff  算法提高更新性能
- 虚拟 DOM 是什么？   ==>      本质是保存  dom  关键信息的 JS 对象
- diff  算法如何比较新旧虚拟 DOM？
  - 根元素改变    ==>   删除当前  DOM 树重新建
  - 根元素未变，属性改变   ==>    更新属性
  - 根元素未变，子元素/内容改变     ==>   无key ,就地更新/  有 key ,按key 比较





## 2、动态class

### 1、如何给标签class属性动态赋值？

**语法：  ：class = "{类名 ： 布尔值}" ， true 使用， false 不用**

~~~vue
<template>
  <div>
    <!-- 语法： ：class="{类名： 布尔值}" -->
    <!-- 布尔值决定要不要给该标签添加类，使用场景：vue 变量控制标签是否是否应该有类名 -->
    <p :class="{red_str:bool}">动态class</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bool: true
    };
  }
};
</script>

<style scoped>
.red_str {
  color: red;
}
</style>
~~~





### 2、动态 style

**语法：           ：style = "{css:属性名：值}"**

~~~vue
<template>
  <div>
    <!-- 语法： ：style = "{css:属性名：值}" -->
    <!-- <p :style="{backgroundColor: 'red'}">改变样式</p>   也可以直接写属性值 -->
    <p :style="{backgroundColor: colorStr}">改变样式</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      colorStr: "yellow"
    };
  }
};
</script>

<style>
</style>
~~~





## 3、案例：品牌管理

### 1、品牌数据铺设

**需求：**

1. 把默认数据显示到表格上
2. 把资产超过100的，都用红色字体标记出来

**分析：**

1. 先铺设静态页面    ---  去 .md 文档里，复制数据和标签模板
2. 此案例使用  Bootstrap ，需要下载，并导入到工程 main.js 中
3. 用 v-for 配合默认数据，把数据默认铺设到表格上显示
4. 直接在标签上，大于100价格，动态设置red类名

**用到了哪些技术点？**

- 布局使用 bootstrap,yarn 下载引入使用
- v-for 把数据渲染到表格里
-  ：class 动态判断价格给红色类名

~~~vue
<template>
  <div id="app">
    <div class="container">
      <!-- 顶部框模块 -->
      <div class="form-group">
        <div class="input-group">
          <h4>品牌管理</h4>
        </div>
      </div>

      <!-- 数据表格 -->
      <table class="table table-bordered table-hover mt-2">
        <thead>
          <tr>
            <th>编号</th>
            <th>资产名称</th>
            <th>价格</th>
            <th>创建时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <!-- { id: 100, name: "外套", price: 199, time: new Date("2010-08-12") }, -->
          <tr v-for="obj in list" :key="obj.id">
            <td>{{obj.id}}</td>
            <td>{{obj.name}}</td>

            <!-- 如果价格超过100，就有red这个类 -->
            <td :class="{red:obj.price > 100}">{{obj.price}}</td>
            <td>{{obj.time}}</td>
            <td>
              <a href="#">删除</a>
            </td>
          </tr>
        </tbody>
        <!-- 
        <tfoot >
          <tr>
            <td colspan="5" style="text-align: center">暂无数据</td>
          </tr>
        </tfoot>
        -->
      </table>

      <!-- 添加资产 -->
      <form class="form-inline">
        <div class="form-group">
          <div class="input-group">
            <input type="text" class="form-control" placeholder="资产名称">
          </div>
        </div>&nbsp;&nbsp;&nbsp;&nbsp;
        <div class="form-group">
          <div class="input-group">
            <input type="text" class="form-control" placeholder="价格">
          </div>
        </div>&nbsp;&nbsp;&nbsp;&nbsp;
        <!-- 阻止表单提交 -->
        <button class="btn btn-primary">添加资产</button>
      </form>
    </div>
  </div>
</template>

<script>
// 1、明确需求
// 2、标签 + 样式 + 默认数据
// 3、下载 bootstrap,并在main.js 引入 bootstrap.css
// 4、把list 数组，铺设到表格
export default {
  data() {
    return {
      name: "", // 名称
      price: 0, // 价格
      list: [
        { id: 100, name: "外套", price: 199, time: new Date("2010-08-12") },
        { id: 101, name: "裤子", price: 34, time: new Date("2013-09-01") },
        { id: 102, name: "鞋", price: 25.4, time: new Date("2018-11-22") },
        { id: 103, name: "头发", price: 19900, time: new Date("2020-12-12") }
      ]
    };
  }
};
</script>

<style >
.red {
  color: red;
}
</style>
~~~





### 2、品牌数据增加，功能实现

**分析：**

1. 添加资产按钮-- 绑定点击事件
2. 给表单 v-model 绑定 vue 变量收集用户输入内容
3. 添加数组到数组中
4. 判断用户内容是否符合规定

**用到了哪些技术点？**

- @绑定事件
- v-model 收集表单数据
- .prevent 阻止按钮提交表单刷新页面





### 3、品牌数据删除，功能实现

需求1：点击删除的 a 标签，删除数据

需求2：删除没数据了要提示暂无数据的 tfoot

**分析：**

1. a 标签绑定点击事件
2. 给事件方法传 id
3. 通过 id ,找到对应数据删除
4. 删除光了要让 tfoot 显示
5. 删除光了再新增，有bug(id 值问题) 需要修复

**用到了哪些技术点？**

- 事件传 id
- 删除数组元素方法
- id 值判断问题



**最后的代码：**

~~~vue
<template>
  <div id="app">
    <div class="container">
      <!-- 顶部框模块 -->
      <div class="form-group">
        <div class="input-group">
          <h4>品牌管理</h4>
        </div>
      </div>

      <!-- 数据表格 -->
      <table class="table table-bordered table-hover mt-2">
        <thead>
          <tr>
            <th>编号</th>
            <th>资产名称</th>
            <th>价格</th>
            <th>创建时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <!-- { id: 100, name: "外套", price: 199, time: new Date("2010-08-12") }, -->
          <tr v-for="obj in list" :key="obj.id">
            <td>{{obj.id}}</td>
            <td>{{obj.name}}</td>

            <!-- 如果价格超过100，就有red这个类 -->
            <td :class="{red:obj.price > 100}">{{obj.price}}</td>
            <td>{{obj.time}}</td>
            <td>
              <a href="#" @click="delFn(obj.id)">删除</a>
            </td>
          </tr>
        </tbody>

        <tfoot v-show="list.length === 0">
          <tr>
            <td colspan="5" style="text-align: center">暂无数据</td>
          </tr>
        </tfoot>
      </table>

      <!-- 添加资产 -->
      <form class="form-inline">
        <div class="form-group">
          <div class="input-group">
            <input type="text" class="form-control" placeholder="资产名称" v-model="name">
          </div>
        </div>&nbsp;&nbsp;&nbsp;&nbsp;
        <div class="form-group">
          <div class="input-group">
            <input type="text" class="form-control" placeholder="价格" v-model.number="price">
          </div>
        </div>&nbsp;&nbsp;&nbsp;&nbsp;
        <!-- 阻止表单提交 -->
        <button class="btn btn-primary" @click.prevent="addFn">添加资产</button>
      </form>
    </div>
  </div>
</template>

<script>
// 1、明确需求
// 2、标签 + 样式 + 默认数据
// 3、下载 bootstrap,并在main.js 引入 bootstrap.css
// 4、把list 数组，铺设到表格

// 5、
export default {
  data() {
    return {
      name: "", // 名称
      price: 0, // 价格
      list: [
        { id: 100, name: "外套", price: 199, time: new Date("2010-08-12") },
        { id: 101, name: "裤子", price: 34, time: new Date("2013-09-01") },
        { id: 102, name: "鞋", price: 25.4, time: new Date("2018-11-22") },
        { id: 103, name: "头发", price: 19900, time: new Date("2020-12-12") }
      ]
    };
  },
  methods: {
    addFn() {
      if (this.name.trim().length === 0 || this.price === 0) {
        alert("资产名称或价格不能为空！");
        return;
      }
      // 把值以对象形式,插入 list
      // 解决bug:当所有数据删掉后，再新增时，id 需要一个固定的值
      let id =
        this.list.length > 0 ? this.list[this.list.length - 1].id + 1 : 100;
      this.list.push({
        // 当前数组最后一个对象的 id + 1作为新对象 id值
        id: id,
        name: this.name,
        price: this.price,
        time: new Date()
      });
      this.name = "";
      this.price = "";
    },
    delFn(id) {
      // 通过 id 找到这条数据在数组中的下标
      let index = this.list.findIndex(obj => obj.id === id);
      this.list.splice(index, 1);
    }
  }
};
</script>

<style >
.red {
  color: red;
}
</style>
~~~





## 4、vue 基础 -过滤器

### 1、过滤器的定义使用

**过滤器：过滤器就是一个函数，其实就是传入一个值，然后返回一个处理后的值。**

**用法：**过滤器只能用在，**插值表达式** 和 **v-bind** 动态属性里



**vue中过滤器的使用场景**

- 字符串翻转，"输入 hello"  "输出olleh"
- 字母大写，输入"hello"  "输出 HELLo"

语法：

- Vue.filter("过滤器名"，值) => {return  "返回处理后的值"}
- filters("过滤器名": 值) => {return  "返回处理后的值"}

~~~vue
<template>
  <div>
    <p>原来的样子：{{ msg }}</p>
    <!-- 2、过滤器的使用
    语法：{{ 值 | 过滤器名字 }}-->
    <p>使用翻转过滤器:{{ msg|reverse }}</p>
    <p :title="msg | toUp">鼠标长停，提示信息大写</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: "Hello, Vue"
    };
  },
  // 方式2：局部 - 过滤器
  // 这个只能在当前的 vue 文件使用，但是可以定义多个，所以加了s
  /*
  语法：
  filters: {
    过滤器名字（val）{
      return  处理后的值
    }
  }
  */
  filters: {
    toUp(val) {
      return val.toUpperCase();
    }
  }
};
</script>

<style>
</style>
~~~



### 2、过滤器的传参和多个过滤器的一起使用

**目标：**可同时使用多个过滤器，或者给过滤器传参

**语法：**

- **过滤器传参：vue 变量 | 过滤器（传参）**
- **多个过滤器：vue 变量  |  过滤器1 |  过滤器2**  



## 5、export default 的配置项

**1、 data()** {

​    **return** {

定义变量

​     }

}

  **2、methods:** {

  定义函数

​      }

​     

 **3、 filters:** {

定义过滤器

  }



## 6、yarn serve 老是问题，所以用npm run  serve





## 7、计算属性 - 基础

### 1、计算属性使用场景

当变量的值，需要通过别人计算而得来

### 2、计算属性的特点

函数内使用的变量改变，会自动更新并返回结果

>  // 注意：计算属性和 data 属性都是变量，不能重名

~~~vue
<template>
  <div>
    <p>{{num}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      a: 10,
      b: 20
    };
  },
  // 计算属性：
  // 场景：一个变量的值，需要用另外变量计算而得来
  /*
  computed:{
    计算属性名() {
      return 值
    }
  }
  */
  computed: {
    num() {
      return this.a + this.b;
    }
  }
  // 注意：计算属性和 data 属性都是变量，不能重名
};
</script>

<style>
</style>
~~~





### 3、计算属性 - 缓存特性

**目标：计算属性，基于依赖项的值进行缓存，依赖的变量不变，都直接从缓存取结果**

其实就是第一次遇到计算属性时，渲染计算属性，把函数返回值存入缓存，下次再次遇到直接从缓存取值，不再调用函数。

**计算属性好处是？**

- 带缓存，只有依赖项不变，都直接从缓存取
- 依赖项改变函数自动执行并重新缓存

**计算属性使用场景？**
当变量值，依赖其他变量计算而得来才用

~~~vue
<template>
  <div>
    <!-- 计算属性的优势：
    带缓存
    计算属性对应函数执行后，会把return 值缓存起来，多次调用都是直接从缓存取值
    依赖项值-变化，函数会“自动”重新执行- 并缓存新的值-->
    <!-- 计算属性 -->
    <p>{{reverseMessage}}</p>
    <p>{{reverseMessage}}</p>
    <p>{{reverseMessage}}</p>

    <!-- 普通函数 -->
    <p>{{getMessage()}}</p>
    <p>{{getMessage()}}</p>
    <p>{{getMessage()}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: "Hello, Vue"
    };
  },
  computed: {
    reverseMessage() {
      return this.msg
        .split("")
        .reverse()
        .join("");
    }
  },
  methods: {
    getMessage() {
      return this.msg
        .split("")
        .reverse()
        .join("");
    }
  }
};
</script>

<style>
</style>
~~~







### 4、计算属性 - 完整写法

语法：

~~~
computed:{
	"属性名"：{
	  set(值) {
	  
	  },
	  get(){
	   return "值"
	  }
   }
}
~~~

**何时用计算属性完整写法？**

- 给计算属性变量赋值时

**set函数和 get 函数什么时候执行？**

- set 接收要赋予的值
- get 里要返回给这个计算属性具体值







## 8、案例：复选框

### 1、小选框影响全选

**小选框如何影响全选框选中状态的？**

- v-model 给全选框绑定 isAll 计算属性
- isAll 计算属性里返回统计小选框选中状态的结果



### 2、全选影响单选

~~~vue
 // 5、计算属性 - isAll
  computed: {
    isAll: {
      set(val) {
        // 7、全选框 - 选中状态（true/false）
        // 把全选按钮的值，用循环赋值给 obj.c
        this.arr.forEach(obj => (obj.c = val));
      },
      get() {
        // 统计小选框状态  -> 全选状态
        // every:查找数组里不符合条件，直接返回 false
        return this.arr.every(obj => obj.c === true);
      }
    }
  }
~~~





### 3、反选

**反选是如何实现的？**

​	遍历每个对象，把 c 属性的值取反再赋予回去，利用遍历把对象都取出来。



### 4、案例完整代码

~~~vue
<template>
  <div>
    <span>全选:</span>
    <!-- 4、v-model 关闭全选 - 选中状态 -->
    <input type="checkbox" v-model="isAll">
    <button @click="btn">反选</button>
    <ul>
      <li v-for="(obj,index) in arr" :key="index">
        <input type="checkbox" v-model="obj.c">
        <span>{{obj.name}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
// 目标：铺设页面
// 1、标签 + 样式 + js准备好
// 2、把数据循环展示到页面上
export default {
  data() {
    return {
      arr: [
        {
          name: "猪八戒",
          c: false
        },
        {
          name: "孙悟空",
          c: false
        },
        {
          name: "唐僧",
          c: false
        },
        {
          name: "白龙马",
          c: false
        }
      ]
    };
  },
  // 5、计算属性 - isAll
  computed: {
    isAll: {
      set(val) {
        // 7、全选框 - 选中状态（true/false）
        // 把全选按钮的值，用循环赋值给 obj.c
        this.arr.forEach(obj => (obj.c = val));
      },
      get() {
        // 统计小选框状态  -> 全选状态
        // every:查找数组里不符合条件，直接返回 false
        return this.arr.every(obj => obj.c === true);
      }
    }
  },
  methods: {
    btn() {
      // 8、让数组里对象的 c 属性取反再赋予回去
      // 利用遍历把对象都取出来
      this.arr.forEach(obj => (obj.c = !obj.c));
    }
  }
};
</script>
~~~





## 9、侦听器

### 1、侦听器基础

目标：可以侦听 data/computed 属性值的改变

~~~vue
watch:{
      被侦听的属性名 (newVal, oldVal) { 
      }
}
~~~

~~~vue
<template>
  <div>
    <input type="text" v-model="name">
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: ""
    };
  },
  // 目标:侦听到 name 值的改变
  /*语法：
    watch:{
      被侦听的属性名 (newVal, oldVal) {
        
      }
    }
  */

  watch: {
    name(newVal, oldVal) {
      console.log(newVal, oldVal);
    }
  }
};
</script>

<style>
</style>

~~~





### 2、侦听器 - 深度侦听和立即执行

目标：侦听复杂类型，或者立即执行侦听函数

~~~
语法：
watch ;{
	要侦听的属性名 ：{
		immediate:true,  // 立即执行
		deep:true,  // 深度侦听
		handler(newVal,oldVal) {
		
		}
	}
}
~~~



~~~vue
<template>
  <div>
    <input type="text" v-model="user.name">
    <input type="text" v-model="user.age">
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {
        name: "",
        age: 0
      }
    };
  },

  watch: {
    user: {
      handler(newVal, oldVal) {
        // 这里侦听的是 user 对象
        console.log(newVal, oldVal);
      },
      deep: true, // 深度侦听
      immediate: true // 立即侦听（打开页面，马上侦听一次）
    }
  }
};
</script>

<style>
</style>

~~~





