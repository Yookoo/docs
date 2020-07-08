---
title: 上下文
---

# AnnotationConfigApplicationContext



```java
/**
 * Spring4 介绍
 * AnnotationConfigApplicationContext 基于注解的ApplicationContext，继承自ApplicationContext，用于获取Spring的上下文。
 * 参数： 1.class文件,需要被注入的类的class文件
 *
 * Created by Administrator on 2017/11/17.
 */
public class App {

    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MyConfig.class);
        /**
         * bean的获取
         * 1.通过Bean的名字获取，bean的名字默认为配置类中的方法的名字(如createJeepFactoryBean),
         *      如果使用自定义名字需要在@Bean中指定 @Bean(name = "myBean")
         * 2.通过Bean的类文件来获取
         */
        System.out.println("createMyBean " +  context.getBean("myBean"));
        System.out.println("MyBean.class " +  context.getBean(MyBean.class));
        /**
         * 这里我们只在配置文件中注入了JeepFactoryBean，并没有注入Jeep，Jeep是通过JeepFactoryBean工厂注入的。
         * 我们发现获取JeepFactoryBean获取到的却是Jeep，这是因为JeepFactoryBean是一个特殊的Bean
         */
        System.out.println("createJeepFactoryBean " +  context.getBean("createJeepFactoryBean"));
        System.out.println("Jeep.class " +  context.getBean(Jeep.class));
        context.close();
    }
}

```