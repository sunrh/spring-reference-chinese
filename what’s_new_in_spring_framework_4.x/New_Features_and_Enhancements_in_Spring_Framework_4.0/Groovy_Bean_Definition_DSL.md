# 3.5.使用Groovy DSL配置Bean

从Spring4.0开始，就可以使用Groovy DSL来配置Bean。就好像使用XML配置一样，但会比XML更简单。使用Groovy你将会很方便的将Bean嵌套在你的引导代码(bootstrap code)中。例如：
```
def reader = new GroovyBeanDefinitionReader(myApplicationContext)
reader.beans {
    dataSource(BasicDataSource) {
        driverClassName = "org.hsqldb.jdbcDriver"
        url = "jdbc:hsqldb:mem:grailsDB"
        username = "sa"
        password = ""
        settings = [mynew:"setting"]
    }
    sessionFactory(SessionFactory) {
        dataSource = dataSource
    }
    myService(MyService) {
        nestedBean = { AnotherBean bean ->
            dataSource = dataSource
        }
    }
}
```
更多的消息请参考GroovyBeanDefinitionReader [javadocs](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/beans/factory/groovy/GroovyBeanDefinitionReader.html)。