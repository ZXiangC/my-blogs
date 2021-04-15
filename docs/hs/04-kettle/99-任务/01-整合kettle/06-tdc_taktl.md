### tdc_taktl

![image-20210330163529871](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210330163529871.png)



#### 1 采集报错kettle 

#### (1) tdc_cm_client

- 出错原因

  > 1 数据库值不合法。
  >
  > 2 主键冲突。

```sql
insert into CM_CLIENT 
select 
c_clientid            ,
c_idcard              , -- 出错   值太大 文档【32】 实际 【41】
null c_zip            , -- 出错  值太大  文档【6】  实际 【7】
null c_fax            , -- 出错 值太大   文档【20】 实际 【40】
null  c_legzidnumber  , -- 出错 值太大   文档【32】 实际【38】
null l_affltype       , -- 类型不匹配        
FROM hsstg.V_TS2TA_CLIENT t where t.c_clientid <> '000000408102' ;

-- 且有 两条 c_clientid = 000000408102 的记录导致主键冲突
select t.c_clientid  from hsstg.V_TS2TA_CLIENT t group by t.c_clientid having count(*) > 1;
```

- 结论

  > 自测库中数据不合法，导致数据报错。

#### (2) tdc_ta_beneficiaryowner

- 出错原因

>  "HSTDC"."TA_BENEFICIARYOWNER"."C_CUSTNAME" 的值太大 (实际值: 200, 最大值: 160)

- 自测库中数据

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



> M202103302549,M202103302548,M202103302545,M202103302535