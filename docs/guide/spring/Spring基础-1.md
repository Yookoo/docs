# Spring基础

## Spring容器AnnotationConfigApplication的创建

- 使用传入组件的.class文件方式创建
- 使用传入组件的包名的方式创建
- 使用Configrition,AnnotationScan("包名")组合注解方式,(可以排除包下不需要创建的类)

## Bean装配（Spring-Context 提供）

- 使用@Configuration注解 装配配置文件
- 使用Bean注解 装配Bean
- 使用Conpommet注解 装配组件
- 使用Repository注解 装配Dao
- 使用Service注解 装配 Service
- 使用Controller注解装配 Controller

## Bean 依赖注入

- 使用Autoware 自动注入Bean（Spring-Context 提供）
- 使用Resource 自动注入Bean（JSR-250,jdk提供，无需引入其他Jar）
- 使用Inject 自动注入Bean（JSR-330,jdk提供，需引入javax.inject包）


备注: Spring-Context 包 包含spring-context spring-aop spring-beans spring-core spring-expression commons-logging