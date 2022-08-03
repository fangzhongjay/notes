## 1、为什么要学习 webpack？

1. 减少文件数量
2. 缩减代码体积
3. 提高浏览器打开的速度



## 2、webpack 基本概述

### 1、什么是webpack？

**webpack本质是一个第三方模块包，用于分析，并打包代码。**

- 支持所有类型文件的打包
- 支持 less/sass => css
- 支持 ES6/7/8  => ES5
- 压缩代码，提高加载速度





### 2、webpack 的使用前准备

1. 先安装包  yarn                                     **npm  i   -g  yarn**  

2. 再初始化包环境（生成packjson文件），以前是 npm init -y ,           现在是     **yarn   init**

3. 安装依赖包（俩个）                             **yarn  add  webpack   webpack-cli  -D**

4. 再到 packjson 文件，配置 scripts(自定义命令)  

   **"scripts": {**

   ​    **"bulid": "webpack"**

     **}**

![image-20220507212421535](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220507212421535.png)





### 3、webpack 基础使用

需求：2个 js 文件 =>  打包成 1 个 js 文件

分析：

1. 一定要在 src 文件下新建资源

2. add.js  定义求和函数并导出

3. index.js  引入 add 模块并使用函数输出结果

4. 执行 “ yarn  bulid  ” 自定义命令，进行打包（确保终端路径在 src 的父级文件夹）

   <img src="C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220507214759010.png" alt="image-20220507214759010" style="zoom: 67%;" />



**webpack 如何使用？**

1. 默认在 **src 文件**下的index.js        **——打包入口文件**
2. 需要**引入**到入口的文件才会参与打包
3. 执行 package.json 里的 **bulid 命令**，执行 webpack **打包命令**
4. 执行打包命令就会在改目录下生产 **dist** 文件夹，里面有一个 **main.js 就是打包结果**





### 4、代码更新后，如何更新打包呢？

补充两个知识点：在终端返回上一级是    **cd  ..**      跳转指定是     **cd  xxxx**

代码更新后，如何打包呢？

1. **首先先确保src  的 index.js  文件的引入与使用。**
2. **然后重新执行 yarn bulid   打包命令**





### 5、如何修改 webpack 的默认入口和出口

**webpack 的配置文档：**https://webpack.docschina.org/concepts/



1. 在目录下新建 **webpack.config.js  (webpack 默认配置文件名)**
2. 在 webpack.config.js 文件下通过 **entry 和 output**  设置入口和出口的文件路径，以及出口的文件名
3. **然后重新执行 yarn bulid   打包命令**

<img src="C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220508111340395.png" alt="image-20220508111340395" style="zoom:67%;" />

~~~javascript
const path = require('path');

module.exports = {
    entry: './src/index1.js', // 打包的入口（要有这个文件）
    output: {  // 打包的出口
        path: path.resolve(__dirname, 'dist'), // 打包出口的文件夹路径和名字
        filename: 'bundle.js', // 打包出口文件的名字
    },
};
~~~





### 6、代码 yarn bulid  走了哪些流程？

![image-20220508112408591](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220508112408591.png)

**执行  webpack 命令，找到配置文件，入口和依赖关系，打包代码输出到指定位置。**







## 3、案例：隔行换色

### 1、步骤：

1. 首先先初始化包环境， **yarn init** ，    然后下载包 webpack 和 webpack-cli  ,   **yarn  add webpack   webpack-cli  -D**  ，再去packjson.js 文件**配置自定义命令  bulid**

2. yarn 下载 jquery，  **yarn  add jquery**  ，**新建 public/index.html**   ，准备一些  li 标签

3. 在 **webpack.config.js  文件**更改默认入口和出口的配置

4. **src/main.js   引入 jquery，编写功能代码**

   ~~~javascript
   // 1、yarn add jquery
   // 2、public/index.html   - 10 个li
   // 3、入口处引入 jquery
   import $ from 'jquery'
   // 4、编写隔行变色的代码
   $('#myUL>li:odd').css('color', 'red')
   $('#myUL>li:even').css('color', 'green')
   ~~~

5. 执行打包命令  **yarn  bulid**

6. **复制**  public下的index.html 到 dist 文件下，然后在  index.html **引入**打包后的 js ,右键打开网页观察效果

**注意：第六点不要忘记**





### 2、webpack-plugin 插件（自动生成 html 文件）

在上面案例中，最后的时候还有自己复制html 文件到 dist 文件夹下，还有自己引进打包后的 js 。这样太麻烦了，我们可以引进一个插件 **webpack-plugin**，当我们打包的时候回自动在 dist 文件下生成 html 文件，并且里面已经自己引进打包后的 js 了。

步骤：

1. 下载插件  **yarn add html-webpack-plugin  -D**

2. 在webpack.config.js  **添加配置**, 然后执行打包命令  **yarn  bulid**

   ~~~javascript
   const path = require('path');
   const HtmlWebpackPlugin = require('html-webpack-plugin');      // 1
   
   module.exports = {
       entry: './src/main.js',
       output: {
           path: path.resolve(__dirname, 'dist'),
           filename: 'bundle.js',
       },
       plugins: [new HtmlWebpackPlugin({  // plugins 插件配置
           template: './public/index.html'  // 告诉 webpack 使用插件时，以我们自己的html 文件作为模板去生成dist下的html 文件
       })],
   };
   ~~~



### 3、webpack 打包 css 文件

1. 新建  src/css/index.css 

2. 编写去除 li 圆点样式代码

3. 引入到入口，被webpack 打包

4. 执行打包命令观察效果

   **结果报错：因为 webpack 默认只能处理 js 文件**



**解决方案：下载加载器**

**css-loader**: webpack 解析 css 文件，把css 代码一起打包进 js中

**style-loader**: 将 css 代码插入到 DOM 上（style 标签）

步骤：

1. 下载加载器  **yarn  add  css-loader style-loader -D**

   ![image-20220508135102921](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220508135102921.png)

2. webpack.config.js 配置

   <img src="C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220508135130617.png" alt="image-20220508135130617" style="zoom: 50%;" />

3. 打包观察效果   **yarn bulid**

总结：

webpack 如何支持 css 打包？打包后端额样式在哪里？如何生效？

1. 依赖 css-loader 和 style-loader
2. css  代码会被打包进 js 文件中
3. style-loader 会把 css 代码插入到 head下的 style 标签内





### 4、webpack 打包 less 文件

跟打包 css 文件类似，一样下载 less-loader 加载器。

less-loader:识别 less 文件

**less: 将 less 编译为 css**

![image-20220508142054790](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220508142054790.png)

![image-20220508142112520](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220508142112520.png)





### 5、webpack 如何支持图片打包？对图片有哪2种处理方案？

1. webpack5 ，在rules 里，针对图片文件设置 **type : asset**
2. 小于 8 KB的转为 base64 字符串进 js 里，大于 8 KB 的输出文件

**图片转 base64 打包进 js 中好处和坏处是什么？**

1. 好处是减少浏览器发送的请求次数，读取图片速度快
2. 坏处是图片过大，因为图片转base64 占空间会多 30%左右



### 6、webpack 如何处理图标字体文件呢？

在 webpack.config.js 的 rules 里针对 字体图标文件类型设置  asset/ resourse ,直接输出到 dist 下



### 7、webpack 对 JS 语法降级

**为什么要进行降级呢？**

**因为要兼容低版本**

**babel-loader**文档：https://webpack.docschina.org/loaders/babel-loader/

**babel: 一个 javascript 编译器，把高版本 js 语法降级处理输出兼容的低版本语法**

**babel-loader**: 可以让 webpack 转译打包的 js 代码

步骤：

1. 在 src/main.js  定义箭头函数，并打印箭头函数变量（看看打包的js 文件，是否会对箭头函数降级为低版本的普通函数）

2. 下载加载器          **yarn add babel-loader @babel/core @babel/preset-env -D**

3. 在 webpack.config.js  进行配置

   ~~~javascript
   		   {
                   test: /\.m?js$/,
                   exclude: /(node_modules|bower_components)/, // 不去匹配这些文件夹下的文件
                   use: {
                       loader: 'babel-loader',  // 使用这个 loader 处理 js 文件
                       options: {   // 加载器选项
                           presets: ['@babel/preset-env'] //预设 @babel/preset-env 降级规则-按照这里的规则降级我们的 js 语法
                       }
                   }
               },
   ~~~

   



## 4、webpack 开发服务器



**问题**：每次修改代码，重新 yarn bulid 打包，才能看到最新的效果，实际工作中，打包 yarn  bulid  非常耗时。

**解决：起一个开发服务器**，缓存一些已经打包过的内容，**只重新打包修改的文件**，最终运行在内存中给浏览器使用。



### 1、为什么要使用 webpack 开发服务器呢？

1. 打包在内存中，速度快
2. 自动更新打包变化的代码，实时返回给浏览器显示



### 2、怎么使用开发服务器？

1. 下载模块包              yarn  add  webpack-dev-server  -D

2. 自定义  webpack 开发服务器启动命令     serve        -在package.json 里

   ~~~javascript
   "scripts": {
       "bulid": "webpack",
       "serve": "webpack serve"
     },
   ~~~

   

3. 启动当前工程里的 webpack 开发服务器    yarn  serve

4. 然后会在终端给我们一个地址 + 端口服务器  ，访问即可看到在内存中打包的  index.html  页面

5. 页面不再使用  yarn bulid   所以也没有 dist  文件夹，因为全部打包在服务器内存里了 



