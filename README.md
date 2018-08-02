## Okhttp简介
OkHttp是Android和Java应用的HTTP和HTTP/2的客户端，是由Square公司维护，是Android最火热的网络开源项目，用于替代HttpUrlConnection和Apache HttpClient(android API23 里已移除HttpClient)，Volley也是一个网络请求库，由于内部使用HttpClient，所以不建议使用。OkHttp使用了大量的Builder模式

OkHttp有一些高效的属性如下：
    + 在HTTP/2支持下，如果网络请求的host是同一个时，允许这些请求公用同一个socket
    + 通过使用连接池减少网络延迟(如果HTTP/2不可用)
    + 透明的GZIP压缩，减少数据流量
    + 缓存网络响应，避免重复网络请求
    
## 添加依赖

```java
dependencies {
    implementation 'com.squareup.okhttp3:okhttp:3.10.0'
}
```

## OkHttp同步请求方法
同步请求，发送请求后，就会进入阻塞状态，直到收到响应。

+ 创建OkHttpClient和Request对象
+ 将Request封装成Call对象
+ 调用Call的execute()发送同步请求

```java
OkHttpClient okHttpClient = new OkHttpClient.Builder().readTimeout(1000, TimeUnit.SECONDS).build();
Request request = new Request.Builder().url("http://www.baidu.com").get().build();
Call call = okHttpClient.newCall(request);
try {
    Response response = call.execute();
} catch (IOException e) {
    e.printStackTrace();
}
```

![](https://ws4.sinaimg.cn/large/006tKfTcgy1ftudjyvv6oj31700p6471.jpg)


## OkHttp异步请求方法
异步请求，发送请求后，不会阻塞当前线程，而是开启子线程执行。

+ 创建OkHttpClient和Request对象
+ 将Request封装成Call对象
+ 调用Call的enqueue方法进行异步请求

```java
OkHttpClient okHttpClient = new OkHttpClient.Builder().readTimeout(1000, TimeUnit.SECONDS).build();
final Request request = new Request.Builder().url("https://www.imooc.com/").get().build();
Call call = okHttpClient.newCall(request);
call.enqueue(new Callback() {
    @Override
    public void onFailure(Call call, IOException e) {

    }

    @Override
    public void onResponse(Call call, Response response) throws IOException {
        
    }
});
```

其中onFailure和onResponse是在子线程中执行。