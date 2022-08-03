## 1、ES6 模块化

### 1、回顾：Node.js 中如何实现模块化

Node.js 遵循了 **CommonJS** 的模块化规范。其中：

- 导入其他模块使用 **require( )** 方法
- 模块对外共享成员使用 **module.exports** 对象

**模块化的好处：**

大家都遵循同样的模块化规范写代码，**降低了沟通的成本，极大方便了各个模块之间的相互调用**，利人利己。





### 2、前端模块化规范的分类

在 ES6 模块化规范诞生之前， JavaScript 社区已经尝试并提出了 **AMD**、**CMD**、**CommonJS** 等模块化规范。

但是，这些由社区提出的模块化标准，还是存在一定的差异性与局限性、并不是浏览器与服务器**通用**的模块化标准，例如：

- **AMD 和 CMD** 适用于**浏览器端**的 JavaScript 模块化
- **CommonJS**  适用于**服务器端**的 JavaScript 模块化



### 3、什么是 ES6 模块化规范

ES6 模块化规范是浏览器与服务器**通用**的模块化开发规范。它的出现替代了 AMD 、 CMD 、 CommonJS。

ES6 模块化规范中定义：

- **每个 js 文件都是一个独立的模块**
- 导入其他模块成员使用 **import** 关键字
- 向外共享模块成员使用 **export** 关键字



### 4、在 node.js 中体验 ES6 模块化

因为 node.js 中**默认支持的是 CommonJS 模块化规范**，若想基于 Node.js 体验与学习 ES6 的模块化语法，可以按照如下两个步骤进行配置。

首先先检查安装的 Node.js 的版本大于  **v14.15.1**。 (在黑窗口中输入 node -v  即可检查node.js的版本)，之后在要package.json 的根节点中添加 **"type": "module"**

![image-20220506125207240](C:\Users\小灰灰\AppData\Roaming\Typora\typora-user-images\image-20220506125207240.png)





### 5、ES6 模块化的基本语法

ES6的模块化主要包含如下  **3种**  用法：**记得 import 导入时文件路径后缀要加 .js**

1. **默认导出**与**默认导入**
2. **按需导出**与**按需导入**
3. **直接导入**并**执行**模块中的代码



1、**默认导出**与**默认导入**

默认导出的语法：**export default 默认导出的成员**

~~~javascript
let n1 = 10
let n2 = 20

function show() {

}
// 默认导出，向外共享成员
export default {
    n1,
    show
}
~~~

默认导入的语法：**import 接收名称 from '导入文件的路径'**                (接收名称可以任意命名，只要符合规范)

~~~javascript
// 导入上面的模块，并使用 m1 成员进行接收
import m1 from './01默认导出.js'
console.log(m1)  // { n1: 10, show: [Function: show] }
~~~

> 注意：
>
> 1. 默认导出时，只**允许使用一次** **export default** ，否则会报错！
> 2. 默认导入时，**接收名称**可以任意名称，**只要是合法的成员名称即可**。





2、按需导出与按需导入

按需导出的语法：**export** 按需导出的成员

~~~javascript
export let s1 = 'aaa'
export let s2 = 'bbb'
export function say() { }
~~~



按需导入的语法：**import  {要导入的名称}  from  '导入的文件路径'**

~~~javascript
import { s1, s2 as str2, say } from './03按需导出.js'
console.log(s1)  // aaa
console.log(str2)  // bbb
console.log(say) // [Function: say]
~~~

> 注意：
>
> 1. 每个模块中可以使用多次按需导出
> 2. 按需导入的成员名称必须和按需导出的名称保持一致
> 3. 按需导入时，可以使用 as 关键字进行重命名
> 4. 按需导入可以和默认导入一起使用（花括号外面的就是默认导入的成员，下面举例）

~~~javascript
export let s1 = 'aaa'
export let s2 = 'bbb'
export function say() { }

export default {
    s: 111
}

// 另一个文件
import info, { s1, s2 as str2, say } from './03按需导出.js'
console.log(s1)  // aaa
console.log(str2)  // bbb
console.log(say) // [Function: say]
console.log(info) // { s: 111 }
~~~



3、直接导入并执行模块中的代码

如果只想**单纯地执行某个模块中的代码**，并不需要得到模块中向外共享的成员。此时，可以直接导入并执行模块代码，示例代码如下：

~~~javascript
// 05文件
for (let i = 0; i < 3; i++) {
    console.log(i)
}

// 06文件
// 直接导入并执行模块代码，不需要得到模块向外共享的成员
import './05直接运行模块中的代码.js'
// 0  1  2

~~~





## 2、Promise

### 1、回调地狱

**多次回调函数的相互嵌套**，就形成了**回调地狱**。示例代码如下：

~~~javascript
setInterval(() => {  // 第1层回调函数
    console.log('延迟1秒后输出')

    setInterval(() => { // 第2层回调函数
        console.log('延迟2秒后输出')

        setInterval(() => {// 第3层回调函数
            console.log('延迟3秒后输出')
        }, 3000)
    }, 2000)
}, 1000)
~~~

**回调地狱的缺点：**

- 代码**耦合性太强**，牵一发而动全身，**难以维护**
- 大量冗余的代码相互嵌套，代码的**可读性变差**

**如何解决回调地狱的问题？**

为了解决回调地狱的问题，**ES6** 中新增了 **Promise** 的概念



### 2、Promise 的基本概念

1. **Promise 是一个构造函数**
   - 我们可以创建 Promise 的实例   **const p = new Promise()**
   - new 出来的 Promise 实例对象，**代表一个异步操作**
2. **Promise.prototype 上包含一个 .then() 方法**
   - 每一次 new Promise() 构造函数得到的实例对象，都可以通过原型链的方式访问到 .then() 方法，例 p.then()
3. **.then() 方法用来预先指定成功和失败的回调函数**
   - p.then(成功的回调函数， 失败的回调函数)
   - **p.then(result =>{}，error => {} )**
   - 调用 .then() 方法时，成功的回调函数是必选的，失败的回调函数是可选的



### 3、基于 then-fs 读取文件内容

由于 **node.js 官方提供的 fs 模块**仅支持以**回调函数的方式**读取文件，不支持 **Promise 的调用方式**。因此，我们需要先安装 **then-fs 这个第三方包** ,从而支持我们基于 Promise 的方式读取文件内容。

> npm i then-fs 



调用 **then-fs 提供的 readFile()** 方法，可以异步地读取文件的内容，**它的返回值是 Promise 的实例对象**。因此**可以调用 .then() 方法**为每个Promise 异步操作指定成功和失败之后的回调函数。示例代码如下：

~~~javascript
import thenFs from 'then-fs' // 导入 then-fs 包，用 thenFS 来接收

// 基于Promise 的方式读取文件，
// 调用 then-fs 提供的readFile() 方法，返回值是Promise的实例对象，所以可以调用 .then()方法
// 注意 .then() 中的失败回调是可选的，可以被省略
thenFs.readFile('./files/1.txt', 'utf-8').then((r1) => { console.log(r1) })
thenFs.readFile('./files/2.txt', 'utf-8').then((r2) => { console.log(r2) })
thenFs.readFile('./files/3.txt', 'utf-8').then((r3) => { console.log(r3) })
~~~

**注意：上述的代码无法保证文件的读取顺序，需要做进一步的改进！**



**.then() 方法的特性**

如果上一个 .then() 方法中**返回了一个新的 Promise 实例对象**，则可以通过下一个 **.then() 继续**进行处理，通过 .then() 方法的**链式调用**，就解决了回调地狱的问题。



**基于 Promise 按顺序读取文件的内容**

**Promise 支持链式调用**，从而来解决回调地狱的问题。示例代码如下：

~~~javascript
import thenFS from 'then-fs'

// 返回值是一个新的 Promise 实例对象，就继续调用.then() 方法，返回值会按顺序打印
thenFS.readFile('./files/1.txt', 'utf8')
    .then((r1) => {
        console.log(r1)
        return thenFS.readFile('./files/2.txt', 'utf8')
    })
    .then((r2) => {
        console.log(r2)
        return thenFS.readFile('./files/3.txt', 'utf8')
    })
    .then((r3) => {
        console.log(r3)
    })
// 打印结果   111   222   333
~~~





### 4、通过 .catch() 捕获错误

在 Promise 的链式操作中如果发生了错误，可以使用 **Promise.prototype.catch()  方法**进行捕获和处理：

~~~javascript
.catch((err) => {
        console.log(err.message)
 })
当发生错误时，会导致中间的 .then() 无法正常执行，所以如果不希望前面错误导致后续的 .then 无法正常执行，则可以将 .catch 的调用提前（将 catch 放在前面）
~~~



### 5、Promise.all() 方法和 Promise.race() 方法

Promise.all() 方法会发起**并行**的 Promise 异步操作，等**所有的异步操作全部结束后**才会执行下一步的 .then 操作（等待机制）。示例代码如下：

**注意：数组中 Promise 实例的顺序就是最终结果的顺序！**

~~~javascript
import thenFS from 'then-fs'

// 定义一个数组，存放 3 个读文件的异步操作
const promiseArr = [
    thenFS.readFile('./files/1.txt', 'utf8'),
    thenFS.readFile('./files/2.txt', 'utf8'),
    thenFS.readFile('./files/3.txt', 'utf8'),
]

Promise.all(promiseArr).then(result => {
    console.log(result) // 111  222  333
})
~~~



Promise.race() 方法会发起**并行**的 Promise 异步操作，只要任何一个异步操作完成，就立即执行下一步的 **.then()  操作（赛跑机制）**示例代码如下：

~~~javascript
import thenFS from 'then-fs'

// 定义一个数组，存放 3 个读文件的异步操作
const promiseArr = [
    thenFS.readFile('./files/1.txt', 'utf8'),
    thenFS.readFile('./files/2.txt', 'utf8'),
    thenFS.readFile('./files/3.txt', 'utf8'),
]

Promise.race(promiseArr).then(result => {
    console.log(result) // 可以是111，也可能是222或333，就看并行的这三个异步操作哪个先完成
})
~~~





### 6、基于 Promise 封装读文件的方法

1、**方法的封装要求：**

1. 方法的名称要定义为  **getFile**
2. 方法接收一个**形参 fpath**, 表示要读取的文件的路径
3. 方法的**返回值**为 Promise 实例对象

**注意： new Promise () 只是创建了一个形式上的异步操作**，所以要在里面写具体的异步操作。



2、**创建具体的异步操作：**
如果想要创建**具体的异步操作**，则需要在 new Promise() 构造函数期间，传递一个function 函数，将具体的异步操作定义到function 函数内部，示例代码如下：

~~~javascript
import fs from 'fs'

function getFile(fpath) { // 方法接收一个形参 fpath,表示要读取的文件的路径
    // 方法的返回值为 Promise 的实例对象
    return new Promise(function () {
        fs.readFile(fpath, 'utf8', (err, dataStr) => {

        })
    })
}
~~~



**3、调用 resolve 和  reject 回调函数**

Promise **异步操作的结果**，可以调用 **resolve 或 reject** 回调函数进行处理。示例代码如下：

~~~javascript
import fs from 'fs'

function getFile(fpath) { // 方法接收一个形参 fpath,表示要读取的文件的路径
    // 方法的返回值为 Promise 的实例对象
    return new Promise(function (resolve, reject) { // resolve 是成功的回调函数，reject 是失败的回调函数
        fs.readFile(fpath, 'utf8', (err, dataStr) => {
            if (err) return reject(err) // 将失败的结果传给 reject 回调函数
            resolve(dataStr)    // 将成功的结果传给 resolve 回调函数
        })
    })
}

getFile('./files/1.txt').then((r1) => { console.log(r1) }, (err) => { console.log(err.message) })
~~~





### 7、async 和 await

**async 和 await** 是 **ES8** 引入的新语法，用来**简化 Promise 异步操作**。在async 和 await 出现之前，开发者只能通过**链式 .then()** 的方式处理 Promise 异步操作。

~~~javascript
// 使用ES6的新语法，来添加then-fs包
import thenFS from 'then-fs'

async function getAllFile() {
    // readFile()方法，它的返回值是 Promise 的实例对象
    const r1 = thenFS.readFile('./files/1.txt', 'utf8')
    console.log(r1)  // Promise { _40: 0, _65: 0, _55: null, _72: null }

    // 在readFile()方法，前面加一个await ,并且函数用 async 修饰，即可打印文件内容
    const r2 = await thenFS.readFile('./files/1.txt', 'utf8')
    console.log(r2)  // 111
}

getAllFile()
~~~



**async 和 await 的使用注意事项：**

1. 如果在 function 中使用 await, 则 function **必须**被 async 修饰
2. 在 async 方法中，**第一个 await 之前的代码会同步执行**，await 之后的代码会**异步执行**。（**执行机制：先执行同步，再执行异步**）

~~~
console.log('A')
async function get() {
    console.log('B')
    const r1 = await thenFS.readFile('./files/1.txt', 'utf8')
    const r2 = await thenFS.readFile('./files/2.txt', 'utf8')
    const r3 = await thenFS.readFile('./files/3.txt', 'utf8')
    console.log('D')
}

get()
console.log('C')
// 执行结果 A B C  111  222  333 D
~~~

