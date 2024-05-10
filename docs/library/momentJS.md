#### [官网](http://momentjs.cn/)

> Moment 被设计为在浏览器和 Node.js 中都能工作。

#### 使用

- 获取当前时间戳

```js
moment().valueOf();
```

- 获取指定时间时间戳

```js
moment(time).valueOf();
```

- 获取最近一天

```js
moment().subtract(1, "days").format("x");
```

- 获取最近一周

```js
moment().subtract(1, "weeks").format("x");
```

- 获取最近三个月

```js
moment().subtract(3, "months").format("x");
```

- 获取最近一年

```js
moment().subtract(1, "years").format("x");
```

- ivew date 配置

```
options: {
        disabledDate(date) {
          return (
            date &&
            date.valueOf() >=
              moment()
                .subtract(1, 'days')
                .format('x')
          )
        },
        shortcuts: [
          {
            text: '最近一周',
            value() {
              const start = new Date(new Date().toLocaleDateString())
              const end = new Date(new Date().toLocaleDateString())
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
              end.setTime(end.getTime() - 3600 * 1000 * 24 * 1)
              return [start, end]
            }
          },
          {
            text: '最近一个月',
            value() {
              const start = new Date(new Date().toLocaleDateString())
              const end = new Date(new Date().toLocaleDateString())
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
              end.setTime(end.getTime() - 3600 * 1000 * 24 * 1)
              return [start, end]
            }
          },
          {
            text: '最近三个月',
            value() {
              const start = new Date(new Date().toLocaleDateString())
              const end = new Date(new Date().toLocaleDateString())
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
              end.setTime(end.getTime() - 3600 * 1000 * 24 * 1)
              return [start, end]
            }
          }
        ]
      },
```
