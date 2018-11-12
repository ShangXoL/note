## @Configuration的使用

* AnnotationConfigApplicationContext 搭配上 @Configuration 和 @Bean 注解，自此，XML 配置方式不再是 Spring IoC 容器的唯一配置方式。两者在一定范围内存在着竞争的关系，但是它们在大多数情况下还是相互协作的关系，两者的结合使得 Spring IoC 容器的配置更简单，更强大。

* 代码实例：
```
@Configuration
public class MyConfig{
    @Bean
    public UserDao userDao(){
    
        return new UserDaoImpl();
    }
}
```

*  [参考资料](https://www.jianshu.com/p/06b2f181d799)