### tdc_taktl

![image-20210330205554376](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210330205554376.png)

#### 1 采集报错kettle 

#### (1) tdc_fa_stockgrade

- 出错原因

  > 无法将 NULL 插入 ("HSTDC"."FA_STOCKGRADE"."D_GRADEDATE"

- 结论

  > 自测库中数据不合法，导致数据报错。

###  2 未映射字段

> 未映射的字段主要有四类：
>
> - 1 stg 缺辅助字段：l_collflag , l_modiflag ,c_modiman 。
> - 2 stg 缺业务字段：后续讨论后再映射。
> - 3 tdc 缺业务字段：后续讨论后再映射。
> - 4 stg 和tdc 字段名称有差异：后续讨论后再映射。

### 3 主版本合并了三个East 4 kettle

- 详情见本 pdf 截图 中新增 标注的kettle 

### 4 修改单

> M202103302735,M202103302739,M202103302741,M202103302744,M202103302742