## 简介
**超文本传输协议**是用于传输超媒体资源文件的**应用层**协议，它是为Web浏览器与Web服务器之间交互设计的协议。

## 协议基本构成
以下是一段简单的HTTP请求内容，第一行作为固定格式，包含请求方法“GET”，资源路径“/”以及协议版本号“HTTP/1.1” 

```http request
GET / HTTP/1.1
User-Agent: Mozilla/5.0
```

## 常见Head头部
请求头(Request header):  
- Accept 告知服务端客户端可以处理的内容类型，值用MIME类型表示。
- Connection
- Content-Type
- Cookie
- Host
- Origin
- Referer
- User-Agent

响应头(Response header):  
- Access-Control-*
- Age
- Cache-Control
- Connection
- Content-Encoding
- Content-Length 
- Content-Type
- Date
- Location

## HTTP缓存
缓存可分为**私有缓存**和**公共缓存**，私有缓存只能用于单独的用户，公共缓存能够被多个用户使用。
  
**Cache-Control响应头**  
可选值：  
- **no-store** 没有缓存，每次请求都将从服务器获取完整内容。
- **no-cache** 缓存但重新验证，客户端应提交**验证字段**到服务端，若服务端返回**304**则说明缓存可用
- **public** 公共缓存，内容可以被任何中间代理、CDN站点缓存
- **private** 私有缓存，仅浏览器能够缓存
- **max-age=xxx** 缓存最大有效期（单位秒），该选项优先级高于**Expires**头，在有效期类浏览器将不会请求服务器获取内容
- **must-revalidate** 强制验证缓存状态，已过期缓存将不被使用

**ETag响应头**  
作为**强校验器**，其值对用户代理不透明。如果响应中包含ETag字段，那么字段的值可以被后续用作缓存校验，在后续请求中添加**If-None-Match**头，并将值设为ETag返回的值，若服务返回304则说明缓存有效。
  
**If-None-Match请求头**  
配合ETag用作缓存校验


**Last-Modified响应头**  
值是一个日期格式化后的字符串，由于只能精确到秒，所以称为**弱校验器**。

**If-Modified-Since请求头**  
配合Last-Modified用作缓存校验


## HTTP cookie
Cookie是服务器发送给浏览器并存储在浏览器端的一小段数据，它会在下次向同一服务器请求时携带在请求头中发送给服务器。  
主要被用于这几个方面：  
- 会话状态管理（如用户登陆状态保持，购物车
- 个性化设置 （如自定义设置、主题
- 浏览器行为跟踪

**Set-Cookie响应头和Cookie请求头**  
写入浏览器Cookie的两种方式：
- 通过服务端响应头字段写入
- 浏览器执行Javascript写入

服务器返回响应头：
> Set-Cookie: <cookie名>=<cookie值>

JavaScript代码：
> document.cookie="<cookie名>=<cookie值>";

在下一次请求服务时，将在请求头部添加Cookie:
```http request
GET /index.html HTTP/1.1
Host: github.com
Cookie: cookie1=1; name=jack
```


## CORS跨域资源请求
常用访问控制响应头部：  
- **Access-Control-Allow-Credentials**
- **Access-Control-Allow-Headers**
- **Access-Control-Allow-Methods**
- **Access-Control-Allow-Origin**
- **Access-Control-Max-Age**
- **Access-Control-Max-Request-Headers**
- **Access-Control-Max-Request-Method**