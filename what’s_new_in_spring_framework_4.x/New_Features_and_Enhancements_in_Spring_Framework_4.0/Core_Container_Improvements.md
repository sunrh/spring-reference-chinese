# 3.6.内核改进

对于内核的改进主要体现在以下几个方面：
* Spring现在可以在注入中使用泛型类型([generic types as a form of qualifier]())。例如你想使用Spring Data的Repository，你仅仅需要做如下实现：@Autowired Repository<Customer> customerRepository(以前的实现比较麻烦要set一下)。
* 如果你开启了Spring的元注解(meta-annotation)支持，你还可以[基于源注解创造新的注解]()(expose specific attributes from the source annotation)。
* 现在，当你[注入List或者Array]()的时候，这些Bean可以通过声明@Order或者实现Ordered接口实现排序。
* 在注入Bean的时候，我们可以使用@Lazy实现延迟注入，同样适用于@Bean。
* 当你使用Java的配置方式(非XML)，你可以使用@Description来告诉其他开发人员一些关于该类的介绍。
* @Conditional类似于@Profile用于[有条件的过滤Beans]()，但比Profile好的是可以定义策略来切换不同环境。
* [基于CGLIB的代理类]()不在需要默认的构造方法。这个支持是由 [objenesis](http://code.google.com/p/objenesis/)库提供。这个库重新打包到 Spring 框架中，作为Spring框架的一部分发布。通过这个策略，针对代理实例被调用没有构造可言了。
