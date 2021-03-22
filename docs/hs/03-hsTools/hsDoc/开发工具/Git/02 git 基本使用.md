

### 4.1 使用命令行
[参考链接](https://www.cnblogs.com/www-yang-com/p/10427518.html)

#### 4.1.1 创建仓库将本地代码提交到远程

- 第一步：初始化本地版本库

```java
git init
```
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318161512290.png)

- 第二步：将所有代码添加到暂存区

```java
git add .
```
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318161648159.png)

- 第三步：提交到本地版本库

```java
git commit -m '提交信息'
```
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318161810976.png)

- 第四步：添加远程仓库

```java
git remote add origin 仓库地址

```
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318162002672.png)

- 第五步：表示第一次推送需要拉取一下代码（相当于连接到远程仓库 ）

```java
git pull --rebase origin master
```
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318162123552.png)

- 第六步：正式上传至码云中

```java
git push origin master
```
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318162333396.png)

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
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318165323171.png)
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318165411525.png)
#### 4.2.2 初始化仓库(相当于 git init)

```java
1 这个操作相当于git init 
```

![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318170217369.png)
#### 4.2.3 IDEA中Git操作（相当于git add. git commit -m）

```java
1 选中项目-右键
2 选择git 这里面封装了对git所有操作
```

![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318171235228.png)
#### 4.2.4 配置连接远程仓库
配置远程仓库地址（相当于 git remote add origin）
![在这里插入图片描述](01%20git%20%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.assets/20200318171818978.png)
连接一下仓库（第一次需要连接一下仓库）

```java
git pull --rebase origin master
```
### Git 版本控制
#### 1 直接回退到上一个版本
[回退到上一个版本](https://blog.csdn.net/hehyyoulan/article/details/93193562)

[参考链接](https://blog.csdn.net/yhch1024/article/details/81220858)

