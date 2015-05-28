# 5.4.1.依赖注入

依赖注入（DI）背后的基本原理是对象之间的依赖关系（即一起工作的其它对象）只会通过以下几种方式来实现：构造器的参数、工厂方法的参数，或给由构造函数或者工厂方法创建的对象设置属性。因此，容器的工作就是创建bean时注入那些依赖关系。相对于由bean自己来控制其实例化、直接在构造器中指定依赖关系或者类似服务定位器（Service  Locator）模式这3种自主控制依赖关系注入的方法来说，控制从根本上发生了倒转，这也正是控制反转（Inversion of Control， IoC） 名字的由来。

应用DI原则后，代码将更加清晰。而且当bean自己不再担心对象之间的依赖关系（甚至不知道依赖的定义指定地方和依赖的实际类）之后，实现更高层次的松耦合将易如反掌。DI主要有两种注入方式，即Setter注入和构造器注入。

## 构造器注入
基于构造器的DI通过调用带参数的构造器来实现，每个参数代表着一个依赖。此外，还可通过给静态工厂方法传参数来构造bean。接下来的介绍将认为给构造器传参与给静态工厂方法传参是类似的。下面展示了只能使用构造器参数来注入依赖关系的例子。请注意，这个类并没有什么特别之处。
```
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on a MovieFinder
    private MovieFinder movieFinder;

    // a constructor so that the Spring container can inject a MovieFinder
    public SimpleMovieLister(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...

}
```
### 构造器参数解析
构造器参数解析根据参数类型进行匹配，如果bean的构造器参数类型定义非常明确，那么在bean被实例化的时候，bean定义中构造器参数的定义顺序就是这些参数的顺序，依次进行匹配，比如下面的代码
```
package x.y;

public class Foo {

    public Foo(Bar bar, Baz baz) {
        // ...
    }

}
```
上述例子中由于构造参数非常明确（这里我们假定Bar和Baz之间不存在继承关系）。因此下面的配置即使没有明确指定构造参数顺序（和类型）也会工作的很好。
```
<beans>
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
    </bean>

    <bean id="bar" class="x.y.Bar"/>

    <bean id="baz" class="x.y.Baz"/>
</beans>
```
我们再来看另一个bean，该bean的构造参数类型已知，匹配也没有问题(跟前面的例子一样)。但是当使用简单类型时，比如`<value>true<value>`，Spring将无法知道该值的类型。不借助其他帮助，他将无法仅仅根据参数类型进行匹配，比如下面的这个例子：
```
package examples;

public class ExampleBean {

    // Number of years to calculate the Ultimate Answer
    private int years;

    // The Answer to Life, the Universe, and Everything
    private String ultimateAnswer;

    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }

}
```
针对上面的场景可以通过使用'type'属性来显式指定那些简单类型的构造参数的类型，比如：
```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```
我们还可以通过index属性来显式指定构造参数的索引，比如下面的例子：
```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```
通过使用索引属性不但可以解决多个简单属性的混淆问题，还可以解决有可能有相同类型的2个构造参数的混淆问题了，注意index是从0开始。

我们更可以使用构造器上的参数名来传递：
```
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```
这种注入方法有个风险就是如果你的源代码在编译时关闭了debug标志，那么Spring就无法获取构造器的参数名了。不过可以使用[@ConstructorProperties](http://download.oracle.com/javase/6/docs/api/java/beans/ConstructorProperties.html)属性显式指定构造器参数名称。
```
package examples;

public class ExampleBean {

    // Fields omitted

    @ConstructorProperties({"years", "ultimateAnswer"})
    public ExampleBean(int years, String ultimateAnswer) {
        this.years = years;
        this.ultimateAnswer = ultimateAnswer;
    }

}
```
## Setter注入
通过调用无参构造器或无参static工厂方法实例化bean之后，调用该bean的setter方法，即可实现基于setter的DI。
下面的例子将展示只使用setter注入依赖。注意，这个类并没有什么特别之处，它就是普通的Java类。
```
public class SimpleMovieLister {

    // the SimpleMovieLister has a dependency on the MovieFinder
    private MovieFinder movieFinder;

    // a setter method so that the Spring container can inject a MovieFinder
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // business logic that actually uses the injected MovieFinder is omitted...

}
```
BeanFactory对于它所管理的bean提供两种注入依赖方式（实际上它也支持同时使用构造器注入和Setter方式注入依赖）。需要注入的依赖将保存在BeanDefinition中，它能根据指定的PropertyEditor实现将属性从一种格式转换成另外一种格式。然而，大部份的Spring用户并不需要直接以编程的方式处理这些类，而是采用XML的方式来进行定义，在内部这些定义将被转换成相应类的实例，并最终得到一个Spring IoC容器实例。

```
构造器注入还是Setter注入?
由于大量的构造器参数可能使程序变得笨拙，特别是当某些属性是可选的时候。因此通常情况下，Spring开发团队提倡使用setter注入。而且setter DI在以后的某个时候还可将实例重新配置（或重新注入）（JMX MBean就是一个很好的例子）。

尽管如此，构造器注入还是得到很多纯化论者（也有很好的理由）的青睐。一次性将所有依赖注入的做法意味着，在未完全初始化的状态下，此对象不会返回给客户代码（或被调用），此外对象也不需要再次被重新配置（或重新注入）。

对于注入类型的选择并没硬性的规定。只要能适合你的应用，无论使用何种类型的DI都可以。对于那些没有源代码的第三方类，或者没有提供setter方法的遗留代码，我们则别无选择－－构造器注入将是你唯一的选择。
```
处理bean依赖关系通常按以下步骤进行：
* 根据定义bean的配置（文件）创建并初始化BeanFactory实例（大部份的Spring用户使用支持XML格式配置文件的BeanFactory或ApplicationContext实现）。
* 每个bean的依赖将以属性、构造器参数、或静态工厂方法参数的形式出现。当这些bean被实际创建时，这些依赖也将会提供给该bean。
* 每个属性或构造器参数既可以是一个实际的值，也可以是对该容器中另一个bean的引用。
* 每个指定的属性或构造器参数值必须能够被转换成特定的格式或构造参数所需的类型。默认情况下，Spring会以String类型提供值转换成各种内置类型，比如int、long、String、boolean等。

Spring会在容器被创建时验证容器中每个bean的配置，包括验证那些bean所引用的属性是否指向一个有效的bean（即被引用的bean也在容器中被定义）。然而，在bean被实际创建之前，bean的属性并不会被设置。对于那些singleton类型和被设置为提前实例化的bean（比如ApplicationContext中的singleton bean）而言，bean实例将与容器同时被创建。而另外一些bean则会在需要的时候被创建，伴随着bean被实际创建，作为该bean的依赖bean以及依赖bean的依赖bean（依此类推）也将被创建和分配。
```
循环依赖

在采用构造器注入的方式配置bean时，很有可能会产生循环依赖的情况。

比如说，一个类A，需要通过构造器注入类B，而类B又需要通过构造器注入类A。如果为类A和B配置的bean被互相注入的话，那么Spring IoC容器将检测出循环引用，并抛出 BeanCurrentlyInCreationException异常。

对于此问题，一个可能的解决方法就是修改源代码，将某些构造器注入改为setter注入。另一个解决方法就是完全放弃构造器注入，只使用setter注入。换句话说，除了极少数例外，大部分的循环依赖都是可以避免的，不过采用setter注入产生循环依赖的可能性也是存在的。

与通常我们见到的非循环依赖的情况有所不同，在两个bean之间的循环依赖将导致一个bean在被完全初始化的时候被注入到另一个bean中（如同我们常说的先有蛋还是先有鸡的情况）。
```
通常情况下，你可以信赖Spring，它会在容器加载时发现配置错误（比如对无效bean的引用以及循环依赖）。Spring会在bean创建时才去设置属性和依赖关系（只在需要时创建所依赖的其他对象）。这意味着即使Spring容器被正确加载，当获取一个bean实例时，如果在创建bean或者设置依赖时出现问题，仍然会抛出一个异常。因缺少或设置了一个无效属性而导致抛出一个异常的情况的确是存在的。因为一些配置问题而导致潜在的可见性被延迟，所以在默认情况下，ApplicationContext实现中的bean采用提前实例化的singleton模式。在实际需要之前创建这些bean将带来时间与内存的开销。而这样做的好处就是ApplicationContext被加载的时候可以尽早的发现一些配置的问题。不过用户也可以根据需要采用延迟实例化来替代默认的singleton模式。

如果撇开循环依赖不谈，当协作bean被注入到依赖bean时，协作bean必须在依赖bean之前完全配置好。例如bean A对bean B存在依赖关系，那么Spring IoC容器在调用bean A的setter方法之前，bean B必须被完全配置，这里所谓完全配置的意思就是bean将被实例化（如果不是采用提前实例化的singleton模式），相关的依赖也将被设置好，而且所有相关的lifecycle方法（如IntializingBean的init方法以及callback方法）也将被调用。

###一些注入的例子
首先是一个用XML格式定义的Setter DI例子。相关的XML配置如下：
```
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- setter injection using the nested ref element -->
    <property name="beanOne">
        <ref bean="anotherExampleBean"/>
    </property>

    <!-- setter injection using the neater ref attribute -->
    <property name="beanTwo" ref="yetAnotherBean"/>
    <property name="integerProperty" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {

    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;

    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }

    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }

    public void setIntegerProperty(int i) {
        this.i = i;
    }

}
```
正如你所看到的，bean类中的setter方法与xml文件中配置的属性是一一对应的。接着是构造器注入的例子：
```
<bean id="exampleBean" class="examples.ExampleBean">
    <!-- constructor injection using the nested ref element -->
    <constructor-arg>
        <ref bean="anotherExampleBean"/>
    </constructor-arg>

    <!-- constructor injection using the neater ref attribute -->
    <constructor-arg ref="yetAnotherBean"/>

    <constructor-arg type="int" value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {

    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;

    public ExampleBean(
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {
        this.beanOne = anotherBean;
        this.beanTwo = yetAnotherBean;
        this.i = i;
    }

}
```
如你所见，在xml bean定义中指定的构造器参数将被用来作为传递给类ExampleBean构造器的参数。
现在来研究一个替代构造器的方法，采用static工厂方法返回对象实例：
```
<bean id="exampleBean" class="examples.ExampleBean" factory-method="createInstance">
    <constructor-arg ref="anotherExampleBean"/>
    <constructor-arg ref="yetAnotherBean"/>
    <constructor-arg value="1"/>
</bean>

<bean id="anotherExampleBean" class="examples.AnotherBean"/>
<bean id="yetAnotherBean" class="examples.YetAnotherBean"/>
```

```
public class ExampleBean {

    // a private constructor
    private ExampleBean(...) {
        ...
    }

    // a static factory method; the arguments to this method can be
    // considered the dependencies of the bean that is returned,
    // regardless of how those arguments are actually used.
    public static ExampleBean createInstance (
        AnotherBean anotherBean, YetAnotherBean yetAnotherBean, int i) {

        ExampleBean eb = new ExampleBean (...);
        // some other operations...
        return eb;
    }

}
```
请注意，传给static工厂方法的参数由constructor-arg元素提供，这与使用构造器注入时完全一样。而且，重要的是，工厂方法所返回的实例的类型并不一定要与包含static工厂方法的类类型一致。尽管在此例子中它的确是这样。非静态的实例工厂方法与此相同（除了使用factory-bean属性替代class属性外），因而不在此细述。
