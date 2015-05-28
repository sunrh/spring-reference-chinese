# 5.3.Bean介绍

Spring IoC容器将管理一个或多个bean，这些bean 将通过配置文件中的bean定义被创建(在XML格式中为`<bean/>`元素)。

在容器内部，这些bean定义由BeanDefinition对象来表示，该定义将包含以下信息：
* 全限定类名：这通常就是已定义bean的实际实现类。
* bean行为的定义，这些定义将决定bean在容器中的行为（作用域、生命周期回调等等）
* 对其他bean的引用，这些引用bean也可以称之为协作bean（collaborators） 或依赖bean（dependencies）.
* 创建bean实例时的其他配置设置。比如使用bean来定义连接池，可以通过属性或者构 造参数指定连接数，以及连接池大小限制等。

上述内容直接被翻译为每个bean定义包含的一组properties。下面的表格列出了部分 内容的详细链接：

表 5.1. bean定义

|名称|链接|
| -- | -- |
|class|[5.3.2, “实例化bean”]()|
|name|[5.3.1, “bean的命名”]()|
|scope|[5.5, “Bean的作用域”]()|
|constructor arguments|[5.4.1, “依赖注入”]()|
|properties|[5.4.1, “依赖注入”]()|
|autowiring mode|[5.4.5, “自动装配（autowire）协作者”]()|
|dependency checking mode|[5.4.5, “依赖检查”]()|
|lazy-initialization mode|[5.4.4, “延迟初始化bean”]()|
|initialization method|[初始化回调]()|
|destruction method|[析构回调]()|

除了通过bean定义来描述要创建的指定bean的属性之外，某些BeanFactory的实现也允许将那些非BeanFactory创建的、已有的用户对象注册到容器中，比如使用DefaultListableBeanFactory 的registerSingleton(..)方法。不过大多数应用还是采用 元数据定义为主。