# MyBatis 缓存详解

参考文档：[MyBatis官方文档](http://www.mybatis.org/mybatis-3/zh/index.html)

MyBatis的缓存主要分为两种一级缓存也叫本地缓存（local cache）和二级缓存（second level cache）。

### 一级缓存、本地缓存

*一级缓存是session级缓存，即缓存只在session范围生效。*

每当一个新 session 被创建，MyBatis 就会创建一个与之相关联的本地缓存。任何在 session 执行过的查询语句本身都会被保存在本地缓存中，那么，相同的查询语句和相同的参数所产生的更改就不会二度影响数据库了。本地缓存会被增删改、提交事务、关闭事务以及关闭 session 所清空。

默认情况下，本地缓存数据可在整个 session 的周期内使用，这一缓存需要被用来解决循环引用错误和加快重复嵌套查询的速度，所以它不可以被禁用掉，但是你可以设置 localCacheScope=STATEMENT 表示缓存仅在语句执行时有效。

注意，如果 localCacheScope 被设置为 SESSION，那么 MyBatis 所返回的引用将传递给保存在本地缓存里的相同对象。对返回的对象（例如 list）做出任何更新将会影响本地缓存的内容，进而影响存活在 session 生命周期中的缓存所返回的值。因此，不要对 MyBatis 所返回的对象作出更改，以防后患。

手动清空本地缓存：

```java
void clearCache()
```

### 二级缓存

*二级缓存是namespace级缓存，二级缓存会在同一 namespace中生效。*

默认情况下，MyBatis 3 没有开启二级缓存，要开启二级缓存,你需要在你的 SQL 映射文件（mapper.xml）中添加一行:

```html
<cache/>
```

*其实还需要在配置文件中把`mybatis.configuration.cache-enabled`设置为true(默认为true)，若添加`<cache/>`标签后缓存不生效，可以检查是否将其设置为了false*

字面上看就是这样。这个简单语句的效果如下:

- 映射语句文件中的所有 select 语句将会被缓存。
- 映射语句文件中的所有 insert,update 和 delete 语句会刷新缓存。
- 缓存会使用 Least Recently Used(LRU,最近最少使用的)算法来收回。
- 根据时间表(比如 no Flush Interval,没有刷新间隔), 缓存不会以任何时间顺序 来刷新。
- 缓存会存储列表集合或对象(无论查询方法返回什么)的 1024 个引用。
- 缓存会被视为是 read/write(可读/可写)的缓存,意味着对象检索不是共享的,而 且可以安全地被调用者修改,而不干扰其他调用者或线程所做的潜在修改。

所有的这些属性都可以通过缓存元素的属性来修改。比如:

```html
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```

这个更高级的配置创建了一个 FIFO 缓存，并每隔 60 秒刷新，存数结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此在不同线程中的调用者之间修改它们会 导致冲突。

可用的收回策略有:

- `LRU` – 最近最少使用的:移除最长时间不被使用的对象。
- `FIFO` – 先进先出:按对象进入缓存的顺序来移除它们。
- `SOFT` – 软引用:移除基于垃圾回收器状态和软引用规则的对象。
- `WEAK` – 弱引用:更积极地移除基于垃圾收集器状态和弱引用规则的对象。

默认的缓存回收策略是 LRU。

flushInterval(刷新间隔)可以被设置为任意的正整数，而且它们代表一个合理的毫秒形式的时间段。默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新。

size(引用数目)可以被设置为任意正整数,要记住你缓存的对象数目和你运行环境的 可用内存资源数目。默认值是 1024。

readOnly(只读)属性可以被设置为 true 或 false。只读的缓存会给所有调用者返回缓 存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。可读写的缓存 会返回缓存对象的拷贝(通过序列化) 。这会慢一些,但是安全,因此默认是 false。

*若在SqlSession关闭时，SqlSession对应的本地缓存会自动转化为二级缓存。*

### 自定义缓存

使用自定缓存，只需要实现MyBatis的Cache接口并在`<cache/>`中配置缓存类型：

```h&#39;t
<cache type="com.domain.something.MyCustomCache"/>
```

*自定义缓存没有使用过，如果大家有兴趣可以参考[MyBatis官方文档](http://www.mybatis.org/mybatis-3/zh/index.html)自定义缓存部分*

### 后记

这篇文章主要由[MyBatis官方文档](http://www.mybatis.org/mybatis-3/zh/index.html)整理而来，用于记录我的学习过程，作为2019年的开始，以后的学习都需要有产出物，否则学了之后很快就会忘记。





