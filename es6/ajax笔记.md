## AJAX

### AJAX 的优点

1. 可以无需刷新页面而与服务器端进行通信。
2. 允许你根据用户事件来更新部分页面内容。

### AJAX 的缺点

1. 没有浏览历史，不能回退
2. 存在跨域问题(同源)
3. SEO (搜索引擎优化)不友好

### HTTP

HTTP（hypertext transport protocol）协议『超文本传输协议』，协议详细规定了浏览器和万维网服务器之间互相通信的规则。
约定, 规则

#### 请求报文

重点是格式与参数

```
行      POST  /s?ie=utf-8  HTTP/1.1
头      Host: atguigu.com
        Cookie: name=guigu
        Content-type: application/x-www-form-urlencoded
        User-Agent: chrome 83
空行
体      username=admin&password=admin
```

#### 响应报文

```
行      HTTP/1.1  200  OK
头      Content-Type: text/html;charset=utf-8
        Content-length: 2048
        Content-encoding: gzip
空行
体      <html>
            <head>
            </head>
            <body>
                <h1>尚硅谷</h1>
            </body>
        </html>
```

- 404 找不到
- 403 拒绝请求
- 401 未授权
- 500 内容错误
- 200 成功

### express 框架

```js
//1. 引入express
const express = require("express");

//2. 创建应用对象
const app = express();

//3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get("/", (request, response) => {
  //设置响应
  response.send("HELLO EXPRESS");
});

//4. 监听端口启动服务
app.listen(8000, () => {
  console.log("服务已经启动, 8000 端口监听中....");
});
```

### 原生 AJAX

```js
//1. 创建对象
const xhr = new XMLHttpRequest();
//2. 初始化 设置请求方法和 url
xhr.open("GET", "http://127.0.0.1:8000/server");
//3. 发送
xhr.send();
//4. 事件绑定 处理服务端返回的结果
// on  when 当....时候
// readystate 是 xhr 对象中的属性, 表示状态 0(初始化) 1(open调用完毕) 2(send调用完毕) 3(服务端返回部分结果) 4(服务端返回所有结果)
// change  改变
xhr.onreadystatechange = function () {
  //判断 (服务端返回了所有的结果)
  if (xhr.readyState === 4) {
    //判断响应状态码 200  404  403 401 500
    // 2xx -> 成功
    if (xhr.status >= 200 && xhr.status < 300) {
      //处理结果  行 头 空行 体
      //响应
      // console.log(xhr.status);//状态码
      // console.log(xhr.statusText);//状态字符串
      // console.log(xhr.getAllResponseHeaders());//所有响应头
      // console.log(xhr.response);//响应体
      //设置 result 的文本
      result.innerHTML = xhr.response;
    } else {
    }
  }
};
```
