# 4.3.Web改进

* 现有的ResourceHttpRequestHandler已经在`ResourceResolver`，`ResourceTransformer`和`ResourceUrlProvider`中扩展。详细内容查看[17.16.7.资源服务]()。
* JDK 1.8 的java.util.Optional支持Controller中的@RequestParam，@RequestHeader和@MatrixVariable方法参数。
* `ListenableFuture`已经在所在的服务(或者调用`AsyncRestTemplate`)中作为返回值而不再是`DeferredResult`。
* `@ModelAttribute`现在允许互相调用了。见[SPR-6299](https://jira.spring.io/browse/SPR-6299)。
* Jackson的@JsonView用于`@ResponseBody`和`ResponseEntity`序列化不同内容相同的 POJO（如摘要与明细）。
* Jackson 已支持 JSONP 。
* 有三个新的`HttpMessageConverter`选择了：
    * GSON
    * Google Protocol Buffers
    * 如果`@EnableWebMvc`或`<mvc:annotation-driven/>`那不再是JAXB2来解析XML，而是`jackson-dataformat-xml`的XML序列化来操作，当然，需要引入相关依赖包。
* `@RequestMapping`如果不提供值的话，Spring会默认生成一个值，例如：FooController中的方法handleFoo，将会默认变成"FC#handleFoo"，这个命名策略也是可以修改的，详细见"[17.7.2 在类或方法上创建视图链接]()"。
* `RequestEntity` 提供了一个 builder 风格的 API 来封装从客户端发送到服务器的Http Rest风格的请求。