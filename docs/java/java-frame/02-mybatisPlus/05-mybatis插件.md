## 一、性能分析插件

性能分析拦截器，用于输出每条 SQL 语句及其执行时间

SQL 性能执行分析,开发环境使用，超过指定时间，停止运行。有助于发现问题

### 1、配置插件

**（1）参数说明**

参数：maxTime： SQL 执行最大时长，超过自动停止运行，有助于发现问题。

参数：format： SQL是否格式化，默认false。

**（2）在 MybatisPlusConfig 中配置**

 

```java
/**
 * SQL 执行性能分析插件
 * 开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长
 */
@Bean
@Profile({"dev","test"})// 设置 dev test 环境开启
public PerformanceInterceptor performanceInterceptor() {
    PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
    performanceInterceptor.setMaxTime(100);//ms，超过此处设置的ms则sql不执行
    performanceInterceptor.setFormat(true);
    return performanceInterceptor;
}
```

**（3）Spring Boot 中设置dev环境**

 

```java
#环境设置：dev、test、prod
spring.profiles.active=dev
```

可以针对各环境新建不同的配置文件`application-dev.properties`、`application-test.properties`、`application-prod.properties`

也可以自定义环境名称：如test1、test2

### 2、测试

**（1）常规测试**

 

```java
/**
 * 测试 性能分析插件
 */
@Test
public void testPerformance() {
    User user = new User();
    user.setName("我是Helen");
    user.setEmail("helen@sina.com");
    user.setAge(18);
    userMapper.insert(user);
}
```

**输出：**

![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/bb355a17-3cdc-4f0a-82f2-232defbd235b.png)

**（2）将maxTime 改小之后再次进行测试**

 

```
performanceInterceptor.setMaxTime(5);//ms，超过此处设置的ms不执行
```

如果执行时间过长，则抛出异常：The SQL execution time is too large, 

**输出：**

![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/1ae5ae68-b6b2-4801-ae26-29835b175b24.png)

## 二、日志

### 1、日志级别

- *OFF* | 关闭：最高级别，不打印日志。 
- *FATAL* | 致命：指明非常严重的可能会导致应用终止执行错误事件。
- *ERROR* | 错误：指明错误事件，但应用可能还能继续运行。
- *WARN* | 警告：指明可能潜在的危险状况。 
- *INFO* | 信息：指明描述信息，从粗粒度上描述了应用运行过程。 
- *DEBUG* | 调试：指明细致的事件信息，对调试应用最有用。
- *TRACE* | 跟踪：指明程序运行轨迹，比DEBUG级别的粒度更细。 

- *ALL* | 所有：所有日志级别，包括定制级别。

所以，日志优先级别标准顺序为：

> ALL < TRACE < DEBUG < INFO < WARN < ERROR < FATAL < OFF

### 2、项目里面配置日志

- 只需要添加文件即可，不需要多余配置

![image-20210525200257713](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210525200257713.png)

##  三、逻辑删除插件

- 物理删除：真实删除，将对应数据从数据库中删除，之后查询不到此条被删除数据
- 逻辑删除：假删除，将对应数据中代表是否被删除字段状态修改为“被删除状态”，之后在数据库中仍旧能看到此条数据记录

### 1、准备配置

（1）数据库中添加 **deleted**字段

 

```sql
ALTER TABLE `user` ADD COLUMN `deleted` boolean
```

![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/bc4cbff4-c2b8-45d5-ae8d-53439dd2330c.png)

（2）实体类添加**deleted** 字段

并加上 @TableLogic 注解 和 @TableField(fill = FieldFill.INSERT) 注解

 

```java
@TableLogic
@TableField(fill = FieldFill.INSERT)
private Integer deleted;
```

**（3）元对象处理器接口添加deleted的insert默认值**

 

```java
@Override
public void insertFill(MetaObject metaObject) {
    ......
    this.setFieldValByName("deleted", 0, metaObject);
}
```

**（4）application.properties 加入配置**

此为默认值，如果你的默认值和mp默认的一样,该配置可无

 

```java
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

### 2、注册插件

**（5）在 MybatisPlusConfig 中注册 Bean**

 

```java
@Bean
public ISqlInjector sqlInjector() {
    return new LogicSqlInjector();
}
```

**（6）测试逻辑删除**

- 测试后发现，数据并没有被删除，deleted字段的值由0变成了1
- 测试后分析打印的sql语句，是一条update
- **注意：**被删除数据的deleted 字段的值必须是 0，才能被选取出来执行逻辑删除的操作

 

```java
/**
 * 测试 逻辑删除
 */
@Test
public void testLogicDelete() {

    int result = userMapper.deleteById(1L);
    System.out.println(result);
}
```

**（7）测试逻辑删除后的查询**

MyBatis Plus中查询操作也会自动添加逻辑删除字段的判断

 

```java
/**
 * 测试 逻辑删除后的查询：
 * 不包括被逻辑删除的记录
 */
@Test
public void testLogicDeleteSelect() {
    User user = new User();
    List<User> users = userMapper.selectList(null);
    users.forEach(System.out::println);
}
```

测试后分析打印的sql语句，包含 WHERE deleted=0 

```sql
SELECT id,name,age,email,create_time,update_time,deleted FROM user WHERE deleted=0
```

## 四、乐观锁

**主要适用场景：**当要更新一条记录的时候，希望这条记录没有被别人更新，也就是说实现线程安全的数据更新

**乐观锁实现方式：**

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

### 1、准备配置

***\*（1）数据库中添加version字段\****

 

```
ALTER TABLE `user` ADD COLUMN `version` INT
```



![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/7bf260f8-d483-49fe-b448-b2fbea3dddaf.png)

***\*（2）实体类添加version字段\****

并添加 @Version 注解

 

```java
@Version
@TableField(fill = FieldFill.INSERT)
private Integer version;
```

***\*（3）元对象处理器接口添加version的insert默认值\****

 

```java
@Override
public void insertFill(MetaObject metaObject) {
    ......
    this.setFieldValByName("version", 1, metaObject);
}
```

**特别说明:**

支持的数据类型只有 int,Integer,long,Long,Date,Timestamp,LocalDateTime整数类型下 `newVersion = oldVersion + 1``newVersion` 会回写到 `entity` 中仅支持 `updateById(id)` 与 `update(entity, wrapper)` 方法在 `update(entity, wrapper)` 方法下, `wrapper` 不能复用!!!

### 2、注册插件

**（4）\**在 MybatisPlusConfig 中注册 Bean\****

 

```java
package com.atguigu.mybatisplus.config;

import com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@EnableTransactionManagement
@Configuration
@MapperScan("com.atguigu.mybatis_plus.mapper")
public class MybatisPlusConfig {

    /**
     * 乐观锁插件
     */
    @Bean
    public OptimisticLockerInterceptor optimisticLockerInterceptor() {
        return new OptimisticLockerInterceptor();
    }
}
```

**（5）测试乐观锁可以修改成功**

测试后分析打印的sql语句，将version的数值进行了加1操作

 

```java
/**
 * 测试 乐观锁插件
 */
@Test
public void testOptimisticLocker() {

    //查询
    User user = userMapper.selectById(1L);
    //修改数据
    user.setName("Helen Yao");
    user.setEmail("helen@qq.com");
    //执行更新
    userMapper.updateById(user);
}
```

**（5）测试乐观锁修改失败**

 

```java
/**
 * 测试乐观锁插件 失败
 */
@Test
public void testOptimisticLockerFail() {

    //查询
    User user = userMapper.selectById(1L);
    //修改数据
    user.setName("Helen Yao1");
    user.setEmail("helen@qq.com1");

    //模拟取出数据后，数据库中version实际数据比取出的值大，即已被其它线程修改并更新了version
    user.setVersion(user.getVersion() - 1);

    //执行更新
    userMapper.updateById(user);
}
```

## 五、逻辑删除

- 物理删除：真实删除，将对应数据从数据库中删除，之后查询不到此条被删除数据
- 逻辑删除：假删除，将对应数据中代表是否被删除字段状态修改为“被删除状态”，之后在数据库中仍旧能看到此条数据记录

### 1、准备配置

（1）数据库中添加 **deleted**字段

 

```sql
ALTER TABLE `user` ADD COLUMN `deleted` boolean
```

![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/bc4cbff4-c2b8-45d5-ae8d-53439dd2330c.png)

（2）实体类添加**deleted** 字段

并加上 @TableLogic 注解 和 @TableField(fill = FieldFill.INSERT) 注解

 

```java
@TableLogic
@TableField(fill = FieldFill.INSERT)
private Integer deleted;
```

**（3）元对象处理器接口添加deleted的insert默认值**

 

```java
@Override
public void insertFill(MetaObject metaObject) {
    ......
    this.setFieldValByName("deleted", 0, metaObject);
}
```

**（4）application.properties 加入配置**

此为默认值，如果你的默认值和mp默认的一样,该配置可无

 

```java
mybatis-plus.global-config.db-config.logic-delete-value=1
mybatis-plus.global-config.db-config.logic-not-delete-value=0
```

### 2、注册插件

**（5）在 MybatisPlusConfig 中注册 Bean**

 

```java
@Bean
public ISqlInjector sqlInjector() {
    return new LogicSqlInjector();
}
```

**（6）测试逻辑删除**

- 测试后发现，数据并没有被删除，deleted字段的值由0变成了1
- 测试后分析打印的sql语句，是一条update
- **注意：**被删除数据的deleted 字段的值必须是 0，才能被选取出来执行逻辑删除的操作

 

```java
/**
 * 测试 逻辑删除
 */
@Test
public void testLogicDelete() {

    int result = userMapper.deleteById(1L);
    System.out.println(result);
}
```

**（7）测试逻辑删除后的查询**

MyBatis Plus中查询操作也会自动添加逻辑删除字段的判断

 

```java
/**
 * 测试 逻辑删除后的查询：
 * 不包括被逻辑删除的记录
 */
@Test
public void testLogicDeleteSelect() {
    User user = new User();
    List<User> users = userMapper.selectList(null);
    users.forEach(System.out::println);
}
```

测试后分析打印的sql语句，包含 WHERE deleted=0 

```sql
SELECT id,name,age,email,create_time,update_time,deleted FROM user WHERE deleted=0
```

## 六、自动填充

项目中经常会遇到一些数据，每次都使用相同的方式填充，例如记录的创建时间，更新时间等。

我们可以使用MyBatis Plus的自动填充功能，完成这些字段的赋值工作：

### 1、准备配置

（1）数据库表中添加自动填充字段

在User表中添加datetime类型的新的字段 *create_time*、*update_time*

（2）实体上添加注解

 

```java
@Data
public class User {
    ......
        
    @TableField(fill = FieldFill.INSERT)
    private Date createTime;

    //@TableField(fill = FieldFill.UPDATE)
    @TableField(fill = FieldFill.INSERT_UPDATE)
    private Date updateTime;
}
```

### 2、实现接口

（3）实现元对象处理器接口

**注意：不要忘记添加 @Component 注解**



```java
package com.atguigu.mybatisplus.handler;
import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import java.util.Date;

@Component
public class MyMetaObjectHandler implements MetaObjectHandler {

    private static final Logger LOGGER = LoggerFactory.getLogger(MyMetaObjectHandler.class);

    @Override
    public void insertFill(MetaObject metaObject) {
        LOGGER.info("start insert fill ....");
        this.setFieldValByName("createTime", new Date(), metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        LOGGER.info("start update fill ....");
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
}
```

### 