

### 4.1 使用命令行

[参考链接](https://www.cnblogs.com/www-yang-com/p/10427518.html)

#### 4.1.1 创建仓库将本地代码提交到远程

- 第一步：初始化本地版本库

```java
git init
```

![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318161512290.png)

- 第二步：将所有代码添加到暂存区

```java
git add .
```

![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318161648159.png)

- 第三步：提交到本地版本库

```java
git commit -m '提交信息'
```

![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318161810976.png)

- 第四步：添加远程仓库

```java
git remote add origin 仓库地址

```

![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318162002672.png)

- 第五步：表示第一次推送需要拉取一下代码（相当于连接到远程仓库 ）

```java
git pull --rebase origin master
```

![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318162123552.png)

- 第六步：正式上传至码云中

```java
git push origin master
```

![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318162333396.png)

若上传有问题，可以试试   

```java
git push origin master -f 表示舍弃线上的文件，强制推送
```

#### 4.1.2 将远程仓库代码clone 到本地，并提交

- 第一步

  ```shell
  git clone 远程仓库地址
  ```

- 第二步 ：修改，新增文件后

  ```shell
  git add .
  git commit -m '记录'
  ```

- 第三步 ：直接推送到远程

  ```shell
  git push
  ```

### 4.2 使用IDEA

#### 4.2.1 IDEA配置git 

Settings->Versions Control->Git
![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318165323171.png)
![在这里插入图片描述](../../../../../NoteLog/hs/Hs/docs/%25E5%25BC%2580%25E5%258F%2591%25E5%25B7%25A5%25E5%2585%25B7/Git/01%2520git%2520%25E5%259F%25BA%25E6%259C%25AC%25E4%25BD%25BF%25E7%2594%25A8.assets/20200318165411525.png)

#### 4.2.2 初始化仓库(相当于 git init)

```java
1 这个操作相当于git init 
```

![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318170217369.png)

#### 4.2.3 IDEA中Git操作（相当于git add. git commit -m）

```java
1 选中项目-右键
2 选择git 这里面封装了对git所有操作
```

![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318171235228.png)

#### 4.2.4 配置连接远程仓库

配置远程仓库地址（相当于 git remote add origin）
![在这里插入图片描述](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318171818978.png)
连接一下仓库（第一次需要连接一下仓库）

```java
git pull --rebase origin master
```

### Git 版本控制

#### 1 直接回退到上一个版本

[回退到上一个版本](https://blog.csdn.net/hehyyoulan/article/details/93193562)

[参考链接](https://blog.csdn.net/yhch1024/article/details/81220858)

- 图片
![test](02-git%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20171220184945791)

| hello | 卧槽 | 武   |
| ----- | ---- | ---- |
| 111   | 111  | 111  |
| 11    | 11   | 11   |
| 11    | 11   | 11   |

```html run
<template>
  <el-tabs type="border-card">
    <el-tab-pane label="用户管理">用户管理</el-tab-pane>
    <el-tab-pane label="配置管理">配置管理</el-tab-pane>
    <el-tab-pane label="角色管理">角色管理</el-tab-pane>
    <el-tab-pane label="定时任务补偿">定时任务补偿</el-tab-pane>
  </el-tabs>
</template>
```

```html run
<template>
  <h2 class="title">{{name}} DEMO利器!</h2>
</template>
<script>
  export default {
    data () {
      return {
        name: 'docsify-plugin-run'
      }
    }
  }
</script>
<style>
  .title {
    color: #42b983;
  }
</style>
```

https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210104220203502.png

![image-20210104220310668](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210104220310668.png)

```vue
<template>
  <div>
    <br />
    <!-- 数据展示 -->
    <el-table :data="list" style="width: 100%">
      <el-table-column label="序号" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">
            {{ (page - 1) * limit + scope.$index + 1 }}</span
          >
        </template>
      </el-table-column>
      <el-table-column label="姓名" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.name }}</span>
        </template>
      </el-table-column>
      <el-table-column label="讲师简介" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.intro }}</span>
        </template>
      </el-table-column>
      <el-table-column label="讲师资历" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{ scope.row.career }}</span>
        </template>
      </el-table-column>
      <el-table-column label="讲师头衔" width="180">
        <template slot-scope="scope">
          <span style="margin-left: 10px">{{
            scope.row.level == 1 ? "高级讲师" : "首席讲师"
          }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)"
            >编辑</el-button
          >
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.row.id)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页条 -->
    <!-- 分页 -->
    <el-pagination
      :current-page="page"
      :page-size="limit"
      :total="total"
      style="padding: 30px 0; text-align: center"
      layout="total, prev, pager, next, jumper"
      @current-change="pageCurrentChange"
    />
  </div>
</template>

<script >
import teacher from "@/api/edu/teacher1";

export default {
  // 定义变量双重绑定
  data() {
    return {
      list: null,
      pojo: null,
      page: 1,
      limit: 2,
      total: null,
      searchMap: {},
    };
  },

  created() {
    this.getTeacherPageList();
  },

  methods: {
    /**
     * 取值函数
     *  */
    getTeacherPageList() {
     // alert(this.page + "----------" + this.limit);
      teacher.search(this.page, this.limit, this.searchMap).then((res) => {
        this.list = res.data.rows;
        this.total = res.data.total;
        //console.log(this.list);
        //console.log(this.total);
      });
    },
    /**
     * 操作处理函数
     */
    handleEdit(index, row) {
      this.pojo = teacher.findByid(row.id).then((res) => {
        this.pojo = res.data.teacher;

        console.log(this.pojo);
      });

      //console.log(index, row);
    },


    handleDelete(id) {
      this.$confirm("此操作将删除讲师记录, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      }).then(() => {
        teacher.deleteById(id).then((res) => {
          this.$message({
            type: res.success == 1 ? "success" : "error",
            message: res.message,
          });
          // 回到查询页面
          this.getTeacherList();
        });
      });
    },
  },
};
</script>
```

