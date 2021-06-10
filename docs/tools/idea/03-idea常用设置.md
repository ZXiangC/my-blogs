## 一、编辑设置



### 1、添加 Ctrl+Shift +W 关闭全部

- 1 设置关闭关闭当前页

  > settings——>keymap——>main menu——>window——>editor tabs——>close右键添加即可。

## 二 写代码设置

### 1、生成 serialVersionUID

- 1.正常使用Idea编码过程中，java类实现了Serializable接口后，并未提醒生成UID,也没有快捷入口

 

![img](https://img2020.cnblogs.com/blog/978388/202004/978388-20200410132856355-1437819112.png)

 

 

 

- 2.Idea 打开setting-->Inspections-->输入搜索：

```java
Serializable class without 'serialVersionUID'
```

默认是未勾选的：

![img](https://gitee.com/ZXiangC/picture/raw/master/img/978388-20200410133053580-1879097044.png)



- 将这一项勾选上，应用后保存

![img](https://gitee.com/ZXiangC/picture/raw/master/img/978388-20200410133505255-632019408.png)

- 3.然后将光标放置在 Serializable接口的 实现类名上

![img](https://gitee.com/ZXiangC/picture/raw/master/img/978388-20200410133546214-158795283.png)

 

 

 

此时，直接 Alt+Enter 即可自动生成 serialVersionUID

 ![img](https://img2020.cnblogs.com/blog/978388/202004/978388-20200410134107978-2058165622.png)

 

##  三、IDEA设置

### 1、取消自动更新IDEA

**设置不自动检测更新**   File->setting->Apper&&Be->SystemSetting->Update->取消勾选

