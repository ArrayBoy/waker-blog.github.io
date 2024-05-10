## 连接mysql

* 安装mysql驱动 [node-mysql](https://www.oschina.net/translate/node-mysql-tutorial)
```javascript
npm install mysql -s
```

* 配置数据库连接
```javascript
const serverConfig = {
    // 启动端口
    port: 3000,

    // 数据库配置
    database: {
        DATABASE: 'websites',
        USERNAME: 'root',
        PASSWORD: '123456',
        PORT: '3306',
        HOST: 'localhost'
    }
}
```

* 引入模块，连接mysql

```javascript
const mysql = require('mysql');
const serverConfig = require('../config/default.js')

const connection = mysql.createConnection({
    host: serverConfig.database.HOST,
    user: serverConfig.database.USERNAME,
    password: serverConfig.database.PASSWORD,
    database: serverConfig.database.DATABASE
});

connection.connect((err) => {
    if (err) {
        console.error('error connecting: ' + err.stack);
        connection.end();
        return;
    }
    console.log(`mysql connected as id ${connection.threadId}`)
});
```

* 具体查询

```javascript
const router = require('koa-router')()
const colors = require('colors-console')
const connection = require('../mysql')

//mysql 测试接口
router.post('/mysql/test', async (ctx) => {
    console.log(colors(['red', 'whiteBG', 'underline'], `请求成功！`))
    const sql = 'SELECT * FROM websites';
    await new Promise((resolve, reject) => {
        connection.query(sql, (error, result) => {
            if (error) reject(error);
            console.log(colors(['red', 'whiteBG', 'underline'], `mysql 查询成功！`))
            resolve(result);
        });
    }).then((result) => {
        ctx.body = {
            title: 'koa2 json',
            msg: 'mysql，专门给你的噢！',
            data: result
        }
    }).catch((err) => {
        console.log(err);
    });
})
```

> 注意连接时候可能会报错："Client does not support authentication protocol requested by server的问题",在项目里面通过npm install 安装的mysql和最新版本MySQL加密方式不同，导致连接失败。在最新下载的MySql客户端版本使用的是caching_sha2_password加密方式，所以默认创建的root用户和密码都是这个加密方式。而npm包里的mysql模块还是使用原来的mysql_native_password加密方式，两者不互通，连接会报错。

解决方法： 将mysql的用户密码从caching_sha2_password加密方式改回mysql模块能支持的 mysql_native_password加密方式

```javascript
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';  //将密码123456的认证修改成mysql_native_password，之后再连接mysql就会成功
```

> koa支持的是asyac/await的方式，所有要将connection.query封装成一个promise对象，这样才能正常返回res

```javascript
参考“具体查询”代码
```