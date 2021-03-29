###  tdc_amktl_innate

![image-20210329114234911](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329114234911.png)

1 合并tdc_amktl_innate 时候有1 个kettle 采集报错，截图中用 椭圆形标注

- (1) tdc_am_pactrate_innate 。

  > - kettle 报错：无法将null 值插入tdc 的字段。
  > - 出错原因 ：stg 层有不合法数据导致出现。

2 有部分字段未映射上

> 未映射的字段主要有两类
>
> - 辅助字段：l_collflag , l_modiflag ,c_modiman  
> - 业务字段：后续讨论后再加入。

3 合并了两个新的kettle 

- tdc_am_assetInfo_innate
- tdc_am_groupholdstock_innate



