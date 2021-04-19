### tdc_am_ktl

![image-20210329163226765](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329163226765.png)

#### 1 采集报错kettle 

##### (1) tdc_cm_rival

> 出错原因：HSTDC"."CM_RIVAL"."C_EXTYPE" 的值太大 (实际值: 14, 最大值: 3)

- 自测库中数据

![image-20210329161317303](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329161317303.png)

> 结论： 测试库中数据不合法。导致kettle 报错。

##### (2) tdc_cm_rivalholder

> 出错原因：HSTDC"."CM_RIVALHOLDER"."C_SHOLDERFLAG" 的值太大 (实际值: 2, 最大值: 1)

- 1 自测库中数据

![image-20210329162059855](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329162059855.png)

- 2 基础数据库文档定义

![image-20210329162217340](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329162217340.png)

> 结论： 测试库中数据不合法。导致kettle 报错。

##### (3) tdc_am_assetbackedsecurity

> 出错原因： 无法将 NULL 插入 ("HSTDC"."AM_ASSETBACKEDSECURITY"."C_FUNDCODE")

c_fundCode 为主键。数据不合法导致采集报错。

###  2 未映射字段

> 未映射的字段主要有两类
>
> - 辅助字段：l_collflag , l_modiflag ,c_modiman  
> - 业务字段：后续讨论后再加入。

### 3 主版本合并了五个East 4 kettle

- 详情见本 pdf 截图 中新增 标注的kettle 。

### 4 修改单编号

> M202103291923 ,M202103301318 ,M202103301323 ,M202103301324

​	

