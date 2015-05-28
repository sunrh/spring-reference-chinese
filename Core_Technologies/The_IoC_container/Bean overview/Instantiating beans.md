# 5.3.2.实例化Bean

从本质上来说，bean定义描述了如何创建一个或多个对象实例。当需要的时候， 容器会从bean定义列表中取得一个指定的bean定义，并根据bean定义里面的配置元数据 使用反射机制来创建（或取得）一个实际的对象。

当采用XML描述配置元数据时，将通过`<bean/>`元素的class属性来指定实例化对象的类型。class 属性 (对应BeanDefinition实例的Class属性)通常是必须的(不过也有两种例外的情形，见 [使用实例工厂方法实例化]()和 [5.7.bean定义的继承]())。class属性主要有两种用途 ：
* 在大多数情况下，容器将直接通过反射调用指定类的构造器来创建bean(这有点类似于 在Java代码中使用new操作符)。
* 在极少数情况下，容器将调用 类的静态工厂方法来创建bean实例，class 属性将用来指定实际具有静态工厂方法的类(至于调用静态工厂方法创建的对象类型是当前class还是其他的class则无关紧要)。

```
内部类名

如果需要你希望将一个静态的内部类配置为一个bean的话，那么内部类的名字需要采用二进制的写法。

比如说，在com.example包下有一个叫Foo的类，而Foo类有一个静态的内部类叫Bar，那么在bean定义的时候，class属性必须这样写：
com.example.Foo$Bar
注意这里我们使用了$字符将内部类和外部类进行分隔
```

## 用构造器来实例化
当采用构造器来创建bean实例时，Spring对class并没有特殊的要求，我们通常使用的class都适用。也就是说，被创建的类并不需要实现任何特定的接口，或以特定的方式编码，只要指定bean的class属性即可。不过根据所采用的IoC类型，class可能需要一个默认的空构造器。

此外，IoC容器不仅限于管理JavaBean，它可以管理任意的类。不过大多数使用Spring的人喜欢使用实际的JavaBean(具有默认的(无参)构造器及setter和getter方法)，但在容器中使用非bean形式(non-bean style)的类也是可以的。比如遗留系统中的连接池，很显然它与JavaBean规范不符，但Spring也能管理它。

当使用基于XML的元数据配置文件，可以这样来指定bean类：
```
<bean id="exampleBean" class="examples.ExampleBean"/>

<bean name="anotherExample" class="examples.ExampleBeanTwo"/>
```
给构造函数指定参数以及为bean实例设置属性将在随后的[依赖注入]()中谈及。

## 使用静态工厂方法实例化
当采用静态工厂方法创建bean时，除了需要指定class属性外，还需要通过factory-method属性来指定创建bean实例的工厂方法。Spring将调用此方法(其可选参数接下来介绍)返回实例对象，就此而言，跟通过普通构造器创建类实例没什么两样。

下面的bean定义展示了如何通过工厂方法来创建bean实例。注意，此定义并未指定返回对象的类型，仅指定该类包含的工厂方法。在此例中，createInstance()必须是一个static方法。

```
<bean id="clientService"
    class="examples.ClientService"
    factory-method="createInstance"/>
```

```
public class ClientService {
    private static ClientService clientService = new ClientService();
    private ClientService() {}

    public static ClientService createInstance() {
        return clientService;
    }
}
```
给工厂方法指定参数以及为bean实例设置属性将在随后的部份中谈及。

## 使用实例工厂方法实例化

与使用静态工厂方法实例化类似，用来进行实例化的非静态实例工厂方法位于另外一个bean中，容器将调用该bean的工厂方法来创建一个新的bean实例。为使 用此机制，class属性必须为空，而factory-bean属性必须指定为当前(或其祖先)容器中包含工厂方法的bean的名称，而该工厂bean的工厂方法本身必须通过factory-method属性来设定。
```
<!-- the factory bean, which contains a method called createInstance() -->
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<!-- the bean to be created via the factory bean -->
<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>
```

```
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();
    private DefaultServiceLocator() {}

    public ClientService createClientServiceInstance() {
        return clientService;
    }
}
```
一个工厂类也可以有多个工厂方法，如下代码所示：
```
<bean id="serviceLocator" class="examples.DefaultServiceLocator">
    <!-- inject any dependencies required by this locator bean -->
</bean>

<bean id="clientService"
    factory-bean="serviceLocator"
    factory-method="createClientServiceInstance"/>

<bean id="accountService"
    factory-bean="serviceLocator"
    factory-method="createAccountServiceInstance"/>
```

```
public class DefaultServiceLocator {

    private static ClientService clientService = new ClientServiceImpl();
    private static AccountService accountService = new AccountServiceImpl();

    private DefaultServiceLocator() {}

    public ClientService createClientServiceInstance() {
        return clientService;
    }

    public AccountService createAccountServiceInstance() {
        return accountService;
    }

}
```
虽然设置bean属性的机制仍然在这里被提及，但隐式的做法是由工厂bean自己来管理以及通过依赖注入(DI)来进行配置。
```
Spring文档中的factory bean指的是配置在Spring容器中通过使用 实例 或 静态工厂方法创建对象的一种bean。而文档中的FactoryBean （注意首字母大写）指的是Spring特有的 FactoryBean。
```
