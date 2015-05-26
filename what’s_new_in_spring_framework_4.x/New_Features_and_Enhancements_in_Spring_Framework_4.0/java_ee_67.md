# Java EE 6/7

Java EE 6 或以上的版本是Spring4的基线, JPA2.0和Servlet3.0规范对Spring4有着特殊的意义。为了保持对Google App Engine以及旧版本服务器的兼容,Spring4也可以部署在 Servlet2.5的环境。但是我们强烈的建议您使用Servlet3.0+。

如果你是WebSphere 7的用户，请确认已经安装JPA2.0包。在WebLogic 10.3.4或更高版本，需要安装JPA2.0补丁。这样就能支持Spring4了。

长远来看，JavaEE 7规范已加入Spring4的豪华午餐(支持)：尤其是 JMS 2.0, JTA 1.2, JPA 2.1, Bean Validation 1.1和JSR-236 Concurrency Utilities。跟过去一样，我们专注在独立的使用这些规范上，如在 Tomcat 或者独立环境中。但是，当把 Spring 应用部署到 Java EE 7 服务器时它同样适用。

注意，Hibernate 4.3 是 JPA 2.1 的提供者，因此它只支持 Spring4。如同Bean Validation 1.1 提供者的 Hibernate Validator 5.0。这两个都不支持 Spring3.2。