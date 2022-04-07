# JS程序执行的时间线
>问题一：JS程序执行的时间线？

- document.readyState 监听js，dom变更的整个过程。触发window.onload 初始化结束

~~~
1、创建document对象，开始解析web页面。创建HTMLHtmlElement对象，添加到document中。这个阶段document.readyState = 'loading'；

2、遇到link外部css，创建线程加载，并继续解析文档。并发；

3、遇到script外部js，并且没有设置async、defer，浏览器创建线程加载，并阻塞，等待js加载完成并执行该脚本，然后继续解析文档。js拥有修改dom的能力-->domcument.write；

4、遇到script外部js，并且设置有async、defter，浏览器创建线程加载，并继续解析文档；

defer属性设置后，表示加载script外部js和解析html页面是异步，并且等到html解析完成再执行js解析后的代码；

async属性设置后，表示加载script外部js和解析html页面是异步，但是当js解析完成，立刻执行它，此时html解析是被阻塞的；

document.createElement('script')的方式动态插入script元素来模拟async属性，实现脚本异步加载和执行；

5、遇到img等，浏览器创建线程加载，并继续解析文档。并发；

6、当文档解析完成，document.readyState = 'interactive'；

7、文档解析完成后，所有设置有defer的脚本会按照顺序执行（注意与async的不同）；

8、document对象触发DOMContentLoaded事件，这也标志着程序执行从同步脚本执行阶段，转化为事件驱动阶段；

9、当所有async的脚本加载完成并执行后、img等加载完成后，document.readyState = 'complete'，window对象触发load事件；

10、从此，以异步响应方式处理用户输入、网络事件等。
~~~