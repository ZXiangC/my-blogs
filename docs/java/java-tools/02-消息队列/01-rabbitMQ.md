##  安装RabbitMQ
[Docker中安装RabbitMQ](https://blog.csdn.net/weixin_44212308/article/details/104100211)

##  RabbitMQ的使用
### 操作界面
**核心功能就是交换机和队列**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220165914954.png)

##  一、队列基本操作
###    1、显示所有队列

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220170554525.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)

### 2 、添加队列

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220174903249.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)

### 3 、删除队列（要点进去找）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220175022951.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)

##  二、交换器基本操作
### 1、显示交换器

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220195525166.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)

### 2、增加交换器

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220195903494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)

### 3、删除交换器

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220200025282.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122020044681.png)

###  4、给交换器绑定队列

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220200247304.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)

### 5、交换机 三种模式

1 直连模式特征 （direct）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220200950564.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)
2 分列模式特征(fanout)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191220200956697.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)
3 主题模式特征(toptic)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122020101265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDIxMjMwOA==,size_16,color_FFFFFF,t_70)