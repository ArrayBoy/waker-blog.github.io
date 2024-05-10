# Base notes

## 开发模式
   * mini-base-cli
   * mini-base-ts-cli
   * mini-cloud-cli

## 文件说明
#### [小程序配置 app.json](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)

app.json 是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。QuickStart 项目里边的 app.json 配置内容如下：

```JSON
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "Weixin",
    "navigationBarTextStyle":"black"
  }
}
```

#### [工具配置 project.config.json](https://developers.weixin.qq.com/miniprogram/dev/devtools/projectconfig.html)

保存个性化配置，例如界面颜色、编译配置等

#### [页面配置 page.json](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html#%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE)

保存个性化配置，例如界面颜色、编译配置等

## 语言

#### [WXML 模板](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/)

* 小程序的 WXML 用的标签是 view, button, text 等等，这些标签就是小程序给开发者包装好的基本能力，我们还提供了地图、视频、音频等等组件能力。——对html标签的封装

* 多了一些 wx:if 这样的属性以及 {{ }} 这样的表达式

#### [WXSS 样式](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html)

* 新增了尺寸单位--rpx（responsive pixel **1rpx = 0.5px = 1物理像素**）

* 提供了全局的样式和局部样式。和前边 app.json, page.json 的概念相同，你可以写一个 app.wxss 作为全局样式，会作用于当前小程序的所有页面，局部页面样式 page.wxss 仅对当前页面生效。

* 此外 WXSS 仅支持部分 CSS 选择器

#### [JS 逻辑交互](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)

* 页面交互

```html
<view>{{ msg }}</view>
<button bindtap="clickMe">点击我</button>
```
点击 button 按钮的时候，我们希望把界面上 msg 显示成 "Hello World"

```javascript
Page({
  clickMe: function() {
    this.setData({ msg: "Hello World" })
  }
})
```

## 宿主环境

我们称微信客户端给小程序所提供的环境为宿主环境。小程序借助宿主环境提供的能力，可以完成许多普通网页无法完成的功能。

#### 渲染层和逻辑层

* **小程序的运行环境分成渲染层和逻辑层，其中 WXML 模板和 WXSS 样式工作在渲染层，JS 脚本工作在逻辑层。**

* 小程序的渲染层和逻辑层分别由2个线程管理：渲染层的界面使用了WebView 进行渲染；逻辑层采用**JsCore线程**运行JS脚本。一个小程序存在多个界面，所以渲染层存在多个WebView线程，**这两个线程的通信会经由微信客户端（下文中也会采用Native来代指微信客户端）做中转，逻辑层发送网络请求也经由Native**转发，小程序的通信模型下图所示。
![图片alt](https://res.wx.qq.com/wxdoc/dist/assets/img/4-1.ad156d1c.png)

有关渲染层和逻辑层的详细文档参考 [小程序框架](https://developers.weixin.qq.com/miniprogram/dev/framework/MINA.html) 。

## 程序与页面
微信客户端在打开小程序之前，会把整个小程序的代码包下载到本地。

紧接着通过 app.json 的 pages 字段就可以知道你当前小程序的所有页面路径,而写在 pages 字段的第一个页面就是这个小程序的首页（打开小程序看到的第一个页面）。微信客户端就把首页的代码装载进来，通过小程序底层的一些机制，就可以渲染出这个首页。

小程序启动之后，在 app.js 定义的 App 实例的 onLaunch 回调会被执行:

```javascript
App({
  onLaunch: function () {
    // 小程序启动之后 触发
  }
})
```
#### 整个小程序只有一个 App 实例，是全部页面共享的，更多的事件回调参考文档 [注册程序 App](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/app.html) 。

#### 页面解析过程
微信客户端会先根据 logs.json 配置生成一个界面，顶部的颜色和文字你都可以在这个 json 文件里边定义好。紧接着客户端就会装载这个页面的 WXML 结构和 WXSS 样式。最后客户端会装载 logs.js，你可以看到 logs.js 的大体内容就是:
```javascript
Page({
  data: { // 参与页面渲染的数据
    logs: []
  },
  onLoad: function () {
    // 页面渲染后 执行
  }
})
```
Page 是一个页面构造器，这个构造器就生成了一个页面。在生成页面的时候，小程序框架会把 data 数据和 index.wxml 一起渲染出最终的结构，于是就得到了你看到的小程序的样子。在渲染完界面之后，页面实例就会收到一个 onLoad 的回调，你可以在这个回调处理你的逻辑。

#### 有关于 Page 构造器更多详细的文档参考 注册页面  **[Page](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html)**

## 组件
小程序提供了丰富的基础组件给开发者，开发者可以像搭积木一样，组合各种组件拼合成自己的小程序。

就像 HTML 的 div, p 等标签一样，在小程序里边，你只需要在 WXML 写上对应的组件标签名字就可以把该组件显示在界面上，例如，你需要在界面上显示地图，你只需要这样写即可：
```html
<map longitude="广州经度" latitude="广州纬度"></map>
```

#### 更多的组件可以参考 [小程序的组件](https://developers.weixin.qq.com/miniprogram/dev/component/)


## API
#### [API](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/api.html)
为了让开发者可以很方便的调起微信提供的能力，例如获取用户信息、微信支付等等，小程序提供了很多 API 给开发者去使用。

要获取用户的地理位置时，只需要：
```javascript
wx.getLocation({
  type: 'wgs84',
  success: (res) => {
    var latitude = res.latitude // 纬度
    var longitude = res.longitude // 经度
  }
})
```
调用微信扫一扫能力，只需要：
```javascript
wx.scanCode({
  success: (res) => {
    console.log(res)
  }
})
```

**需要注意的是：多数 API 的回调都是异步，你需要处理好代码逻辑的异步问题。**

## 序协同工作和发布
#### [go](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/release.html#%E5%8D%8F%E5%90%8C%E5%B7%A5%E4%BD%9C)

## 开发指南
* [开发指南](https://developers.weixin.qq.com/ebook?action=get_post_info&docid=0008aeea9a8978ab0086a685851c0a)

* [官方样式库](https://github.com/Tencent/weui-wxss)

* [Flex布局](https://developers.weixin.qq.com/ebook?action=get_post_info&docid=00080e799303986b0086e605f5680a)

* [Exparser框架](https://developers.weixin.qq.com/ebook?action=get_post_info&docid=0000aac998c9b09b00863377251c0a)

## 开发笔记

* 在当前页面保存不返回首页
    > 普通编译 → 选择添加编译模式 → 确定
* 引入官方weui [地址](https://developers.weixin.qq.com/miniprogram/dev/extended/weui/quickstart.html)
    > 通过 useExtendedLib 扩展库 的方式引入，这种方式引入的组件将不会计入代码包大小,可省略 import 步骤,然后在页面的 json 文件加入 usingComponents 配置字段
    ```JSON
    {
        "usingComponents": {
            "mp-dialog": "weui-miniprogram/dialog/dialog"
        }
    }
    ```
    在对应页面的 wxml 中直接使用该组件
    ```javascript
    <mp-dialog title="test" show="{{true}}" bindbuttontap="tapDialogButton" buttons="{{[{text: '取消'}, {text: '确认'}]}}">
        <view>test content</view>
    </mp-dialog>
    ```
* 新版本已模仿vue省去了methods: 写法，采用了class写法
* 更新数据
    ```javascript
    this.setData({
        // 更新属性和数据的方法与更新页面数据的方法类似
    })
    ```
* 提交数据
    ```javascript
    this.data.formData.input
    ```
* 发送请求 [requets](https://developers.weixin.qq.com/miniprogram/dev/api/network/request/wx.request.html)
    ```javascript
    wx.request({
        url: 'test.php', //仅为示例，并非真实的接口地址
        data: submitData,
        header: {
            'content-type': 'application/json' // 默认值
        },
        method: 'POST',
        dataType: 'json', //返回的数据格式
        success(res) {
            console.log(res.data)
        },
        fail(err) {
            console.log(err.errMsg)
        },
        complete(res) {  //接口调用结束的回调函数（调用成功、失败都会执行）
            console.log(res)
        }
    })
    ```
* 网络-服务器服务器域名配置 [详情](https://developers.weixin.qq.com/miniprogram/dev/framework/ability/network.html)
    > 如果是本地开发可以勾选开发者工具详情里面的“不校验合法域名，web-view(业务域名)，TLS版本以及https证书”选项，这样就可以跨域访问本地服务了