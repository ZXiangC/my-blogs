## Git工作流程
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318153241264.png)
## Git安装
[git官网](https://git-scm.com/download)
安装后默认位置（IDEA中配置git有用）
```java
C:\Program Files\Git\cmd\git.exe
```

![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318153546184.png)
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/2020031815353630.png)

```java
安装：
 1 一直点击下一步即可，都使用默认路径
```
## 三 ssh公钥和私钥
### 3.1 本地使用命令生成公钥和私钥
使用GitBash 输入如下命令
```java
ssh-keygen -t rsa
```
本机公钥和私钥生成位置

```java
C:\Users\祥子\.ssh
```
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/2020031815441277.png)
### 3.2 码云上面ssh配置（github类似）
设置->安全设置-ssh公钥
第一步：
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318154724512.png)
第二步：
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318154758272.png)
第三步：
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318155020180.png)

##  四 Git使用
### 4.1 使用命令行（将本地代码上传到码云）
[参考链接](https://www.cnblogs.com/www-yang-com/p/10427518.html)
第一步：初始化本地版本库

```java
git init
```
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318161512290.png)
第二步：将所有代码添加到暂存区

```java
git add .
```
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318161648159.png)
第三步：提交到本地版本库

```java
git commit -m '提交信息'
```
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318161810976.png)
第四步：添加远程仓库

```java
git remote add origin 仓库地址

```
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318162002672.png)
第五步：表示第一次推送需要拉取一下代码（相当于连接到远程仓库 ）

```java
git pull --rebase origin master
```
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318162123552.png)
第六步：正式上传至码云中

```java
git push origin master
```
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318162333396.png)

若上传有问题，可以试试   

```java
git push origin master -f 表示舍弃线上的文件，强制推送
```
### 4.2 使用IDEA（将本地代码上传到码云）
#### IDEA配置git 

Settings->Versions Control->Git
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318165323171.png)
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318165411525.png)
#### 初始化仓库(相当于 git init)

```java
1 这个操作相当于git init 
```

![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318170217369.png)
#### IDEA中Git操作（相当于git add. git commit -m）

```java
1 选中项目-右键
2 选择git 这里面封装了对git所有操作
```

![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318171235228.png)
#### 配置连接远程仓库
配置远程仓库地址（相当于 git remote add origin）
![在这里插入图片描述](01-git%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318171818978.png)
连接一下仓库（第一次需要连接一下仓库）
```java
git pull --rebase origin master
```
### Git 版本控制
#### 1 直接回退到上一个版本
[回退到上一个版本](https://blog.csdn.net/hehyyoulan/article/details/93193562)

### 五 tortoiseGit 使用

[tortoiseGit 上传代码到github](https://www.cnblogs.com/hjvsdr/p/7152141.html)



## Git 错误解决方法
### 错误1 
[Push rejected: Push to origin/master was rejected](https://blog.csdn.net/lzf1759891062/article/details/82222011)
### 错误2
[XXX项目 is registered as a Git root, but no Git repositories were found there](https://blog.csdn.net/yhch1024/article/details/81220858)