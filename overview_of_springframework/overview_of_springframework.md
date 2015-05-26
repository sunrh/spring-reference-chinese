# 第一章：Spring框架概述

Spring框架是一个轻量级的，并且提供一站式构建企业应用的解决方案。同时，她也是模块化的，因为你可以只使用框架当中的某些你需要的部分而没必要全部引用。例如你可以使用IOC容器，同时使用其他技术作为表现层，使用[Hibernate框架]()或者[JDBC]()作为数据层。Spring框架支持声明式事务管理，通过RMI或者Web Services连接你的远程服务，并支持多种选择持久化你的数据。她也提供了全功能的[MVC框架]()，同时使用[AOP]()无侵入的将逻辑植入到你的产品中。

Spring的设计之初就是以非侵入作为目的，意思是你的逻辑代码与框架本身并没有依赖关系。在你的集成层(如数据连接层)中，一些数据连接所需要的依赖包，在Spring中已经存在了，这样做的好处就是你的代码将从这些依赖中独立出来。

通过这文档将让你了解Spring Framework的特性。如果有任何要求，意见或者问题，请发送到[用户邮件列表](https://groups.google.com/forum/#!forum/spring-framework-contrib)。框架本身的问题请到StackOverflow中提问(见[https://spring.io/questions](https://spring.io/questions))