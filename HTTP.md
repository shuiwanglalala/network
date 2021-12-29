# HTTP

## HTTP是无连接的

[怎么理解TCP是面向连接的，HTTP基于TCP却是无连接的？](https://www.zhihu.com/question/51996213/answer/128801185)

https://segmentfault.com/a/1190000015821798

**因为短连接、长连接的实现在HTTP之外，与HTTP无关，从HTTP自身来看，HTTP依然是无连接的**

### HTTP/0.9时代：短连接

每个HTTP请求都要经历一次DNS解析、三次握手、传输和四次挥手。反复创建和断开TCP连接的开销巨大

### HTTP/1.0时代：持久连接概念提出

人们认识到短连接的弊端，提出了持久连接的概念，在HTTP/1.0中得到了初步的支持

持久连接，即一个TCP连接服务多次请求

### HTTP/1.1时代：持久连接成为默认的连接方式；提出pipelining概念

HTTP/1.1开始，即使请求header中没有携带Connection: Keep-Alive，传输也会默认以持久连接的方式进行

同时，持久连接的弊端被提出 —— HOLB（Head of Line Blocking）即持久连接下一个连接中的请求仍然是串行的，如果某个请求出现网络阻塞等问题，会导致同一条连接上的后续请求被阻塞。所以HTTP/1.1中提出了pipelining概念，即客户端可以在一个请求发送完成后不等待响应便直接发起第二个请求，服务端在返回响应时会按请求到达的顺序依次返回，这样就极大地降低了延迟

然而pipelining并没有彻底解决HOLB，为了让同一个连接中的多个响应能够和多个请求匹配上，响应仍然是按请求的顺序串行返回的。所以pipelining并没有被广泛接受，几乎所有代理服务都不支持pipelining，部分浏览器不支持pipelining，支持的大部分也会将其默认关闭

### SPDY和HTTP/2：multiplexing

multiplexing即多路复用，在SPDY中提出，同时也在HTTP/2中实现。

[multiplexing技术](https://www.zhihu.com/search?q=multiplexing技术&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A128801185})能够让多个请求和响应的传输完全混杂在一起进行，通过streamId来互相区别。这彻底解决了holb问题，同时还允许给每个请求设置优先级，服务端会先响应优先级高的请求

## Session/Cookie/Token

[都说HTTP协议是无状态的，这里的「状态」到底指什么？](https://mp.weixin.qq.com/s/EZwOUGMrGKEF_POisJKmuw)

值得反复思考

[Session/Cookie/Token 还傻傻分不清？](Session/Cookie/Token 还傻傻分不清？)

[彻底理解cookie，session，token](https://zhuanlan.zhihu.com/p/63061864)

## HTTP历史

[图解 HTTP 的前世今生！](https://mp.weixin.qq.com/s/vk_QZDpJLVEXXNe1jYzd8w)

## HTTP方法

[post 相比get 有很多优点，为什么现在的HTTP通信中大多数请求还是使用get？](https://www.zhihu.com/question/31640769/answer/52824098)

所以如果你希望

－ 请求中的URL可以被手动输入

－ 请求中的URL可以被存在书签里，或者历史里，或者快速拨号里面，或者分享给别人。

－ 请求中的URL是可以被搜索引擎收录的。

－ 带云压缩的浏览器，比如Opera mini/Turbo 2, 只有GET才能在服务器端被预取的。

－ 请求中的URL可以被缓存。

请使用GET.

反之，就用POST. 特别是有一些东西你是不想让人家可以在浏览器地址栏里面可以输入的。比如，如果你设计一个blog系统, 设计这样一个URL来删掉所有帖子

另外一个准则是，可以重复的交互，比如取个数据，跳个页面， 用GET

不可以重复的操作， 比如创建一个条目/修改一条记录， 用POST, 因为POST不能被缓存，所以浏览器不会多次提交

> 1.3 Quick Checklist for Choosing HTTP GET or POST
>
> - Use GET if:
>   - The interaction is more like a question (i.e., it is a safe operation such as a query, read operation, or lookup).

and

> - Use POST if:
>   - The interaction is more like an order, or
>   - The interaction changes the state of the resource in a way that the user would perceive (e.g., a subscription to a service), or o The user be held accountable for the results of the interaction.
>
> However, before the final decision to use HTTP GET or POST, please also consider considerations for sensitive data and practical considerations.

先说明下安全和幂等的概念：

- 在 HTTP 协议里，所谓的「安全」是指请求方法不会「破坏」服务器上的资源。
- 所谓的「幂等」，意思是多次执行相同的操作，结果都是「相同」的。

那么很明显 **GET 方法就是安全且幂等的**，因为它是「只读」操作，无论操作多少次，服务器上的数据都是安全的，且每次的结果都是相同的。

**POST** 因为是「新增或提交数据」的操作，会修改服务器上的资源，所以是**不安全**的，且多次提交数据就会创建多个资源，所以**不是幂等**的。

## 面试

[硬核！30 张图解 HTTP 常见的面试题](https://mp.weixin.qq.com/s/FJGKObVnU61ve_ioejLrtw)