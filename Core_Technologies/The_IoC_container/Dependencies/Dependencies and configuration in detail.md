# 5.4.2.依赖配置详解

正如前面章节所提到的，bean的属性及构造器参数既可以引用容器中的其他bean，也可以是内联（inline）bean。在spring的XML配置中使用`<property/>`和`<constructor-arg/>`元素定义。

### 直接变量(基本类型、字符串等)
`<property/>`元素中的`<value/>`元素通过人可以理解的字符串来指定属性或构造器参数的值。Spring的转换器将用于把字符串从java.lang.String类型转化为实际的属性或参数类型。
```
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- results in a setDriverClassName(String) call -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="masterkaoli"/>
</bean>
```

接下来我们采用`p`命名空间来演示上面的代码：
```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="masterkaoli"/>

</beans>
```
上面这种形式似乎更有效率一点；当然我们也可以按照下面这种方式配置一个java.util.Properties实例：
```
<bean id="mappings"
    class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">

    <!-- typed as a java.util.Properties -->
    <property name="properties">
        <value>
            jdbc.driver.className=com.mysql.jdbc.Driver
            jdbc.url=jdbc:mysql://localhost:3306/mydb
        </value>
    </property>
</bean>
```
看到什么了吗？如果采用上面的配置，Spring容器将使用JavaBean PropertyEditor把`<value/>`元素中的文本转换为一个java.util.Properties实例。由于这种做法的简单，因此Spring团队在很多地方也会采用内嵌的`<value/>`元素来代替value属性。

### idref元素
idref元素用来将容器内其它bean的id传给`<constructor-arg/>` 或 `<property/>`元素，同时提供错误验证功能。
```
<bean id="theTargetBean" class="..."/>

<bean id="theClientBean" class="...">
    <property name="targetName">
        <idref bean="theTargetBean" />
    </property>
</bean>
```
上述bean定义片段完全地等同于（在运行时）以下的片段：
```
<bean id="theTargetBean" class="..." />

<bean id="client" class="...">
    <property name="targetName" value="theTargetBean" />
</bean>
```
第一种形式比第二种更可取的主要原因是，使用idref标记允许容器在部署时 验证所被引用的bean是否存在。而第二种方式中，传给client bean的targetName属性值并没有被验证。任何的输入错误仅在client bean实际实例化时才会被发现（可能伴随着致命的错误）。如果client bean 是prototype类型的bean，则此输入错误（及由此导致的异常）可能在容器部署很久以后才会被发现。

```
`idref`中的`local`属性已经从4.0xsd移除，如果你要从低版本升级到4.0的话，请使用`idref`中的`bean`替代。
```
上面的例子中，与在ProxyFactoryBean bean定义中使用`<idref/>`元素指定AOP interceptor的相同之处在于：如果使用`<idref/>`元素指定拦截器名字，可以避免因一时疏忽导致的拦截器ID拼写错误。

### 引用其它的bean（协作者）

在`<constructor-arg/>`或`<property/>`元素内部还可以使用ref元素。该元素用来将bean中指定属性的值设置为对容器中的另外一个bean的引用。如前所述，该引用bean将被作为依赖注入，而且在注入之前会被初始化（如果是singleton bean则已被容器初始化）。尽管都是对另外一个对象的引用，但是通过id/name指向另外一个对象却有三种不同的形式，不同的形式将决定如何处理作用域及验证。

第一种形式也是最常见的形式是通过使用`<ref/>`标记指定bean属性的目标bean，通过该标签可以引用同一容器或父容器内的任何bean（无论是否在同一XML文件中）。XML 'bean'元素的值既可以是指定bean的id值也可以是其name值。
```
<ref bean="someBean"/>
```
第二种形式是使用ref的local属性指定目标bean,这种方式现在更改为`ref bean`而不再是`ref local`（4.0开始）

第三种方式是通过使用ref的parent属性来引用当前容器的父容器中的bean。parent属性值既可以是目标bean的id值，也可以是name属性值。而且目标bean必须在当前容器的父容器中。使用parent属性的主要用途是为了用某个与父容器中的bean同名的代理来包装父容器中的一个bean(例如，子上下文中的一个bean定义覆盖了他的父bean)。

```
<!-- in the parent context -->
<bean id="accountService" class="com.foo.SimpleAccountService">
    <!-- insert dependencies as required as here -->
</bean>
```

```
<!-- in the child (descendant) context -->
<bean id="accountService" <!-- bean name is the same as the parent bean -->
    class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target">
        <ref parent="accountService"/> <!-- notice how we refer to the parent bean -->
    </property>
    <!-- insert other configuration and dependencies as required here -->
</bean>
```

### 内部bean
所谓的内部bean（inner bean）是指在一个bean的`<property/>`或 `<constructor-arg/>`元素中使用`<bean/>`元素定义的bean。内部bean定义不需要有id或name属性，即使指定id 或 name属性值也将会被容器忽略。

```
<bean id="outer" class="...">
    <!-- instead of using a reference to a target bean, simply define the target bean inline -->
    <property name="target">
        <bean class="com.example.Person"> <!-- this is the inner bean -->
            <property name="name" value="Fiona Apple"/>
            <property name="age" value="25"/>
        </bean>
    </property>
</bean>
```
注意：内部bean中的scope标记及id或name属性将被忽略。内部bean总是匿名的且它们总是prototype模式的。同时将内部bean注入到包含该内部bean之外的bean是不可能的。

### 集合
通过`<list/>`、`<set/>`、`<map/>`及`<props/>`元素可以定义和设置与Java Collection类型对应List、Set、Map及Properties的值。

```
<bean id="moreComplexObject" class="example.ComplexObject">
    <!-- results in a setAdminEmails(java.util.Properties) call -->
    <property name="adminEmails">
        <props>
            <prop key="administrator">administrator@example.org</prop>
            <prop key="support">support@example.org</prop>
            <prop key="development">development@example.org</prop>
        </props>
    </property>
    <!-- results in a setSomeList(java.util.List) call -->
    <property name="someList">
        <list>
            <value>a list element followed by a reference</value>
            <ref bean="myDataSource" />
        </list>
    </property>
    <!-- results in a setSomeMap(java.util.Map) call -->
    <property name="someMap">
        <map>
            <entry key="an entry" value="just some string"/>
            <entry key ="a ref" value-ref="myDataSource"/>
        </map>
    </property>
    <!-- results in a setSomeSet(java.util.Set) call -->
    <property name="someSet">
        <set>
            <value>just some string</value>
            <ref bean="myDataSource" />
        </set>
    </property>
</bean>
```
注意：map的key或value值，或set的value值还可以是以下元素：
```
bean | ref | idref | list | set | map | props | value | null
```

### 集合的合并
Spring IoC容器将支持集合的合并。这样我们可以定义parent-style和child-style的`<list/>`、`<map/>`、`<set/>`或`<props/>`元素，子集合的值从其父集合继承和覆盖而来；也就是说，父子集合元素合并后的值就是子集合中的最终结果，而且子集合中的元素值将覆盖父集全中对应的值。

请注意，关于合并的这部分利用了parent-child bean机制。此内容将在后面介绍。

下面的例子展示了集合合并特性：
```
<beans>
    <bean id="parent" abstract="true" class="example.ComplexObject">
        <property name="adminEmails">
            <props>
                <prop key="administrator">administrator@example.com</prop>
                <prop key="support">support@example.com</prop>
            </props>
        </property>
    </bean>
    <bean id="child" parent="parent">
        <property name="adminEmails">
            <!-- the merge is specified on the child collection definition -->
            <props merge="true">
                <prop key="sales">sales@example.com</prop>
                <prop key="support">support@example.co.uk</prop>
            </props>
        </property>
    </bean>
<beans>
```
在上面的例子中，childbean的adminEmails属性的·<props/>·元素上使用了merge=true属性。当child bean被容器实际解析及实例化时，其adminEmails将与父集合的adminEmails属性进行合并。

```
administrator=administrator@example.com
sales=sales@example.com
support=support@example.co.uk
```
注意到这里子bean的Properties集合将从父`<props/>`继承所有属性元素。同时子bean的support值将覆盖父集合的相应值。

对于`<list/>`、`<map/>`及`<set/>`集合类型的合并处理都基本类似，在某个方面`<list/>`元素比较特殊，这涉及到List集合本身的语义学，就拿维护一个有序集合中的值来说，父bean的列表内容将排在子bean列表内容的前面。对于Map、Set及Properties集合类型没有顺序的概念，因此作为相关的Map、Set及Properties实现基础的集合类型在容器内部没有排序的语义。

### 集合合并的限制
你不能合并不同类型的集合(如Map跟List)，如果你尝试这么做的话，将会抛出异常。

### 强类型集合
泛型在Java5中被引入，这样你就可以使用强类型集合。比如，声明一个只能包含String类型元素的Collection。假若使用Spring来给bean注入强类型的Collection，那就可以利用Spring的类型转换能，当向强类型Collection中添加元素前，这些元素将被转换。

```
public class Foo {

    private Map<String, Float> accounts;

    public void setAccounts(Map<String, Float> accounts) {
        this.accounts = accounts;
    }
}
```

```
<beans>
    <bean id="foo" class="x.y.Foo">
        <property name="accounts">
            <map>
                <entry key="one" value="9.99"/>
                <entry key="two" value="2.75"/>
                <entry key="six" value="3.99"/>
            </map>
        </property>
    </bean>
</beans>
```
在foo bean的accounts属性被注入之前，通过反射，利用强类型`Map<String, Float>`的泛型信息，Spring的底层类型转换机制将会把各种value元素值转换为Float类型，因此字符串9.99、2.75及3.99就会被转换为实际的Float类型。
