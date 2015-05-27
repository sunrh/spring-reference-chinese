# 5.1.IOC以及Beans的介绍

本章覆盖了控制反转的原理。依赖注入专门处理那些类之间的依赖关系，她通过构造参数，工厂方法中的参数，或者成员属性来注入的。这些注入的工作是在他们被构造或者从工厂方法返回的时候完成的。让容器来做注入的工作从而反转了由类本身实例化并且自己完成依赖的行为。

`org.springframework.beans`与`org.springframework.context`构成了Spring IOC容器的基石。`BeanFactory`接口让你有能力管理各种类型的对象。`ApplicationContext`是`BeanFactory`的孙接口，她很容易就能集成进Spring AOP特性，消息处理，事件发布，应用层的上下文(如，`WebApplicationContext`)。

简而言之，`BeanFactory`提供了框架最基本的功能，`ApplicationContext`添加了一些企业特性的功能。`ApplicationContext`是本章节中专门用来描述IOC的类，如果你想知道更多关于`BeanFactory`的资料，参见[5.16.BeanFactory]()。

在Spring中，我们把那些被Spring IOC容器管理的关键对象(组成你应用程序的主体)称之为beans.一个Bean实际上是指一个被IOC容器管理，实例，装配的对象(object)。而bean定义以及bean相互间的依赖关系将通过配置元数据来描述。