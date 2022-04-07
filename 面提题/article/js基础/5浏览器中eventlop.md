# 浏览器中的 Event Loop

js代码都是同步执行的，有序的放入到执行栈中。如果遇到**异步代码**，会先**挂起**到**任务队列中，**当执行栈为空时候，将EventLop需要的执行的时候，从**任务队列中**取出，放入执行栈中执行。执行结束推出执行栈。


- EventLop 主要 放在 **任务队列中**，**队列数据结构：先进先出原则**

## 任务队列

- 微任务  `process.nextTick，promise，MutationObserver` 
 - process.nextTick 是node.js中
 - MutationObserve 用于代替 Mutation events 作为观察DOM树结构发生变化时，做出相应处理的API.应用（vue源代码虚拟dom）
 - promise 是es6中的
- 宏任务  `script，setTimeout，setInterval，setImmediate，I/O，UI rendering`

   
## Event Loop 执行顺序如下所示

- 首先执行同步代码，script 属于宏任务
- 当执行完所有同步代码后，执行栈为空，查询是否有异步代码需要执行，如果有
- 执行所有微任务
- 当执行完所有微任务后，如有必要会**渲染页面**
- 然后开始下一轮 Event Loop，执行**宏任务中的异步代码**，也就是**setTimeout中的回调函数**


>这里很多人会有个误区，认为微任务快于宏任务，其实是错误的。因为宏任务中包括了script，浏览器会先执行一个宏任务，接下来有异步代码的话才会先执行微任务

## 案例代码
~~~js
        // 浏览器加载时间是 16.7
        //  0ms 1ms 浏览器还没有加载完毕。相当于 16ms以为程序时间相当于一样。所以优先顺序 ，先执行上面在执行下面
        // 0.1ms的时间间隔，相当于相同时间
        setTimeout(function(){
            console.log('8888');
        },17)
        var a = 123;
        console.log('1111')


        setTimeout(function(){
            console.log('222222');
            p.then(function(){
                console.log('6666');
            })
            console.log('7777');
        },16.7)

        var p = new Promise(function(resolve,reject){
            console.log('444');
            resolve()

        })
        p.then(function(){
            console.log('555');
        })
        console.log('33333');

        // 程序输入
        // 111  444  333  555 222

        // 1:执行script中所有同步代码
        
        // 准备
        // stackEack = []
        // 第一轮
        // script (宏任务) 中所有同步代码  ，推入 stackEack
        // 111 444 3333 宏任务结束  推出 stackEack
        // 找微任务:promise  推入 stackEack
        // 555         推出 stackEack
        // -------本轮结束 执行栈空 -----
        // 第二轮eventLop
        // 执行宏任务，setTimOut的回调函数中的同步代码
        // 222 666
        // 找微任务:promise.then
        // 777 
        // ----本轮结束 ------
        // 第三轮：eventLop
        // 执行setTimeOut中回调函数 中所有同步代码。时间17
        // 888
        // 没有微任务
        // ------本轮结束 程序结束----

~~~