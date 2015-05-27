# 3.7.Web强化

Spring框架现在主要运行与Servlet 3.0+的环境中，当然Servlet2.5的运行环境还是可以的(不建议)，如果你要用[Spring MVC测试框架]()，那你需要添加Servlet3.0的包在你的测试classpath中(Mock的对象都是基于Servlet3)。

除了我们待会提到的WebSocket之外，下面的这些改进已经加入了Spring豪华午餐中：
* [新的@RestController注解]()取代了以前在每个@RequestMapping下的@ResponseBody，更加方便你的Rest开发。
* AsyncRestTemplate支持Rest客户端的[异步无阻塞]()支持。
* Spring提供了[全面的时区支持]()(为什么还要再重复一遍....)。