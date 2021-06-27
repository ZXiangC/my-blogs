### 1、配置说明

- 1）配置类别名

- properties 文件配置

```java
# 加上这个配置后，对应的 XML 文件中可以直接使用类名，而不用使用全限定的类名
mybatis-plus.type-aliases-package =com.chen.mybatis.entity
```

### 2、 动态  SQL片段抽取

- Sql 中可将重复的 sql 提取出来，使用时用 include 引用即可，最终达到 sql 重用的目的

```xml
    <!--预先定义sql --> 
    <sql id="selectUser"> select * from User</sql>
    
    <select id="findByIds"  resultType="User">
        <!--使用include 引用 --> 
        <include refid="selectUser"></include>
        
        <where>
            <foreach collection="array" open="id in(" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </where>
    </select>
```



### 3、动态 SQL  之  **if**

我们根据实体类的不同取值，使用不同的 SQL语句来进行查询。

比如在 id如果不为空时可以根据id查询，如果username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询中经常会碰到。

- xml 

```xml
<select id="findByCondition" parameterType="User" resultType="User">
    select * from User
    <where>
        <if test="id != 0 and id != null">0
            and id=#{id}
        </if>
        <if test="username!=null">
            and username=#{username}
        </if>
    </where>
</select>
```

- interface

```java
User findByCondition(User user);
```

- test 

```java
   @Test
    public void testFindCondition() {
        User u = userMapper.findByCondition(new User().setUsername("马超"));
        System.out.println(u.toString());
    }
```

### 4、动态 SQL  之<**foreach>** 

foreach标签的属性含义如下：

<foreach>标签用于遍历集合，它的属性：

•collection：代表要遍历的集合元素，注意编写时不要写#{}

•open：代表语句的开始部分

•close：代表结束部分

•item：代表遍历集合的每个元素，生成的变量名

•sperator：代表分隔符

#### 1）查询

参数分别为 数组，集合，map 三种方式作为入参。

- interface

```java
List<User> findByIds(Integer[] ids);

List<User> findByIdList(List<Integer> list);

List<User> findByIdMap(@Param("map") Map<String, Integer> map);
```

- xml

```xml
    <sql id="selectUser"> select * from User</sql>

    <!--参数是数组 --> 
    <select id="findByIds"  resultType="User">
        <include refid="selectUser"></include>
        <where>
            <foreach collection="array" open="id in(" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </where>
    </select>

     <!--参数是集合 --> 
    <select id="findByIdList"  resultType="User">
        <include refid="selectUser"></include>
        <where>
            <foreach collection="list" open="id in(" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </where>
    </select>

    <!--参数是 map --> 
    <select id="findByIdMap" parameterType="map"  resultType="User">
        <include refid="selectUser"></include>
        <where>
            <foreach collection="map" open="id in(" close=")" index="key" item="value" separator=",">
                #{value}
            </foreach>
        </where>
    </select>
```

- test

```java
@Test 
public void findByArray() {
        Integer[] ids = new Integer[]{2, 3};
        List<User> userList = userMapper.findByIds(ids);
        for (User user : userList) {
            System.out.println(user.toString());
        }
    }

@Test
public void testfindByIdList() {
    ArrayList<Integer> list = new ArrayList<>();
    list.add(2);
    list.add(3);

    List<User> userList = userMapper.findByIdList(list);
    for (User user : userList) {
        System.out.println(user.toString());
    }
}

@Test
public void testfindByIdMap() {
    Map<String, Integer> map = new HashMap<>();
    map.put("2", 2);
    map.put("3", 3);

    List<User> userList = userMapper.findByIdMap(map);
    for (User user : userList) {
        System.out.println(user.toString());
    }
}
```

#### 2）插入

参数分别为 数组，集合，map 三种方式作为入参。

- interface

```java
void insertByList(@Param("list") List<User> list);

void insertByArray(@Param("array") User[] users);

void insertByMap(@Param("map") Map<String, User> map);
```

- xml

```xml
    <sql id="insertUser"> insert into user(id,username,password) VALUES</sql>

     <!--参数是 list --> 
    <insert id="insertByList" parameterType="list" >
        <include refid="insertUser"></include>
        <foreach collection="list" index="index" item="user" separator=",">
            (#{user.id},#{user.username},#{user.password})
        </foreach>
    </insert>

     <!--参数是 array --> 
    <insert id="insertByArray" >
        <include refid="insertUser"></include>
        <foreach collection="array" index="index" item="user" separator=",">
            (#{user.id},#{user.username},#{user.password})
        </foreach>
    </insert>

     <!--参数是 map --> 
    <insert id="insertByMap" parameterType="map">
        <include refid="insertUser"></include>
        <foreach collection="map" index="key" item="user" separator=",">
            (#{user.id},#{user.username},#{user.password})
        </foreach>
    </insert>
```

- test

```java
 /**
     * 测试插入
     */
    @Test
    public void testinsertByList() {

        List<User> list = new ArrayList<User>();
        list.add(new User().setId(11).setUsername("11").setPassword("11"));
        list.add(new User().setId(22).setUsername("22").setPassword("22"));

        userMapper.insertByList(list);
    }

    @Test
    public void testinsertByArray() {

        User[] users = new User[2];

        for (int i = 0; i < users.length; i++) {
            users[i] = new User();
        }
        users[0].setId(111).setUsername("111").setPassword("111");
        users[1].setId(1111).setUsername("1111").setPassword("1111");

        for (int i = 0; i < users.length; i++) {
            System.out.println(users[i].toString());
        }

        userMapper.insertByArray(users);
    }

    @Test
    public void testinsertByMap() {

        Map<String, User> map = genUserMap();

        userMapper.insertByMap(map);
    }



    private Map<String, User> genUserMap() {

        Map<String, User> map = new HashMap<>();

        for (int i = 0; i < 2; i++) {
            User user = new User();
            user.setId(new Random().nextInt(1000)+45);
            user.setUsername(new Random().nextInt(1000)+"");
            user.setPassword(new Random().nextInt(1000)+"");
            map.put(i+"",user);
        }
        return map;
    }
```

