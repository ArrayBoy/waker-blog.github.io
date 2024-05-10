#### jsonp 跨域

前端

```js
  mounted() {
    this.createJsonp()
    window.jsonpCallback = data => {
      console.log(data.token)
    }
  },
  methods: {
    createJsonp() {
      let script = document.createElement('script')
      script.type = 'text/javascript'
      script.src = 'http://www.domain2.com:8080/login?callback=jsonpCallback'
      document.head.appendChild(script)
    }
  }
```

java 后端

```java
@RequestMapping(value = "/j/xxx", produces = "text/script;charset=UTF-8")
@ResponseBody
    public String getxxx(HttpServletRequest request, HttpServletResponse response, String callback,) {
        Map<String, Object> resultMap = new HashMap<>();
        JSONObject object = new JSONObject(resultMap);
    return callback + "(" + object.toString() + ")";
}
```
