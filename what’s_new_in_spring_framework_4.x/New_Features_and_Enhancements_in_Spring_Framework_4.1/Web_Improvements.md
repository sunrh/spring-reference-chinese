# 4.3.Web改进

* 现有的ResourceHttpRequestHandler已经在`ResourceResolver`，`ResourceTransformer`和`ResourceUrlProvider`中扩展。详细内容查看[17.16.7.资源服务]()。
* JDK 1.8 的java.util.Optional支持Controller中的@RequestParam，@RequestHeader和@MatrixVariable方法参数。
* `ListenableFuture`已经在所在的服务(或者调用`AsyncRestTemplate`)中作为返回值而不再是`DeferredResult`。
* `@ModelAttribute`现在允许互相调用了。见[SPR-6299](https://jira.spring.io/browse/SPR-6299)。
* Jackson的@JsonView被直接支撑在@ResponseBody和ResponseEntity控制器方法用于序列化不同的细节对于相同的 POJO（如摘要与细节页）。同时通过添加序列化视图类型作为模型属性的特殊键来支持基于视图的渲染。见Jackson Serialization View Support(Jackson序列化视图支持)
* Jackson 现在支持 JSONP ，见 Jackson JSONP Support
一个新的生命周期选项可用于在控制方法返回后，响应被写入之前拦截@ResponseBody和ResponseEntity方法。要充分利用声明@ControllerAdvice bean 实现ResponseBodyAdvice。为@JsonView和 JSONP 的内置支持利用这一优势。参见第17.4.1，“使用HandlerInterceptor 拦截请求”。
有三个新的HttpMessageConverter选项：
GSON - 比 Jackson 更轻量级的封装;已经被使用在 Spring Android
Google Protocol Buffers - 高效和有效的企业内部跨业务的数据通信协议，但也可以用于浏览器的 JSON 和 XML 的扩展
Jackson 基于 XML 序列化，现在通过 jackson-dataformat-xml 扩展得到了支持。如果 jackson-dataformat-xml 在 classpath， 默认情况下使用@EnableWebMvc或<mvc:annotation-driven/>，这是，而不是JAXB2。
如 JSP 等视图现在可以通过名称参照控制器映射建立链接控制器。默认名称分配给每一个@RequestMapping。例如FooController的方法与handleFoo被命名为“FC＃handleFoo”。命名策略是可插拔的。另外，也可以通过其名称属性明确命名的@RequestMapping。在Spring JSP标签库的新mvcUrl功能使这个简单的JSP页面中使用。参见第17.7.2，“Building URIs to Controllers and methods from views”
ResponseEntity提供了一种 builder 风格的 API 来指导控制器向服务器端的响应的展示，例如，ResponseEntity.ok()。
RequestEntity 是一种新型的，提供了一个 builder 风格的 API 来引导客户端的 REST 响应 HTTP 请求的展示。
MVC 的 Java 配置和 XML 命名空间：
视图解析器现在可以配置包括内容协商的支持，请参见17.16.6“视图解析”。
视图控制器现在已经内置支持重定向和设置响应状态。应用程序可以使用它来配置重定向的URL，404 视图的响应，发送“no content”的响应，等等。有些用例这里列出。
现在内置路径匹配的自定义。参见第17.16.9，“路径匹配”。
Groovy 的标记模板支持（基于Groovy的2.3）。见GroovyMarkupConfigurer 和各自的ViewResolver和“视图”的实现。