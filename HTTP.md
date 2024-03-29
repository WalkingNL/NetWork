## HTTP的创造者

**如果有空了，就去源头看看**。今天看了左耳多耗子十月份在他的个人网站上更新的文章，[HTTP的前世今生](https://coolshell.cn/articles/19840.html)，延续了他一贯的风格，文章依然很棒。但关于`pipleline网络传输`这个点，我不太理解。所以就到处去查，最后发现了[这个](https://www.w3.org/Protocols/rfc2616/rfc2616-sec8.html#sec8)，确切的说**才**发现了这个。一直以来，养成了一个特别不好的毛病就是，太依赖别人的分享了，觉得这样快，觉得原始信息被别人剪枝后，更容易看清本质。而且总有一个真实的错觉————读完别人的分享之后，有意无意的觉得这些都是自己的了。而事实上呢？


1. URL解析
2. DNS查询
3. TCP连接
4. 处理请求
5. 接受请求
6. 页面渲染


HTTP1.0在HTTP0.9的基础上进行了很大的扩充，但带来了性能相关的问题，HTTP/1.1主要解决了HTTP/1.0的网络性能问题，以及一些新的东西。

  * HTTP/1.1 增加了Keep-alive机制，避免了TCP连接时三次握手的巨大开销。 
  * 支持pipeline网络传输，只要一个请求发出去，不必等其回来，就可以发第二个请求，这样做减少了整体的响应。但客户端不应当**pipeline**那些使用`非幂等(non-idempotent)`或者`非幂等(non-idempotent)`序列化方法的请求，否则，传输链接的过早终止可能导致不确定的结果。对于一个想要发送`非幂等(non-idempotent)`请求的客户端，应当等到它接收到前一个请求的响应状态之后，再发送。
  * 支持Chunked Response
  * 增加了Cache Control机制。当客户端发送的请求中，包含`max-age`指令时，如果判定缓存层中，资源的缓存时间数值比指定时间的数值小，客户端可以接受缓存的资源；而当指定的`max-age`值为0时，缓存层通常需要将请求转发给应用集群。与缓存相关的还有另一个关键字，`If-Modified-Since`，当服务器的资源在某个时间之后更新了，那么客户端就应该下载最新的资源；如果没有更新，服务端返回“304 Not Modified”的响应，预示着客户端就不用下载了。
  * 协议头中增加了Language，Encoding,Type等头信息，服务端与客户端可以进行更多的协商
  * 加入了头`HOST`。服务器就知道要请求哪个网站了。因为存在多个域名解析到同一个IP上，要区分用户请求的是哪个域名，就需要在HTTP的协议中加入域名的信息
  * 加入了`OPTIONS`方法，主要用于`CORS(Cross Origin Resource Sharing)`应用。

HTTP/1.1可以重用TCP连接，但依然存在许多的性能问题。
1. 请求时串行发送的，需要保证顺序性；
2. 传输数据时，是以文本的方式，需借助zip压缩的方式减少网络带宽，但是zip压缩比较耗CPU资源

它的请求发送中，不支持并行发送，而大量的网页请求中，存在资源类这样的东西，占据了HTTP请求中大量的传输数据，为了提高网络吞吐和性能，
  
---
HTTP HEADER
|field|description|
|--|--|
|X-Requested-With|indicate that the http request is made by XmlHttpRequest instead of being triggered by clicking a regular hyperlink or form submit button, by doing this for the purpose of security to prevent CSRF attacks|
|Referer|contains the address of the web page from which a resource has been requested|
