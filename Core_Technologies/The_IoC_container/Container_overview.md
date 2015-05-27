# 5.2.容器

`org.springframework.context.ApplicationContext`对Bean的管理是通过读取配置元数据实现的。这种元数据可以表现为XML，Java注解或者Java代码。她描述了应用程序中复杂的依赖关系。

Spring提供了一下开箱即用的`ApplicationContext`，如`ClasspathXmlApplicationContext`和`FileSystemXmlApplicationContext`，我们可以用少量的XML配置元数据，其余的在这基础上使用注解或者代码来完成配置元数据。

在大多数的应用程序中，并不需要用显式的代码去实例化一个或多个的Spring IoC容器实例。例如，在web应用程序中，我们只需要在web.xml中添加 (大约)8 行简单的XML描述符即可(见[5.15.4.ApplicationContext在WEB应用中的实例化]())，如果你使用[Spring Tool Suite](https://spring.io/tools/sts)，那配置仅仅需要按几下鼠标或者敲几下键盘即可。

下面的插图解释了Spring是如何工作的：你的应用程序中的类全部结合在元数据配置中，当`ApplicationContext`被创建以及实例化后，你就拥有了一个完全配置好的、可执行的应用。

图5.1. Spring IOC容器

![](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/images/container-magic.png)
