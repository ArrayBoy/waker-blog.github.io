SQL注入是网站存在最多也是最简单的漏洞，主要原因是程序员在开发用户和数据库交互的系统时没有对用户输入的字符串进行过滤，转义，限制或处理不严谨，导致用户可以通过输入精心构造的字符串去非法获取到数据库中的数据。

##### SQL注入原理

> 一般用户登录用的SQL语句为：SELECT * FROM user WHERE username='admin' AND password='passwd'，此处admin和passwd分别为用户输入的用户名和密码，如果程序员没有对用户输入的用户名和密码做处理，就可以构造万能密码成功绕过登录验证，如用户输入'or 1#,SQL语句将变为：SELECT * FROM user WHERE username=''or 1#' AND password=''，‘’or 1为TRUE，#注释掉后面的内容，所以查询语句可以正确执行。

##### 防御（主防在后端）

> 过滤sql语句保留字