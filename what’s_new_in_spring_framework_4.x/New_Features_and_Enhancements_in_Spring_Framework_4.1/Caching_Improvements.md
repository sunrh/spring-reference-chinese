# 4.2.缓存改进

Spring4.1支持[JCache(JSR-107)注解](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#cache-jsr-107)，而你并不需要做任何的更改。

Spring4.1也改进了她的缓存抽象：
* 缓存可以通过```CacheResovler```在运行时解析。也不再需要强制使用```value```作为缓存名称。
* 更多操作级的定制：cache resolver, cache manager, key generator。
* [```@CacheConfig```这一类级别注解]()让我们可以在类里面共享配置。
* ```CacheErrorHandler```让我们更好的处理缓存方法的异常。

Spring4.1还在```CacheInterface```中添加了```putIfAbsent```方法。