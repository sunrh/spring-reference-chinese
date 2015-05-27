# 5.2.2.实例化容器

实例化容器实际上非常简单，如下：
```
ApplicationContext context =
    new ClassPathXmlApplicationContext(new String[] {"services.xml", "daos.xml"});
```
下面的例子展示了服务层的配置文件(services.xml)：
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- services -->

    <bean id="petStore" class="org.springframework.samples.jpetstore.services.PetStoreServiceImpl">
        <property name="accountDao" ref="accountDao"/>
        <property name="itemDao" ref="itemDao"/>
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for services go here -->

</beans>
```
下面的例子展示了数据连接层的配置文件(daos.xml)：
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="accountDao"
        class="org.springframework.samples.jpetstore.dao.jpa.JpaAccountDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <bean id="itemDao" class="org.springframework.samples.jpetstore.dao.jpa.JpaItemDao">
        <!-- additional collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions for data access objects go here -->

</beans>
```
在前面的例子中，服务层的`PetStoreServiceImpl`注入了`JpaAccountDao`,`JpaItemDao`。其中`property name`指向JavaBean的成员属性，`ref`链接到`JpaAccountDao`,`JpaItemDao`中指定的`id`。

## XML配置元数据的结构
将XML配置文件分拆成多个部分是非常有用的。通常每个单独的XML文件表示一个逻辑层或者模块。

为了加载多个XML文件生成一个 ApplicationContext实例， 可以将文件路径作为字符串数组传给ApplicationContext构造器 。而实际上我们可以通过一个或者多个的`<import/>`元素来从另外一个或多个文件加载bean定义。所有的`<import/>`元素必 须在`<bean/>`元素之前完成bean定义的导入。让我们看个例子：
```
<beans>
    <import resource="services.xml"/>
    <import resource="resources/messageSource.xml"/>
    <import resource="/resources/themeSource.xml"/>

    <bean id="bean1" class="..."/>
    <bean id="bean2" class="..."/>
</beans>
```
在上面的例子中，我们从3个外部文件：services.xml、 messageSource.xml及themeSource.xml 来加载bean定义。这里采用的都是相对路径，因此，此例中的services.xml 一定要与导入文件放在同一目录或类路径，而messageSource.xm l和themeSource.xml的文件位置必须放在导入文件所 在目录下的resources目录中。正如你所看到的那样，开头的斜杠 ‘/’实际上可忽略。因此不用斜杠‘/’可能会更好一点。根据Spring XML配置文件的 Schema(或DTD)，被导入文件必须是完全有效的XML bean定义文件，且根节点必须为`<beans/>`元素。