## Cookie and Session

### cookie
HTTP是无状态的请求协议，不会记住用户的状态和信息，也不清楚你之前访问过什么。

因此需要网站服务器记录用户登陆时，需要在用户登录时创建一些信息，并把这些信息记录在当前用户的浏览器中，记录的内容就是cookie。用户下一次使用这个浏览器继续访问这个服务器时，发送的请求会携带cookie信息。

cookie是记录在浏览器中的，会存在以下问题：
1. 浏览器更换或者cookie被删除后，信息丢失；
2. 记录在浏览器端的信息是不安全的，不能记录敏感信息。

### session
session是在服务器端进行数据的记录，并且给每一个用户一个sessionID，并且把这个sessionID记录在浏览器端，也就是设置为cookie。

!["cookie.png"](https://raw.githubusercontent.com/Joey-Hu/markdown-noteook/master/spider/images/cookie_and_session/cookie.png)



参考：
