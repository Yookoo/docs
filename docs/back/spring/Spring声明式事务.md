# Spring声明式事务

## 1、配置环境

### 	1）、引入事务相关的jar包

​	此处数据源使用c3p0数据源，读者可根据自己的需要引入其他的数据源。

​	引入spring-jdbc时会自动引入spring-tx,即可支持Spring声明式事务功能。

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>4.3.13.RELEASE</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.45</version>
</dependency>
<dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.1.1</version>
</dependency>
```

### 2）、编写相关配置类和业务层

​	配置类中包括： 数据源的配置、`JdbcTemplate`的配置

```java
@Configuration
@ComponentScan("com.zhuky.tx")
public class TxConfig {

    @Bean
    public DataSource dataSource() throws PropertyVetoException {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setUser("root");
        dataSource.setPassword("root");
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/default");
        return dataSource;
    }
	
    @Bean
    public JdbcTemplate jdbcTemplate() throws PropertyVetoException {
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource());
        return jdbcTemplate;
    }
}
```

​	此处需要注意在`new JdbcTemplate(dataSource())`时再次调用`dataSource()`方法并不会创建两个数据源，而是会从Spring的IOC容器中获取，如果不习惯这样使用，可使用下面这种常用方法。

```java
    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
        return jdbcTemplate;
    }
```

​	下表是业务层的编写：

​		实体类：

```java
public class User {
    private int id;
    private String username;
    private int age;
    //省略get/set/toString方法
}
```

​		Dao：

```java
@Repository
public class UserDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;
    public boolean insert(){
        String sql = "insert into tbl_user (username, age) values(?,?)";
        int i = jdbcTemplate.update(sql, "zhangsan", 18);
        return i > 0 ;
    }
}
```

​		Service：此处抛出运行时异常使Spring进行事务回滚。

```java
@Service
public class UserService {

    @Autowired
    private UserDao userDao;

    public boolean insertUser(){
        boolean success = userDao.insert();
        throw new RuntimeException("数据回滚");
    }
}
```

## 2、配置事务管理：

### 	1）、在Service层添加`@Transactional`注解

```java
@Service
@Transactional
public class UserService {}
```

### 	2）、开启注解式事务管理

​		 使用xml时：

```xml
<tx:annotation-driven/>
```

​		使用配置类：添加`@EnableTransactionManagement`注解

```java
@Configuration
@ComponentScan("com.zhuky.tx")
@EnableTransactionManagement
public class TxConfig {}
```

### 	3）、配置事务管理器

​		 使用xml时：

```xml
    <!--配置c3p0连接池-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/default"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
    <!-- 配置事务管理器 -->
    <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
```

​		使用配置类：

```java
    @Bean
    public PlatformTransactionManager transactionManager() throws PropertyVetoException {
        return new DataSourceTransactionManager(dataSource());
    }
```

## 3、测试

​	编写测试类：

```java
public class TestOfTx {

    @Test
    public void test01(){
        AnnotationConfigApplicationContext applicationContext = new AnnotationConfigApplicationContext(TxConfig.class);
        UserService userService = applicationContext.getBean(UserService.class);
        boolean b = userService.insertUser();
    }
}
```

​	测试结果：

​	控制台抛出运行时异常：数据回滚

	java.lang.RuntimeException: 数据回滚
		at com.zhuky.tx.service.UserService.insertUser(UserService.java:17)
		...
​	数据库并没有成功添加数据，说明事务回滚了。

![数据库信息](img\TIM截图20181206144903.jpg)

## 4、声明式事务的原理

​	AOP相同

