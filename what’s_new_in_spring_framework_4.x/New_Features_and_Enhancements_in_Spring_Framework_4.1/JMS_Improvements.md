# 4.1.JMS改进

如果你要[注册JMS监听端口(endpoints)]()，可以使用@JMSListener轻松做到。配置方面只需要声明```jms:annotation-driven```(XML)，或者配置@EnableJms，JmsListenerContainerFactory(Java)。其实你完全可以使用JmsListenerConfigurer来注册。

Spring 4.1对JMS的改动，让你可以有益于使用```spring-messaging```，即：

* 你可以使用@Payload、@Header、@Headers和@SendTo这类便捷的注解去声明监听端口。另外，也能使用标准的Message作为方法参数代替javax.jms.Message。
* ```JmsMessageOperations```让```JmsTemplate```很方便操作```Message```。

最后，Spring 4.1还做了其他各种各样的改进：

* ```JmsTemplate```提供了同步的请求-答复。
* 监听器可以在<jms:listener/>指定优先级。
* 消息监听器容器的恢复可以通过BackOff实现。
* JMS 2.0支持共享消费者。