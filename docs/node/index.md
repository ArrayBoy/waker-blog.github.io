#### node概念

* Node.js是一个基于Chrome V8引擎的javascipt的运行环境

* Node.js使用了一个事件驱动、非阻塞I/O的模型

* Node.js轻量又高效,能够使我们在本地运行javascript

> nodeJS底层是C++实现，node适合IO密集处理不适合cpu密集处理，换言之node不易处理需要复杂计算的任务。

#### 服务器Node.js和浏览器js的区别是什么？

* node.js是平台，JavaScript是编程语言；

* javascript是客户端编程语言，需要浏览器的javascript解释器进行解释执行；

* node.js是一个基于Chrome JavaScript运行时建立的平台，它是对Google V8引擎进行了封装的运行环境；

* node.js就是把浏览器的解释器封装起来作为服务器运行平台，用类似javascript的结构语法进行编程，在node.js上运行。

#### NodeJS中五大核心的模块

* http 开启一个Web服务，给浏览器提供服务

* url 给浏览器发送请求用，还可以传递参数(GET)

* querystring 处理浏览器通过GET/POST发送过来的参数

* path 查找文件的路径

* fs 在服务器端读取文件用的
  