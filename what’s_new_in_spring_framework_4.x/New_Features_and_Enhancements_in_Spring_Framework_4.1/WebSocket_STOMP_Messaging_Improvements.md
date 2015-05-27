# 4.4.WebSocket STOMP 消息改进

* 支持SockJS(Java)客户端。见`SockJSClient`同一路径下的其他类。
* STOMP客户端订阅或者取消订阅时，会触发`SessionSubscribeEvent `，`SessionUnsubscribeEvent `事件。
* websocket的生命周期,参见[21.4.13 Websocket生命周期]()
* `@SendToUser`现在只需要通过Session即可，不再需要认证用户。
* `@MessageMapping`可以使用点'.'，而不再需要'/'作为分隔符，参见[SPR-11660](https://jira.spring.io/browse/SPR-11660)。
* STOMP/WebSocket 监听消息的收集和记录。参见[21.4.15, 运行时监控(Runtime Mornitoring)]()。
* 即使是Debug级别的日志也变得更简洁易读。
* STOMP/WebSocket建立连接60s后，如果没有任何活动的话，将会被关闭，见[SPR-11884](https://jira.spring.io/browse/SPR-11884);