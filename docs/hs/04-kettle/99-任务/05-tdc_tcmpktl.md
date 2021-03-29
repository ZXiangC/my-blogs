### tdc_tcmpktl

![image-20210329172655797](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329172655797.png)



#### 1 采集报错kettle 

#### (1) tdc_cm_idinfo

- 出错原因

> HSTDC"."CM_IDINFO"."C_IDCARD" 的值太大 (实际值: 41, 最大值: 33)

- 自测库中数据

![image-20210329201210311](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329201210311.png)

- 结论

> 自测库中数据不合法，导致数据报错。

#### (2) tdc_pm_projectcol

- 出错原因

> 违反唯一约束条件 (HSTDC.PK_PM_PROJECTCOL)

- 自测库中数据

![image-20210329201700786](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329201700786.png)

- 结论

> 自测库中数据不合法，导致数据报错。

#### (3) tdc_pm_account

- 出错原因

  > 违反唯一约束条件 (HSTDC.PK_PM_ACCOUNT)

- 自测库中数据

![image-20210329202308988](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329202308988.png)

- 结论

  > 自测库中数据不合法，导致数据报错。

#### (4) tdc_cm_thirdorg

- 出错原因

  > CM_THIRDORG"."C_ADVISERNAME" 的值太大 (实际值: 200, 最大值: 60)

- 自测库中数据

  ![image-20210329202841468](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329202841468.png)

- 结论

> 自测库中数据不合法，导致数据报错。

###  2 未映射字段

> 未映射的字段主要有四类：
>
> - 1 stg 缺辅助字段：l_collflag , l_modiflag ,c_modiman 。
> - 2 stg 缺业务字段：后续讨论后再映射。
> - 3 tdc 缺业务字段：后续讨论后再映射。
> - 4 stg 和tdc 字段名称有差异：后续讨论后再映射。

### 3 主版本合并了两个East 4 kettle

- 详情见本 pdf 截图 中新增 标注的kettle 