# 3.8.WebSocket, SockJS, 以及STOMP消息协议

在客户端以及服务器之间的基于Websocket以及全双工的通讯已经通过spring-websocket实现了。它兼容[JSR-356(Java Websocket API)](http://jcp.org/en/jsr/detail?id=356),此外还提供了基于SockJS(Websocket模拟器)给那些不支持WebSocket协议的浏览器(例如某IE，<10).

spring-messaging支持STOMP(WebSocket 子协议)。

更详尽的介绍，请参见[21.WebSocket]()。