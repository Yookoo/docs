## Mybatis插件之通用 Mapper

## 作用

​	生成 常用增删改查的Mapper

## 与 MBG 区别

​	MBG 修改很麻烦

## 实体类使用包装类型的原因

Java 的基本数据类型都有默认值会导致 mybatis 在执行相关操作的时候无法判断当前字段是否为 null

所以实体类中的属性尽量不要使用基本数据类型。



## 集成通用 Mapper

添加 Maven 依赖



添加配置

MapperScannerConfiger 改为通用mapper 的MapperScannerConfiger

org-->tk



Mapper<T> 接口

@Table 注解

@Column

常用方法

selectOne

