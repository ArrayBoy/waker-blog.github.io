## 前端优化指标

```javascript
1. FP，First Paint 首次绘制: 标记浏览器渲染任何在视觉上不同于导航前屏幕内容之内容的时间点.

2. FCP，First Contentful Paint 首次内容绘制: 标记浏览器渲染来自 DOM 第一位内容的时间点，该内容可能是文本. 图像. SVG 甚至 元素.

3. FMP，First Meaning Paint 首次有效绘制: 例如，在 YouTube 观看页面上，主视频就是主角元素. 看这个csdn的网站不是很明显, 这几个都成一个时间线了, 截个weibo的看下. 下面的示例图可以看到, 微博的博文是主要元素.

4. ATF，Above The Fold 首屏时间

5. TTI，Time To Interact 首次交互时间，可以用DomReady时间。

6. 资源总下载时间 Load时间 >= DomContentLoaded时间

   * Dom加载完时间：DomContentLoaded。

   * 页面资源加载完时间：Load，包括图片，音视频等异步资源。但是资源加载完之后，页面还没有完全稳定，完全稳定的时间由finish决定。

7. 服务端重要接口加载速度。

8. 客户端启动容器（WebView）时间。
```

## 优化方向

前端性能优化分为两个方向，一是工程化方向，另一个是代码细节方向

#### 1. 工程化方向

```js
1. 客户端Gzip离线包，服务器资源Gzip压缩。

2. JS瘦身，Tree shaking，ES Module，动态Import，动态Polyfill。

3. 图片加载优化，Webp，考虑兼容性，可以提前加载一张图片，嗅探是否支持Webp。

4. 延迟加载不用长内容。通过打点，看某些弹窗内或者子内容是否要初始化加载。

6. 服务端渲染，客户端预渲染。

7. CDN静态资源

8. Webpack Dll，通用优先打包抽离，利用浏览器缓存。

9. 骨架图

10. 数据预取，包括接口数据，和加载详情页图片。

11. Webpack本身提供的优化，Base64，资源压缩，Tree shaking，拆包chunk。

12. 减少重定向。
```

#### 2. 代码细节方向

```js
1. 图片，图片占位，图片懒加载。 雪碧图

2. 使用 prefetch / preload 预加载等新特性

3. 服务器合理设置缓存策略

4. async（加载完当前js立即执行）/defer(所有资源加载完之后执行js)

5. 减少Dom的操作，减少重排重绘

6. 从客户端层面，首屏减少和客户端交互，合并接口请求。

7. 数据缓存。

8. 首页不加载不可视组件。

9. 防止渲染抖动，控制时序。

10. 减少组件层级。

11. 优先使用Flex布局。
```

## 卡顿问题解决

```javascript
1. CSS动画效率比JS高，css可以用GPU加速，3d加速。如果非要用JS动画，可以用requestAnimationFrame。

2. 批量进行DOM操作，固定图片容器大小，避免屏幕抖动。

3. 减少重绘重排。

4. 防抖和节流。

// 防抖
// 核心要点:如果在定时器的时间范围内再次触发，则清除前面的重新计时。替换操作。
// 防抖函数：将多次触发变成最后一次触发；
// 使用场景：提交按钮的点击事件。
function debounce(fn, wait) {
    let timer = null;
    return function () {
        let arg = arguments; //arguments保存事件回调函数中的参数
        if (timer) {
            clearTimeout(timer);
            timer = null;
        }
        timer = setTimeout(() => {
            fn.apply(this, arg) //使用apply改变传入的fn方法中的this指向，指向绑定事件的DOM元素。
        }, wait)
    }
}
function clg() {
    console.log('clg')
}
window.addEventListener('resize',debounce(clg,1000))

// 节流
// 核心要点:如果在定时器的时间范围内再次触发，则不予理睬，等当前定时器完成，才能启动下一个定时器。
// 节流函数：将多次执行变成每隔一个时间节点去执行的函数
// 节流：时间比较+定时器____________比较完全实现
// 1、第一次触发时候需要执行
// 2、单位时间内触发只会触发一次、
// 3、最后一次触发、执行一次
function throttle(fn, wait) {
    let timer, startTime = 0;
    var wait = wait || 160;
    return function (...args) {
        const nowDate = Date.now();
        if (nowDate - startTime >= wait) {
            // 执行第一次、以及单位时间内执行一次
            startTime = nowDate;
            fn.apply(this, args);
        } else {
            // 单位时间内多次触发、清除、重新启动
            clearTimeout(timer);
            timer = setTimeout(() => {
                startTime = nowDate;
                // 执行最后一次
                fn.apply(this, args);
            }, wait)
        }
    }
}

function sayHi() {
    console.log('hi')
}
setInterval(throttle(sayHi,1000),500)

5. 减少临时大对象产生，利用对象缓存，主要是减少内存碎片。

6. 异步操作，IntersectionObserver，PostMessage，RequestIdleCallback。
```

## 性能优化 API

```javascript
1. Performance。performance.now()与new Date()区别，它是高精度的，且是相对时间，相对于页面加载的那一刻。但是不一定适合单页面场景。

2. window.addEventListener("load", ""); window.addEventListener("domContentLoaded", "");

3. Img的onload事件，监听首屏内的图片是否加载完成，判断首屏事件。

4. RequestFrameAnmation 和 RequestIdleCallback。

5. IntersectionObserver,MutationObserver，PostMessage。

6. Web Worker，耗时任务放在里面执行。
```

## 检测工具

```javascript
1. Chrome Dev Tools

2. Page Speed

3. Jspref
```

## 优化细节

1. DNS 预解析

> DNS 解析也是需要时间的，可以通过预解析的方式来预先获得域名所对应的 IP。这样就能节省第一步通过域名查找 ip 的时间，比如知乎：

```javascript
<link rel="dns-prefetch" href="//www.zhihu.com">
```

2. 利用浏览器缓存

> 不需要修改的文件使用强缓存，html 使用协商缓存，利用 webpack 打包生成文件名+hash 值，当文件内容有改变的时候。生成文件名的 hash 值也会改变，这样文件名就变了，浏览自然会重新请求，而不是用缓存中的文件。当文件内容没有改变的时，hash 值不变、文件名不变，浏览使用缓存的文件，不发送请求

3. 图片懒加载

> 当访问一个页面的时候，先把 img 元素或是其他元素的背景图片路径替换成一张大小为 1\*1px 图片的路径（这样就只需请求一次），当图片出现在浏览器的可视区域内时，才设置图片真正的路径，让图片显示出来。这就是图片懒加载。

如何加载图片

```html
<div class="img-container">
  <img data-src="./01.jpeg" alt="" />
</div>
<div class="img-container">
  <img data-src="./02.jpg" alt="" />
</div>
<div class="img-container">
  <img data-src="./03.jpeg" alt="" />
</div>
```

只需要把 data-src 中的地址放到 src 的里面就好了

```javascript
imgs[i].src = imgs[i].getAttribute("data-src");
```

如何判断一个元素出现在视野中？

元素相对顶点的距离(文字) <= 窗口高度 + 滚动的距离

```javascript
const imgs = document.querySelectorAll("img");
// 获取可视区域高度
const viewHeight = window.innerHeight || window.documentElement.clientHeight;
console.log(viewHeight, imgs);
function loadImg() {
  for (let i = 0; i < imgs.length; i++) {
    console.log(imgs[i].getBoundingClientRect().top);
    let dis = viewHeight - imgs[i].getBoundingClientRect().top;
    if (dis > 0) {
      imgs[i].src = imgs[i].getAttribute("data-src");
    }
  }
}
loadImg();
window.addEventListener("scroll", loadImg);
```

4. js 文件异步或延迟加载

```javascript
<script async src="script.js">   //async属性则是下载不会阻塞html解析，但是执行还是会阻塞。

<script defer src="script.js">   //defer属性的script的下载不会阻塞html解析，而且其会在解析完成后才执行。
```

5. http2 的头部压缩

> 现代浏览器都支持 gzip 压缩并会为所有 HTTP 请求自动协商此类压缩。启用 gzip 压缩可大幅缩减所传输的响应的大小（最多可缩减 90%），从而显著缩短下载相应资源所需的时间、减少客户端的流量消耗并加快网页的首次呈现速度。

6. 减少网络请求数

我们可以尽量将多个请求合并到一个里，避免多次请求。阿里云就支持文件的 combo，比如下面的页面：

```javascript
<script type="text/javascript" src="//b.aliyun.com/a.js"></script>
<script type="text/javascript" src="//b.aliyun.com/b.js"></script>
<script type="text/javascript" src="//b.aliyun.com/c.js"></script>
<script type="text/javascript" src="//b.aliyun.com/d.js"></script>
<script type="text/javascript" src="//b.aliyun.com/e.js"></script>
<script type="text/javascript" src="//b.aliyun.com/f.js"></script>
<script type="text/javascript" src="//b.aliyun.com/g.js"></script>

//通过combo功能改写成：

<script type="text/javascript" src="//b.aliyun.com/a.js??a.js,b.js,c.js,d.js,e.js,f.js,g.js"></script>
```

**要注意的是不要过度合并，防止下载的文件资源太大，比如 webpack 合并成 bundle.js，原则上“只发送用户需要的文件”**

7. 使用 CDN

> 使用 CDN 的时候更多的是使用的是 CDN 的静态缓存功能，其实 CDN 也是有能力缓存动态内容的，我们基于这一块做了一个自动匹配确实的 auto-polyfill 服务。它能够根据用户的浏览器型号，补全用户所需要的 polyfill，这样可以最大化地降低用户需要下载的 js 文件大小，进而改善用户体验。

8. 标准文档结构

   - 文件顺序
   - 语义化标签
   - 尽量少用 iframe, table 布局，多用 flex

9. 充分利用本地缓存

> 除了浏览器本身缓存，用户自己也可以实现本地缓存。甚至有工程体系为了降低网络的流量，自己去实现一套增量更新前端文件的体系，每次只需要拉去 patch 而不是全量数据。如果能把用户端的缓存利用起来，还是能对应用表现有很大提升的。美团做了一套这样的体系，而且 IBM 的博客也有这样实现的介绍，在追求省流量的场景下，这种方法就很有效果。

10. Service Worker

##### 什么是 Service Worker?

> Service Worker 本质上充当 Web 应用程序与浏览器之间的代理服务器，也可以在网络可用时作为浏览器和网络间的代理。它们旨在（除其他之外）使得能够创建有效的离线体验，拦截网络请求并基于网络是否可用以及更新的资源是否驻留在服务器上来采取适当的动作。他们还允许访问推送通知和后台同步 API。

> Service worker 可以解决目前离线应用的问题，同时也可以做更多的事。Service Worker 可以使你的应用先访问本地缓存资源，所以在离线状态时，在没有通过网络接收到更多的数据前，仍可以提供基本的功能（一般称之为 Offline First）。这是原生 APP 本来就支持的功能，这也是相比于 web app ，原生 app 更受青睐的主要原因。

service worker 能做些什么?

```javascript
后台消息传递

网络代理，转发请求，伪造响应

离线缓存

消息推送

...
```

##### 注册 service worker

```javascript
// service worker API 是否可用，如果可用， service worker/sw.js 被注册。如果这个 service worker 已经被注册过，浏览器会自动忽略上面的代码。
if ("serviceWorker" in navigator) {
  navigator.serviceWorker
    .register("/sw.js")
    .then(function (registration) {
      // Registration was successful
      console.log(
        "ServiceWorker registration successful with scope: ",
        registration.scope
      );
    })
    .catch(function (err) {
      // registration failed :(
      console.log("ServiceWorker registration failed: ", err);
    });
}
```

##### 激活 service worker

> 在你的 service worker 注册之后，浏览器会尝试为你的页面或站点安装并激活它。install 事件会在安装完成之后触发。install 事件一般是被用来填充你的浏览器的离线缓存能力。你需要为 install 事件定义一个 callback ，并决定哪些文件你想要缓存.

```javascript
// The files we want to cache
var CACHE_NAME = "my-site-cache-v1";
var urlsToCache = ["/", "/css/main.css", "/js/main.js"];

self.addEventListener("install", function (event) {
  // Perform install steps
  event.waitUntil(
    caches.open(CACHE_NAME).then(function (cache) {
      console.log("Opened cache");
      return cache.addAll(urlsToCache);
    })
  );
});
```

上面的代码中，我们通过 caches.open 打开我们指定的 cache 文件名，然后我们调用 cache.addAll 并传入我们的文件数组。这是通过一连串 promise （caches.open 和 cache.addAll） 完成的。event.waitUntil 拿到一个 promise 并使用它来获得安装耗费的时间以及是否安装成功。

##### 监听 service worker

> 现在我们已经将你的站点资源缓存了，你需要告诉 service worker 让它用这些缓存内容来做点什么。有了 fetch 事件，这是很容易做到的。

> 每次任何被 service worker 控制的资源被请求到时，都会触发 fetch 事件，我们可以给 service worker 添加一个 fetch 的事件监听器，接着调用 event 上的 respondWith() 方法来劫持我们的 HTTP 响应，然后你用可以用自己的方法来更新他们。

```javascript
self.addEventListener("fetch", function (event) {
  event.respondWith(
    caches.match(event.request).then(function (response) {
      // Cache hit - return response
      if (response) {
        return response;
      }
      return fetch(event.request);
    })
  );
});
```

上面的代码里我们定义了 fetch 事件，在 event.respondWith 里，我们传入了一个由 caches.match 产生的 promise.caches.match 查找 request 中被 service worker 缓存命中的 response 。

如果我们有一个命中的 response ，我们返回被缓存的值，否则我们返回一个实时从网络请求 fetch 的结果。

#### sw-toolbox

当然，我也可以使用第三方库，例如：lishaoy.net 使用了 sw-toolbox。

11. 服务端预渲染

> 把某些页面由服务端直接代入数据生成。

12. 避免过早优化

    在我看来，过早优化的特征有二：

    - 对所要实现的系统没有一个全面的规划，是为其一；
    - 需求本身不够稳定，或对需求的理解不够透彻，是为其二。

    要避免过早优化，只需要注意两点：

    - 逐步完善系统，不追求一步到位；
    - 在考虑优化的同时，需要清楚它所带来的收益和影响范围。

[网站性能标杆检测](https://httparchive.org/)
