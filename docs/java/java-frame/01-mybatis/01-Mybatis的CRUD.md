

### 1、准备测试表

![image-20210525223728435](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210525223728435.png)

- 建表语句

```sql
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `username` varchar(128) DEFAULT NULL COMMENT '用户名',
  `password` varchar(128) DEFAULT NULL COMMENT '密码',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1112 DEFAULT CHARSET=utf8 COMMENT=' ';
```

### 2、MyBatis的映射文件概述

![图片6](https://gitee.com/ZXiangC/picture/raw/master/imgs/图片6.png)

### 3、MyBatis的增删改查操作

#### 1） MyBatis的插入数据操作 

**1)编写UserMapper映射文件**

```xml
<mapper namespace="userMapper">    
	<insert id="add" parameterType="User">        
		insert into user values(#{id},#{username},#{password})    
	</insert>
</mapper>
```

**3)插入操作注意问题**

• 插入语句使用insert标签

• 在映射文件中使用parameterType属性指定要插入的数据类型

•Sql语句中使用#{实体属性名}方式引用实体中的属性值

#### 2） MyBatis的修改数据操作 

**1)编写UserMapper映射文件**

```xml
 <mapper namespace="userMapper">
    <update id="update" parameterType="User">
        update user set username=#{username},password=#{password} where id=#{id}
    </update>
</mapper>
```

**3)修改操作注意问题**

• 修改语句使用update标签

#### 3） MyBatis的删除数据操作 

**1)编写UserMapper映射文件**

```xml
<mapper namespace="userMapper">
    <delete id="delete" parameterType="java.lang.Integer">
        delete from user where id=#{id}
    </delete>
</mapper>

```

**3)删除操作注意问题**

• 删除语句使用delete标签

•Sql语句中使用#{任意字符串}方式引用传递的单个参数

