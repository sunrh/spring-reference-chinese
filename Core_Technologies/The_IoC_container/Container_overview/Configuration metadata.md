# 5.2.1.配置元数据

在前面的图中，我们知道IOC容器将读取配置元数据(configuration metadata)；并且从配置中读取应用程序各个对象的实例化，配置以及组装。

通常我们使用简单直观的XML来配置。现在越来越多的人使用Java的方式(Java-based)或注解的方式(Annotation-based)来配置。

XML配置方式使用`<beans/>`作为顶层element，`<bean/>`作为实际配置Bean的项。Java配置方式的话需要在类上注释`@Configuration`，同时方法上注释`@Bean`。

