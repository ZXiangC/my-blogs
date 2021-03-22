###  1 自动填充

- 第一步： 需要一个Handler 实现MetaObjectHandler

```java
package com.chen.mybatis_plus.handler;

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
        this.setFieldValByName("version", 1, metaObject);
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        LOGGER.info("start update fill ....");
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }

}
```

- 第二步：pojo 属性中需要添加 **@TableField(**fill = FieldFill.INSERT_UPDATE)

### 2 逻辑删除

- 第一步：配置逻辑删除插件

  ```java
       /**
       * 配置逻辑删除插件
       * @return
       */
      @Bean
      public ISqlInjector sqlInjector() {
          return new LogicSqlInjector();
      }
  ```

- 第二步：pojo 属性中添加注解 （注意：需要deleted 这个属性）

  ```java
    @TableLogic
      private Integer deleted;
  ```

### 3 乐观锁，实现version 控制

- 第一步： 添加属性 version

  ```sql
  alter table user add version int;
  ```

- 第二步：配置插件

  ```java
    /**
       * 乐观锁插件
       */
      @Bean
      public OptimisticLockerInterceptor optimisticLockerInterceptor() {
          return new OptimisticLockerInterceptor();
      }
  ```

- 第三步：pojo 中添加注解

  ```java
      @Version
      @TableField(fill = FieldFill.INSERT)
      private Integer version;//版本号
  ```

### 4 性能分析插件，分析每条语句执行时间

- 第一步： 配置插件

```java
 /**
     * SQL 执行性能分析插件
     * 开发环境使用，线上不推荐。 maxTime 指的是 sql 最大执行时长
     *
     * 三种环境
     *      * dev：开发环境
     *      * test：测试环境
     *      * prod：生产环境
     */
    @Bean
    @Profile({"dev","test"})// 设置 dev test 环境开启
    public PerformanceInterceptor performanceInterceptor() {
        PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
        performanceInterceptor.setMaxTime(500);//ms，超过此处设置的ms则sql不执行
        performanceInterceptor.setFormat(true);
        return performanceInterceptor;
    }
```

- 第二步：配置环境

```java
#环境设置：dev、test、prod
spring.profiles.active=dev
```

