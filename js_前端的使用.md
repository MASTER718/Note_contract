#  js 学习：

##  注意：

  * 补充知识
    ```yaml
    1. JavaScript的所有对象都是动态的
    2. tcp是字节流，不是bit流，bit流在物理层
      tcp传输的方式主要包括：
      1.将结构体转成字符串格式进行传输
      2.利用json进行传输 
      3.利用对象
    3. tcp传输时，如果缓存足够大，可以一次性传输多个包，这就是  TCP所谓的字节流传输，而udp是数据报传输，所以udp传输时即使缓存再大，一次也只是发送一条数据
    4. 文本文件为byte；字节为bit
    ```

   * 注意点：
     -  js是同步任务先执行完毕，才去管异步任务(**并且异步任务必须要有回调函数**)不管异步任务执行的快慢

---

###  02-21
###  对象学习
*  1-promise对象:
  所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。 
---
* 2-js默认全局对象window
   * js的函数可以嵌套，内部函数可以访问外部函数定义的变量   寻找变量(包括函数变量) 由“内”向“外”找
   *  js变量作用域实际在函数内部，例如for var i=0 循环后i任然可以使用，
   *  解决方法：使用let关键字声明一个块间的作用域变量
   *  const 声明一个常量

   - 解决对象的指向问题  在函数中  apply/call(指向对象，函数参数) 
     * apply()把参数打包成Array再传入
     * call()把参数按顺序传入
   
  ```js
  Math.max.apply(null, [3, 5, 4]); // 5
  Math.max.call(null, 3, 5, 4); // 5
  对普通函数调用，我们通常把this绑定为null。
  ```
---


* 3-重要：解构赋值
     赋值，提取属性....
    * 注意：
      有些时候，如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误
```js
// 声明变量:
var x, y;
// 解构赋值:
{x, y} = { name: '小明', x: 100, y: 200};
// 语法错误: Uncaught SyntaxError: Unexpected token =
这是因为JavaScript引擎把{开头的语句当作了块处理，于是=不再合法。解决方法是用小括号括起来：
({x, y} = { name: '小明', x: 100, y: 200});
```

---
*  4-利用arguments，你可以获得调用者传入的所有参数。也就是说，即使函数不定义任何参数，还是可以拿到参数的值
    - 也可以使用 function foo(a, b, ...rest)
    rest参数只能写在最后，前面用...标识，从运行结果可知，传入的参数先绑定a、b，多余的参数以数组形式交给变量rest，
    所以，不再需要arguments我们就获取了全部参数

```js
// foo(a[, b], c)
// 接收2~3个参数，b是可选参数，如果只传2个参数，b默认为null：
function foo(a, b, c) {
    if (arguments.length === 2) {
        // 实际拿到的参数是a和b，c为undefined
        c = b; // 把b赋给c
        b = null; // b变为默认值
    }
    
}
```

---

* 5-构造函数建立对象
  写了new关键字就可以调用函数，返回一个对象
``` js
//除了直接用{ ... }创建一个对象外，JavaScript还可以用一种构造函数的方法来创建对象。它的用法是，先定义一个构造函数：

function Student(name) {
    this.name = name;
    this.hello = function () {
        alert('Hello, ' + this.name + '!');
    }
}
//咦，这不是一个普通函数吗？
//这确实是一个普通函数，但是在JavaScript中，可以用关键字new来调用这个函数，并返回一//个对象：
var xiaoming = new Student('小明');
xiaoming.name; // '小明'
xiaoming.hello(); // Hello, 小明! 
function Student(props) {
    this.name = props.name || 'Unnamed';
}

//多个对象使用一个相同函数，减少代码与内存量
Student.prototype.hello = function () {
    alert('Hello, ' + this.name + '!');
}

   **调用构造函数千万不要忘记写new。为了区分普通函数和构造函数，按照约定，构造函数首字母应当大写，而普通函数首字母应当小写**


```


---


*  6-class继承

  ```js
  class Student {
    constructor(name) {
        this.name = name;
    }

    hello() {
        alert('Hello, ' + this.name + '!');
    }
}

  ```
class的定义包含了构造函数constructor和定义在原型对象上的函数hello()（注意没有function关键字），这样就避免了Student.prototype.hello = function () {...}这样分散的代码

---

###  02-22
###  浏览器和同异步学习

* 浏览器

  > 1. window 表示全局作用域/浏览器窗口
  > 2. location对象表示当前页面的URL信息
    > - 例如，一个完整的URL：
  http://www.example.com:8080/path/index.html?a=1&b=2#TOP
  可以用location.href获取
    > - 要获得URL各个部分的值，可以这么写：
      location.protocol; // 'http'
      location.host; // 'www.example.com'
      location.port; // '8080'
      location.pathname; // '/path/index.html'
      location.search; // '?a=1&b=2'
      location.hash; // 'TOP'
  >   3.document对象表示当前页面。由于HTML在浏览器中以DOM形式表示为树形结构，document对象就是整个DOM树的根节点
        ....自行查阅信息

---

* 1.java单线程
   - 当任务较多时这时 CPU 完全可以不管 IO 操作，挂起处于等待中的任务，先运行排在后面的任务。等到 IO 操作返回了结果，再回过头，把挂起的任务继续执行下去。这种机制就是 JavaScript 内部采用的“事件循环”机制（Event Loop）
   - 同异步任务
---
*  2.任务队列：JavaScript 运行时，除了一个正在运行的主线程，引擎还提供一个任务队列（task queue），里面是各种需要当前程序处理的异步任务。（实际上，根据异步任务的类型， 存在多个任务队列）**程序执行步骤----首先，主线程会去执行所有的同步任务。等到同步任务全部执行完，就会去看任务队列里面的异步任务。如果满足条件，那么异步任务就重新进入主线程开始执行，这时它就变成同步任务了。等到执行完，下一个异步任务再进入主线程开始执行。一旦任务队列清空，程序就结束执行**。
---
* 3.异步任务的写法及用法
    -  **异步任务的写法通常是回调函数**，一旦异步任务重新进入主线程，就会执行对应的回调函数。**如果一个异步任务没有回调函数，就不会进入任务队列，也就是说，不会重新进入主线程，因为没有用回调函数指定下一步的操作**。
   - JavaScript 引擎怎么知道异步任务有没有结果，能不能进入主线程呢？答案就是**引擎在不停地检查，一遍又一遍，只要同步任务执行完了，引擎就会去检查那些挂起来的异步任务，是不是可以进入主线程了。这种循环检查的机制，就叫做事件循环（Event Loop）。维基百科的定义是：“事件循环是一个程序结构，用于等待和发送消息和事件**（a programming construct that waits for and dispatches events or messages in a program）”。

---
* 4.异步promise对象
  -  概念：所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理
  -  promise的用法：
      * Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署；resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
       * ```js
         const promise = new Promise(function(resolve, reject) {
         // ... some code
         
         if (/* 异步操作成功 */){
         resolve(value);
         } else {
         reject(error);
         }
         });
         ```
      
  -  特点（了解即可）：
     - （1）对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
     - （2） 一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。
  - 优点：有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。
  - 使用(属性)： 
    -  ```js
        promise.then(function(value) {
        // success
        }, function(error) {
          // failure
        });
        
          
       ```
    -   **then方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态
        变为resolved时调用，第二个回调函数是Promise对象的状态变为rejected时调
        用。这两个函数都是可选的，不一定要提供。它们都接受Promise对象传出的值作为参数。**
    - **如果调用resolve函数和reject函数时带有参数，那么它们的参数会被传递给回调函数。reject函数的参数通常是Error对象的实例，表示抛出的错误；resolve函数的参数除了正常的值以外，还可能是另一个Promise实例**


---

###  02-24

- node.js的部分使用
  > - node的版本不同，引用方式也不同 
  > - package.json的理解
  
- async与await
  > - async表示函数中存在异步操作
  > - await表示紧跟在后面的表达式需要等待结果，await命令只能出现在 async函数里面

- ==Promise的详细解释== - 相应其他的在02-22

     ==Promise是一个构造函数，用于生成Promise实例==
   
   ```js
   let promise = new Promise(function(resolve,reject) {
       console.log("promise");
       resolve();
   });//定义一个promise立即执行
   
   //指定回调函数执行
   promise.then(function(){
       console.log("resolved");
   });
  console.log("hi");
  ```

  - 重要应用
    -   ==Promise.prototype.then==     then()方法直接定义在原型上，yuan
  
  

