# 3.6.内核改进

对于内核的改进主要体现在以下几个方面：
* Spring现在可以在注入中使用泛型类型([generic types as a form of qualifier]())。例如你想使用Spring Data的Repository，你仅仅需要做如下实现：@Autowired Repository<Customer> customerRepository(以前的实现比较麻烦要set一下)。
* 如果你开启了Spring的元注解(meta-annotation)支持，你还可以基于源注解
