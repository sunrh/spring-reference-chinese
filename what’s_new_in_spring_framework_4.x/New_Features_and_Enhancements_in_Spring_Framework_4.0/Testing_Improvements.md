# 3.9.测试改进

除了删除一些在spring-test中过时的代码外，Spring 4 为单元测试和集成测试增加了新的功能。

* 几乎spring-test的所有注解(如：@ContextConfiguration、@WebAppConfiguration、@ContextHierarchy、@ActiveProfiles等)都可以作为元注解来创建自定义的复合注解(composed annotations)并可以减少测试中的配置。
* 现在通过@ActiveProfiles就可以激活@Profile的那些类。
* SocketUtils加到了spring-core中。这个类可以扫描本地主机的空闲的TCP和UDP端口。
* 现在org.springframework.mock.web包中的mocks是基于Servlet 3.0 API的。此外，一些Servlet API mocks(如：MockHttpServletRequest，MockServletContext等等)已经有一些小的改进更新并提高了可配置性。