# 5.2.3.使用容器

在`ApplicationContext`中调用方法`T getBean(String name, Class<T> requiredType)`你就可以获得你定义Bean的实例化对象。如下例所示：
```
// create and configure beans
ApplicationContext context =
    new ClassPathXmlApplicationContext(new String[] {"services.xml", "daos.xml"});

// retrieve configured instance
PetStoreService service = context.getBean("petStore", PetStoreService.class);

// use configured instance
List<String> userList = service.getUsernameList();
```
