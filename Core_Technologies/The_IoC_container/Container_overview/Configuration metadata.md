# 5.2.1.配置元数据

在前面的图中，我们知道IOC容器将读取配置元数据(configuration metadata)；并且从配置中读取应用程序各个对象的实例化，配置以及组装。

通常我们使用简单直观的XML来配置。现在越来越多的人使用Java的方式(Java-based)或注解的方式(Annotation-based)来配置。

XML配置方式在顶层的`<beans/>`元素配置一个或多个`<bean/>`元素。Java配置方式的话需要在类上注释`@Configuration`，同时方法上注释`@Bean`。

bean定义与应用程序中实际使用的对象一一对应。通常情况下bean的定义包括：服务层对象、数据访问层对象（DAO）、类似Struts Action的 表示层对象、Hibernate SessionFactory对象、JMS Queue对象等等。通常bean的定义并不与容器中的领域 对象相同，因为领域对象的创建和加载必须依赖具体的DAO和业务逻辑。此外你可以通过AspectJ去配置那些不在IOC容器内创建的对象。

以下是一个基于XML的配置元数据的基本结构：
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```
id属性是一个用来唯一确认Bean的字符串。class属性需要填写类的完整的名称(路径.类)
