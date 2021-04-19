### 1 主键生成策略

```java
@TableId(value = "id", type = IdType.ID_WORKER_STR)
private String id;
```

![](https://gitee.com/ZXiangC/picture/raw/master/imgs/20190508113713548.png)

### 2 使用 mapper.xml 文件查询语句

#### 第一步：步骤1 加载dao文件

加载方式1.在dao接口上增加mapper注解

```java
@Mapper
public interface AuthFunctionRepo extends BaseRepository<AuthFunction> 
```

加载方式2.在启动类加扫描注解

```java
@MapperScan(basePackages = {"com.yxl.smart.auth.repo"}) 
或者@MapperScan("com.yxl.smart.auth.repo")
```

#### 第二步： pop.xml 文件中添加扫描xml 文件

```sql
1 因为maven 默认不会扫描 *.xml 文件。所以需要配置。
```

#### 第三步 ： yml 文件中配置 xml 文件位置

```java
mybatis-plus:
  mapper-locations: classpath:ccom/chen/mybatis_plus/mapper/xml/*.xml
```

