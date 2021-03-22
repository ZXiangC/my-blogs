### Git工作流程

![在这里插入图片描述](01%20Git%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318153241264.png)

### Git安装

[git官网](https://git-scm.com/download)
安装后默认位置（IDEA中配置git有用）

```java
C:\Program Files\Git\cmd\git.exe
```

![在这里插入图片描述](01%20Git%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318153546184.png)
![在这里插入图片描述](01%20Git%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/2020031815353630.png)

```java
安装：
 1 一直点击下一步即可，都使用默认路径
```

### 三 ssh公钥和私钥

#### 3.1 本地使用命令生成公钥和私钥

使用GitBash 输入如下命令

```java
ssh-keygen -t rsa
```

本机公钥和私钥生成位置

```java
C:\Users\祥子\.ssh
```

![在这里插入图片描述](01%20Git%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/2020031815441277.png)

#### 3.2 码云上面ssh配置（github类似）

设置->安全设置-ssh公钥
第一步：
![在这里插入图片描述](01%20Git%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318154724512.png)
第二步：
![在这里插入图片描述](01%20Git%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318154758272.png)
第三步：
![在这里插入图片描述](01%20Git%20%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA.assets/20200318155020180.png)

