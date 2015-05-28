# 5.3.1.Bean别名

每一个Bean都可以注册一个或多个标识符，但这些标识符必须在容器管理的范围内保持唯一。通常我们一个Bean只会注册一个标识符，其他的我们用别名来表示。

在XML配置文件中，你可以使用`id`或者`name`属性来区分Bean。`id`属性中你有且只能有一个id，以字母数字的方式组成(如：myBean, fooService等)，但如果你想要起多个别名，你可以在`name`属性中使用逗号(,)，分号(;)或者空格来间隔开。

```
bean命名约定
bean的命名采用标准的Java命名约定，即小写字母开头，首字母大写间隔 的命名方式。如accountManager、accountService、userDao及loginController，等等。
对bean采用统一的命名约定将会使配置更加简单易懂。而且在使用Spring AOP时 ，如果要发通知(advice)给与一组名称相关的bean时，这种简单的命名方式将会令你受益匪浅。
```

## bean的别名
在对bean进行定义时，除了使用id属性来指定名称之外，为了提供多个名称，需要通过name属性来加以指定。而所有的这些名称都指向同一个bean，在某些情况下提供别名非常有用，比如为了让应用的每一个组件能更容易的对公共组件进行引用。

然而，在定义bean时就指定所有的别名并不是总是恰当的。有时我们期望 能在当前位置为那些在别处定义的bean引入别名。在XML配置文件中，可用 `<alias/>` 元素来完成bean别名的定义。如：
```
<alias name="fromName" alias="toName"/>
```
考虑一个更为具体的例子，子系统A在XML配置文件中定义了一个名为`subsystemA-dataSource`的DataSource bean。但子系统B却想在其XML文件中以`subsystemB-dataSource`的名字来引用此bean。而且在主程序MyApp的XML配置文件中，希望以myApp-dataSource的名字来引用此bean。最后容器加载三个XML文件来生成最终的ApplicationContext，在此情形下，可通过在MyApp XML文件中添加下列alias元素来实现：
```
<alias name="subsystemA-dataSource" alias="subsystemB-dataSource"/>
<alias name="subsystemA-dataSource" alias="myApp-dataSource" />
```
这样一来，每个组件及主程序就可通过唯一名字来引用同一个数据源而互不干扰。