#### CSRF攻击

> CSRF（Cross-site request forgery），中文名称：跨站请求伪造，也被称为：one click attack/session riding，缩写为：CSRF/XSRF。

简单的理解：攻击者盗用了你的身份，以你的名义发送恶意请求。CSRF能够做的事情包括：以你名义发送邮件，发消息，盗取你的账号，甚至于购买商品，虚拟货币转账......造成的问题包括：个人隐私泄露以及财产安全。

所以遇到CSRF攻击时，将对终端用户的数据和操作指令构成严重的威胁；当受攻击的终端用户具有管理员帐户的时候，CSRF攻击将危及整个Web应用程序。

CSRF这种攻击方式在2000年已经被国外的安全人员提出，但在国内，直到06年才开始被关注，08年，国内外的多个大型社区和交互网站分别爆出CSRF漏洞，如：NYTimes.com（纽约时报）、Metafilter（一个大型的BLOG网站），YouTube和百度HI......而现在，互联网上的许多站点仍对此毫无防备，以至于安全业界称CSRF为“沉睡的巨人”。

**直接理解：跨域携带cookie完成了CSRF，即前端请求时设置，credentials:include 后端也要设置，Access-Control-Allow-Credentials:true**

防御方案：

* 用户操作限制，比如验证码；
* 请求来源限制，比如限制HTTP Referer才能完成操作；
* token验证机制，比如请求数据字段中添加一个token，响应请求时校验其有效性；

> 第一种方案明显严重影响了用户体验，而且还有额外的开发成本；第二种方案成本最低，但是并不能保证100%安全，而且很有可能会埋坑；第三种方案，可行，token验证的CSRF防御机制是公认最合适的方案

token防御的整体思路是：

```javascript
第一步：后端随机产生一个token，把这个token保存在SESSION状态中；同时，后端把这个token交给前端页面；

第二步：下次前端需要发起请求（比如发帖）的时候把这个token加入到请求数据或者头信息中，一起传给后端；

第三步：后端校验前端请求带过来的token和SESSION里的token是否一致；
```

cookie防御的整体思路是：

```javascript
第一步：后端随机产生一个token，基于这个token通过SHA-56等散列算法生成一个密文；

第二步：后端将这个token和生成的密文都设置为cookie，返回给前端；

第三步：前端需要发起请求的时候，从cookie中获取token，把这个token加入到请求数据或者头信息中，一起传给后端；

第四步：后端校验cookie中的密文，以及前端请求带过来的token，进行正向散列验证；
```

**上面防御的思路都是利用了第三方网站不能获取其他网站的cookie或者storage**,也是目前最有用的防御和鉴权方法

* 其他就是get请求最好不带参数，post用户修改数据

* 不用轻易设置withCredentials: true,，也就是不允许跨域携带cookie

[参考](https://zhuanlan.zhihu.com/p/98062456)