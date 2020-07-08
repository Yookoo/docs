# @Configuration注解
- 作用:
用来表示这个类是一个配置类,在配置类中可以使用@Bean注解声明需要被Spring容器纳入管理的类。但是还需要将MyConfig.class传入Spring的上下文中注入才能生效。

**@Configuration配置类中的对象的声明***

```java
/**
 * Spring 配置类 类似 Spring的XML 文件
 * 1.配置类需要有一个注解 @Configuration 用来表示这是一个配置类并将其下的@Bean注解的方法注入到Spring容器
 * 2.还需要将MyConfig.class传入Spring的上下文中注入才能生效
 *
 * Created by Administrator on 2017/11/17.
 */
@Configuration
public class MyConfig {
    /**
     * @Bean(name = "myBean") 中 使用 name 为Bean自定义名字
     * Spring注入的Bean默认都是单例的，如果不想是单例的需通过@Scope("prototype")指定
     *
     * @return
     */
    @Bean(name = "myBean")
    @Scope("prototype")
    public MyBean createMyBean() {
        return new MyBean();
    }

    @Bean
    public JeepFactoryBean createJeepFactoryBean() {
        return new JeepFactoryBean();
    }
}

```

- 原理
```
@Component 注解
```
