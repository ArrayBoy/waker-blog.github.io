1. #### android 系统 webview 嵌入网页，网页更新数据后 webview 不刷新问题？

- 方法一：加时间戳 防止缓存
  > vue 哈希模式的 所以时间戳加在后边参数无效 需要加在#后才行 所以如果需要刷新 h5 页面就加一个这样的时间戳就好了

```js
// 加时间戳 防止缓存
if (url.includes("/#")) {
  url =
    url.split("#")[0].split("?")[0] +
    "?v=" +
    new Date().getTime() +
    "/#" +
    url.split("#")[1];
}
```

处理后的数据是这样的

```js
:8080/?v=19843454334535/#/user
```

- 方法二：js 桥接，给 app 提供事件通知，让 app 端强制刷新

- 方法三：监听网页关闭，打开事件，重新发送请求数据

```js
document.addEventListener(
  "visibilitychange",
  () => {
    console.log("visibilitychange事件");
    this.getShareTimes(); //  更新数据的方法
  },
  false
);
```
