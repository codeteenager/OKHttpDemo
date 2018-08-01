## OKHttp同步方法
同步请求，发送请求后，就会进入阻塞状态，直到收到响应。

+ 创建OKHttpClient和Request对象
+ 将Request封装成Call对象
+ 调用Call的execute()发送同步请求