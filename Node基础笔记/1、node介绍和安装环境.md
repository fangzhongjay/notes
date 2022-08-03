## 1、回顾

### 1、为什么 JavaScript 可以在浏览器中被执行？

因为浏览器里面含有 **JavaScript 解析引擎**。

不同的浏览器使用不同的 JavaScript 解析引擎：

- Chrome 浏览器 => V8

- Firefox  浏览器 => OdinMonkey(奥丁猴) 

- Safri  浏览器 => JSCore

- IE  浏览器 => Chakra (查克拉)

  > 其中 Chrome 浏览器的 V8 解析引擎性能最好！



### 2、浏览器中的 JavaScript 运行环境

**1、运行环境**是指代码正常运行所需的必要环境。（JavaScript 的运行环境其实就是浏览器）

2、运行环境包含两部分：**JavaScript 解析引擎** 和 **内置API 的一些函数**（例如：DOM  BOM XML等等）

> 总结：1、引擎负责解析和执行JavaScript 代码
>
> ​		    2、内置API 是由运行环境提供的特殊接口，只能在所属的运行环境中被调用。



### 3、JavaScript 能否做后端开发

可以。JavaScript  借助**浏览器**这个运行环境可以做**前端开发**，借助 **Node.js**  这个运行环境可以做**后端开发**。



***





## 2、初始 Node.js

### 1、什么是 Node.js

**Node.js** 是一个基于 **Chrome V8 引擎**的 **JavaScript 运行环境**。

> 为什么是基于 Chrome V8 引擎呢？  因为 Chrome V8 引擎  在所有 js 引擎中效率最高。

Node.js 的官网地址：https://nodejs.org/zh-cn/



### 2、Node.js 中的 JavaScript 运行环境

Node.js 运行环境包含两部分： **V8 引擎  和 内置API 。**

**注意**：

1. **浏览器**是 JavaScript 的**前端运行环境。**
2. **Node.js** 是 JavaScript 的**后端运行环境。**
3. Node.js 中**无法调用 DOM 、 BOM 和 ajax 等浏览器内置API。**



### 3、Node.js 可以做什么

Node.js 作为一个 JavaScript 的运行环境，仅仅是提供了基础的功能和 API。然而 ，基于Node.js 提供的这些基础功能，很多强大的工具和框架如雨后春笋，层出不穷。所以学会了 Node.js ，可以让前端程序员胜任更多的工作和岗位。

1. 基于 Express 框架 （http://www.expressjs.comcn/）,可以快速构建 Web 应用。
2. 基于 Electron 框架 （https://electronjs.org/）,可以构建跨平台的桌面应用。
3. 基于 restify 框架（http://restify.com/）,可以快速构建 API 接口项目。
4. 读写和操作数据库、创建实用的命令行工具辅助前端开发。



**学习路径**：

1、浏览器中的 JavaScript 学习路径

JavaScript 基础语法   +  浏览器内置API (如BOM + DOM)   + 第三方库 (jQuery 、art-template等)

2、Node.js 的学习路径

JavaScript 基础语法   +  Node.js 内置API模块  (fs 、 path 、http等)   + 第三方API 模块 (express、mysql等)



### 4、查看已安装的 Node.js 的版本号

安装好 Node.js 之后，可以打开**终端**（也就是黑窗口），输入**node -v**, 按下回车，即可查看已安装的 Node.js 的版本号。

> 终端（英文名：Terminal）是专门为开发人员设计的，用于实现人机交互的一种方式。



### 5、在Node.js 环境中执行 JavaScript 代码

1、打开终端

2、输入  **node  要执行的js 文件的路径** 

![image-20220331151313736](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220331151313736.png)

也可以把终端先切换到要执行js 文件的目录下，  代码  **cd   切换到的路径**，然后直接 输入 **node  js文件名**

![image-20220331151541381](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220331151541381.png)

![image-20220331151631268](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220331151631268.png)



每次打开 cmd 终端，都要跳到 js 文件的目录下，比较麻烦。我们可以在 js 文件区域 按住 **shift 然后点击鼠标右键** （这样打开终端会直接跳到 js 文件的路径下）

<img src="C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220331152940438.png" alt="image-20220331152940438" style="zoom:50%;" />

![image-20220331153114761](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220331153114761.png)



**PowerShell 终端 是新版的终端，cmd 终端 是旧版的终端。**



### 6、终端中的快捷键

在 windows 中 powershell 或 cmd 终端中，我们可以使用如下快捷键来提供终端的效率。

- **↑**   ，可以快速定位到上一次执行的命令
- **tab**  ,  可以快速补全路径  （加入js文件名是 1234.js ,输入1 按tab 会自动补全路径）
- **esc**  ,  清空当前已经输入的命令（个人感觉没啥用）
- **cls**  ,  可以清空终端  （跟 Git 中的 clear 作用一样）



## 3、fs 文件系统模块

### 1、什么是 fs 文件系统模块？

**fs 模块**是Node.js 官方提供的、用来操作文件的模块。它提供了一系列的方法和属性，用来满足用户对文件的操作需求。

**例如：**

- **fs.readFile() 方法，用来读取指定文件中的内容**
- **fs.writeFile() 方法，用来向指定的文件中写入内容**

如果要在 JavaScript 代码中，使用 fs 模块来操作文件，则需要使用如下的方式先导入它：

 **const  fs  = require( 'fs' )**    //  利用require方法  导入 fs 模块，用 一个常量 fs  接着。



### 2、读取指定文件中的内容

**1、fs.readFile(path,  options,  callback)**  ，可以读取文件中的内容，语法格式如下：

参数解读：

参数1：**必选参数**，字符串，表示文件的路径。

参数2：**可选参数**，表示以什么**编码格式**来读取文件(默认值是utf8)。

参数3：**必选参数**，文件读取完成后，通过回调函数拿到读取的结果。

~~~javascript
// 1、导入 fs 模块，来操作文件
const fs = require('fs')

// 2、调用 fs.readFile() 方法读取文件
// 参数3：回调函数，拿到读取失败(err)和成功(dataStr)的结果

fs.readFile('./files/1.txt', 'utf8', function (err, dataStr) {
    console.log(err)  // 打印失败的结果，当读取成功时，err 的值为 null, 读取失败时，err 的值为错误对象
    console.log('--------------')
    console.log(dataStr)   // 打印成功的结果  读取失败时，dataStr 的值为undefined
})
~~~

![image-20220331223607214](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220331223607214.png)



**2、判断文件是否读取成功**

可以利用 if  来判断err 的值是否为null 

![image-20220331230040118](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220331230040118.png)



### 3、向指定的文件中写入内容

**1、fs.writeFile(file,   data,  options,  callback)**方法，可以向指定的文件写入内容，语法格式如下：

参数：

- **参数1：必选参数**，需要指定一个文件路径的字符串，表示内容的存放路径

- **参数2：必选参数**，表示要写入的内容
- **参数3：可选参数**，表示以什么格式写入文件的内容，默认值是utf8
- **参数4：必选参数**，文件写入完成后的回调函数

> 如果该路径文件**不存在**，则会创建且把**内容写入**进去，如果该文件以及**存在**，里面原来的内容则会被写入内容**替代**。

~~~javascript
// 1、导入 fs 文件系统模块
const fs = require('fs')
// 2、调用 fs.writeFile() 方法，写入文件的内容
fs.writeFile('./files/1.txt', 'assbc', function (err) {
    console.log(err)
})
~~~



### 4、案例：考试成绩整理

核心步骤：

1. **导入**需要的 fs 文件系统模块
2. 使用 **fs.readFile( )** 方法**，读取**素材目录下的 成绩.txt 文件
3. **判断**文件是否读取失败
4. 文件读取成功后，**处理成绩数据**
5. 将处理完成的成绩数据，调用 **fs.writeFile( )** 方法，写入到新文件  成绩-ok.txt中

~~~javascript
// 1、导入 fs 模块
const fs = require('fs')

// 2、调用 fs.readFile() 读取文件的内容
fs.readFile('./files/成绩.txt', 'utf8', function (err, dataStr) {
    // 3、判断是否读取成功
    if (err) {
        console.log('文件读取失败 ' + err.message)
    } else {
        // console.log('文件读取成功 ' + dataStr)
        // 4.1先把成绩的数据，按照空格进行分割
        const arrOld = dataStr.split(' ')
        // 4.2循环分割后的数组，对每一项数据，进行字符串的替换操作
        const arrNew = []
        arrOld.forEach(item => {
            arrNew.push(item.replace('=', '：'))
        })
        // 4.3把新数组中的每一项，进行合并，得到一个新数组
        const newStr = arrNew.join('\r\n')
        // console.log(newStr)

        // 5、调用 fs.writeFile() 方法，把处理完毕的成绩，写入新文件中
        fs.writeFile('./files/成绩-ok.txt', newStr, function () {
            if (err) {
                console.log('写入文件失败 ' + err.message)
            } else {
                console.log('写入文件成功 ')
            }
        })
    }
})
~~~





### 5、fs 模块 - 路径动态拼接的问题

在使用 fs 模块操作文件时，如果提供的操作路径是以 ./ 或  ../ 开头的**相对路径**时，很容易出现路径动态拼接错误的问题。

原因：代码在运行的时候，**会以执行 node 命令时所处的目录**，动态拼接出被操作文件的完整路径。

> 简单来说：就是执行 node 命令时，必须在被操作文件的目录下。



**解决方案1：在 fs 模块要写路径时，直接提供一个完整的文件存放路径。**（**缺点：移植性非常差，不利于维护）**

<img src="C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220401202156591.png" alt="image-20220401202156591" style="zoom:50%;" />



**解决方案2（好）**：node 提供的一个成员  **__dirnmae** (注意：那个是英文的双下划线) 作用：表示当前文件所处的目录。

> 其实也就是这个成员帮我们把前面的路径补全了，再加上文件名，就相当于完整路径了。

![image-20220401203059821](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220401203059821.png)



***



## 4、path 路径模块

**path 模块**是 Node.js  官方提供的、用来**处理路径**的模块。它提供了一系列的方法和属性，用来满足用户对路径的处理需求。

例如：

**path.join( )** 方法，用来**将多个路径片段拼接成一个完整的路径字符串**

**path.basename( )** 方法，用来从路径字符串中，将文件名解析出来

也是先要导入，跟 fs 模块一样。const  path = require('path')



### 1、path.join() 的代码示例

~~~javascript
const path = require('path')

// 注意：  ../ 会抵消前面的路径
const pathStr = path.join('/a', '/b/c', '../', './d', 'e')  // ../ 把 /c 抵消了， 几个../ 就抵消前面几个路径
console.log(pathStr)  // \a\b\d\e
~~~



**跟 fs 模块一起使用，跟__dirname 一起使用。**

> **注意：今后凡是涉及到路径拼接的操作，都要使用 path.join() 方法进行处理。不要直接使用 +进行字符串的拼接。**

~~~javascript
const path = require('path')
const fs = require('fs')

fs.readFile(path.join('__dirname', './files/1.txt'), 'utf8', function () {
    if (err) {
        return console.log(err.message)
    }
    console.log(dataStr)
})
~~~





### 2、获取路径中的文件名

**1、path.basename( )** 方法，可以获取**路径中的最后一部分**，经常通过这个方法获取路径中的**文件名**，语法格式如下：

**path.basename( ' path ', ' ext '  )**

- **参数1：path ,必选参数，表示一个路径的字符串**
- **参数2：ext , 可选参数，表示文件扩展名 （也就是文件名的后缀，但添加之后，返回结果会去除后缀）**

~~~javascript
const path = require('path')

// 定义文件的存放路径
const fpath = '/a/b/c/index.html'

const fullName = path.bassname(fpath)
console.log(fullName)  // index.html

const nameWithoutExt = path.bassname('fpath', 'html')  // 把文件的后缀去除了
console.log(nameWithoutExt)  // index   
~~~





### 3、path.extname() 的语法格式

1、使用 **path.extname()** 方法，可以获取路径中的**扩展名**部分。

**path.extname(path)  （返回结果是一个扩展名字符串）**

**参数1：path , 必选参数，表示一个文件的路径** 

~~~javascript
const path = require('path')
const fpath = '/a/b/c/index.html'  
const fext = path.extname(fpath)
console.log(fext)   //  .html
~~~

