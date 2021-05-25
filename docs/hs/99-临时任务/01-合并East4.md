

### 1 TA_ORDER  

- 表：TA_ORDER  
- 字段：F_CHANNELFEE
- 序号：291

```sql
-- 修改前
NVL(TC.F_STAMPTAX,0) +NVL(TC.F_BACKFARE,0) + NVL(TC.F_OTHERFARE1,0)
-- 修改后
NVL(TC.F_TOTALFARE,0)+NVL(TC.F_STAMPTAX,0) +NVL(TC.F_BACKFARE,0) + NVL(TC.F_OTHERFARE1,0)
```

### 2 TA_BENEFICIAL

- 表：TA_BENEFICIAL
- 字段：L_BENEFTYPE
- 字段解释：受益权级别
- 序号：354

```sql
-- 修改前
case when nvl(tfoundinfo.C_STRUCTFLAG,'0')='0' then 9
    else  to_number(nvl(decode(t.c_profittype,'0','0','1','2','2','9'),decode(tfoundinfo.C_STRUCTTYPE,'0','0','1','2',nvl(tfoundinfo.C_STRUCTTYPE,'9'))))
end  L_BENEFTYPE

-- 修改后
case when nvl((select param_value from tsys_parameter where param_code ='STG_BENEFTYPESOURCE'),'0')= '0' and nvl(tfoundinfo.C_STRUCTFLAG,'0')='0' then 9
    else to_number(nvl(decode(t.c_profittype,'0','0','1','2','2','9','3','1'),decode(tfoundinfo.C_STRUCTTYPE,'0','0','1','2',nvl(tfoundinfo.C_STRUCTTYPE,'9'))))
end  L_BENEFTYPE
```

> 全要素总共用到表 ta_beneficial 的两个字段
>
> - l_beneftype  ： 受益权级别
> - c_benefclas  ： 受益级别

### 3 am_collatera

- 表：am_collatera
- 字段：C_COLLTYPE
- 字段解释：押品分类
- 序号：431

```sql
/**************************************************************************************************
                                    修改前的取值
***************************************************************************************************/
--  "1、 C_COLLTYPE  字段取值：
when t.c_asset_type = '1' and t.c_asset_type_ex = '0' then 'A51' --A51:金融质押品_股票/股权/基金_上市公司流通A股股票<-- 资产类型=1股权+资产明细=0流通股 
     when t.c_asset_type = '1' and t.c_asset_type_ex = '1' then 'A53' --A53:金融质押品_股票/股权/基金_上市公司限售流通股 <-- 资产类型=1股权+资产明细=1限售流通股
     when t.c_asset_type = '1' and t.c_asset_type_ex = '2' then 'A55' --A55:金融质押品_股票/股权/基金_非上市公司股权     <-- 资产类型=1股权+资产明细=2非上市公司 
     when t.c_asset_type = '1' and nvl(c_asset_type_ex,'!') not in('0','1','2') then 'A54'  --A54:金融质押品_股票/股权/基金_证券投资基金券<-- 资产类型=1股权+资产明细<>0流通股+1限售流通股+2非上市公司 
     when t.c_asset_type = '2'                           then 'C99' --C99:应收账款_其他_其他        <-- 资产类型=2债权
     when t.c_asset_type = '3' and t.c_asset_type_ex = 'a' then 'D42' --D42:其他押品_交通工具_船舶    <-- 资产类型=3动产+资产明细=a船舶 
     when t.c_asset_type = '3' and t.c_asset_type_ex = 'b' then 'D41' --D41:其他押品_交通工具_车辆    <-- 资产类型=3动产+资产明细=b车辆 
     when t.c_asset_type = '3' and t.c_asset_type_ex = 'c' then 'D32' --D32:其他押品_机器设备_专用设备<-- 资产类型=3动产+资产明细=c设备 
     when t.c_asset_type = '3' and nvl(t.c_asset_type_ex,'!') not in('a','b','c')               then 'D31'  --D31:其他押品_机器设备_通用设备<-- 资产类型=3动产+资产明细<>a船舶+b车辆+c设备(=n其他动产)  
     when t.c_asset_type = '4' and t.c_asset_type_ex = '3' and t.c_houseproperty_type = '1'       then 'B35'  --B35:房地产_居住用房_其他居住房<-- 资产类型=4动产+资产明细=3房产+房产类别=1住宅 
     when t.c_asset_type = '4' and t.c_asset_type_ex = '3' and t.c_houseproperty_type in('2','4') then 'B12'  --B12:房地产_商用房地产_商业用房<-- 资产类型=4动产+资产明细=3房产+房产类别=2商业房地产+4商住房  
     when t.c_asset_type = '4' and t.c_asset_type_ex = '3' and t.c_houseproperty_type = '3'       then 'B13'  --B13:房地产_商用房地产_工业用房<-- 资产类型=4动产+资产明细=3房产+房产类别=3工业房地产 
     when t.c_asset_type = '4' and t.c_asset_type_ex = '3' and t.c_houseproperty_type = '5'       then 'B11'  --B11:房地产_商用房地产_办公用房<-- 资产类型=4动产+资产明细=3房产+房产类别=5办公用房 
     when t.c_asset_type = '4' and t.c_asset_type_ex = '3' and t.c_houseproperty_type = '6'       then 'B51'  --B51:房地产_在建工程类_商用类  <-- 资产类型=4动产+资产明细=3房产+房产类别=6在建工程 
     when t.c_asset_type = '4' and t.c_asset_type_ex = '3' and nvl(t.c_houseproperty_type,'!') not in('1','2','4','3','5','6')    then 'B99'  --B99:房地产_其他_其他 <-- 资产类型=4动产+资产明细=3房产+房产类别=9其他 
     when t.c_asset_type = '7' and upper(t.c_asset_type_ex) = 'O'  then 'A11'  --A11:金融质押品_现金等价物_存单   <-- 资产类型=7有价证券+资产明细=o存单 
     when t.c_asset_type = '7' and upper(t.c_asset_type_ex) = 'P'  then 'A43'  --A43:金融质押品_票据_银行承兑汇票 <-- 资产类型=7有价证券+资产明细=p票据 
     when t.c_asset_type = '7' and upper(t.c_asset_type_ex) = 'Q'  then 'A61'  --A61:金融质押品_保单_人寿保险单   <-- 资产类型=7有价证券+资产明细=q保单 
     when t.c_asset_type = '7' and upper(t.c_asset_type_ex) = 'R'  then 'A31'  --A31:金融质押品_债券_国债         <-- 资产类型=7有价证券+资产明细=r国债 
     when t.c_asset_type = '7' and nvl(upper(t.c_asset_type_ex),'z') not in('O','P','Q','R')  then 'A99'  --A99:金融质押品_其他_存单 <-- 资产类型=7有价证券+资产明细<>o存单+p票据+q保单+r国债(=z其他)
     else 'D99'  --其他押品_其他_其他 "

/**************************************************************************************************
                                    修改后的取值
***************************************************************************************************/
--"1、C_COLLTYPE字段 ：
  CASE
           -- 1股权->0流通股:A51上市公司流通A股股票
           WHEN T.C_ASSET_TYPE = '1' AND T.C_ASSET_TYPE_EX = '0' THEN  'A51'
           -- 1股权->1限售流通股：A53上市公司限售流通股
           WHEN T.C_ASSET_TYPE = '1' AND T.C_ASSET_TYPE_EX = '1' THEN  'A53'
           -- 1股权->2非上市公司:A55股票(权)/基金-非上市公司股权
           WHEN T.C_ASSET_TYPE = '1' AND T.C_ASSET_TYPE_EX = '2' THEN  'A55'
           -- 1股权:A54 证券投资基金券
           WHEN T.C_ASSET_TYPE = '1' THEN               'A54'
           -- 2债权：C99其他
           WHEN T.C_ASSET_TYPE = '2' THEN               'C99'
           ---- 抵押物  ----
           -- 3动产->a船舶:D42船舶
           WHEN T.C_ASSET_TYPE = '3' AND T.C_ASSET_TYPE_EX = 'a' THEN  'D42'
           -- 3动产->b车辆:D41车辆
           WHEN T.C_ASSET_TYPE = '3' AND T.C_ASSET_TYPE_EX = 'b' THEN  'D41'
           -- 3动产->c设备:D32专用设备
           WHEN T.C_ASSET_TYPE = '3' AND T.C_ASSET_TYPE_EX = 'c' THEN  'D32'
           -- 3动产->d飞机:D43机器设备
           WHEN T.C_ASSET_TYPE = '3' AND T.C_ASSET_TYPE_EX = 'd' THEN  'D43'
           -- 3动产->k设施类在建工程:D51机器设备
           WHEN T.C_ASSET_TYPE = '3' AND T.C_ASSET_TYPE_EX = 'k' THEN  'D51'
           -- 3动产:D99其他
           WHEN T.C_ASSET_TYPE = '3' THEN               'D99'
           -- 4不动产->3房产->1居住用房:B35其他居住用房
           WHEN T.C_ASSET_TYPE = '4' AND T.C_ASSET_TYPE_EX = '3' AND   T.C_HOUSEPROPERTY_TYPE = '1' THEN               'B35'
           -- 4不动产->3房产->2商业用房:B12商业用房
           WHEN T.C_ASSET_TYPE = '4' AND T.C_ASSET_TYPE_EX = '3' AND   T.C_HOUSEPROPERTY_TYPE = '2' THEN               'B12'
           -- 4不动产->3房产->3工业用房:B13工业用房
           WHEN T.C_ASSET_TYPE = '4' AND T.C_ASSET_TYPE_EX = '3' AND   T.C_HOUSEPROPERTY_TYPE = '3' THEN               'B13'
           -- 4不动产->3房产->4商住用房:B1商用房地产
           WHEN T.C_ASSET_TYPE = '4' AND T.C_ASSET_TYPE_EX = '3' AND   T.C_HOUSEPROPERTY_TYPE = '4' THEN               'B12'
           -- 4不动产->3房产->5办公用房:B11办公用房
           WHEN T.C_ASSET_TYPE = '4' AND T.C_ASSET_TYPE_EX = '3' AND   T.C_HOUSEPROPERTY_TYPE = '5' THEN               'B11'
           -- 4不动产->3房产->6在建工程:B5房产类在建工程
           WHEN T.C_ASSET_TYPE = '4' AND T.C_ASSET_TYPE_EX = '3' AND   T.C_HOUSEPROPERTY_TYPE = '6' THEN               'B51'
           -- 4不动产->3房产->9其他:B99其他
           WHEN T.C_ASSET_TYPE = '4' AND T.C_ASSET_TYPE_EX = '3' AND   T.C_HOUSEPROPERTY_TYPE = '9' THEN               'B99'
           -- 4不动产->4地产:B24其他用地
           WHEN T.C_ASSET_TYPE = '4' AND T.C_ASSET_TYPE_EX = '4' THEN  'B24'           
     -- 4不动产:B99其他
           WHEN T.C_ASSET_TYPE = '4' AND NVL(T.C_ASSET_TYPE_EX, '!') NOT IN ('3', '4') THEN  'B99'
           -- 7有价证券 ->0存单:A11存单
           WHEN T.C_ASSET_TYPE = '7' AND T.C_ASSET_TYPE_EX = 'o' THEN  'A11'
           -- 7有价证券 ->p票据:A41银行本票
           WHEN T.C_ASSET_TYPE = '7' AND T.C_ASSET_TYPE_EX = 'p' THEN  'A43'
           -- 7有价证券 ->q保单:A61具有现金价值的人寿保险单
           WHEN T.C_ASSET_TYPE = '7' AND T.C_ASSET_TYPE_EX = 'q' THEN  'A61'
           -- 7有价证券 ->r国债:A31国债
           WHEN T.C_ASSET_TYPE = '7' AND T.C_ASSET_TYPE_EX = 'r' THEN  'A31'
           -- 7有价证券 ->z其他:A99其他
           WHEN T.C_ASSET_TYPE = '7' AND NVL(T.C_ASSET_TYPE_EX, 'NULL') NOT IN ('o', 'p', 'q', 'r', '') THEN 'A99'
           --5、6、8、9、E、O、P、b ：D99
           WHEN T.C_ASSET_TYPE IN ('5', '6', '8', '9', 'E', 'O', 'P', 'b') THEN  'D99'
           --A、C、G、H、I、J、Q ：A99
           WHEN T.C_ASSET_TYPE IN ('A', 'C', 'G', 'H', 'I', 'J', 'Q') THEN  'A99'
           --B ：A54
           WHEN T.C_ASSET_TYPE = 'B' THEN  'A54'
           --D ：D6
           WHEN T.C_ASSET_TYPE = 'B' THEN  'D6'
           -- F收费权->t应收账款：C11
           WHEN T.C_ASSET_TYPE = 'F' AND T.C_ASSET_TYPE_EX = 't' THEN 'C11'
           -- F收费权->u其它应收账款：C99
           WHEN T.C_ASSET_TYPE = 'F' AND T.C_ASSET_TYPE_EX = 'u' THEN 'C99'
           -- F收费权->g公路收费权：C21
           WHEN T.C_ASSET_TYPE = 'F' AND T.C_ASSET_TYPE_EX = 'g' THEN 'C21'
           -- F收费权->h农村电网建设与改造工程电费收费：C43
           WHEN T.C_ASSET_TYPE = 'F' AND T.C_ASSET_TYPE_EX = 'h' THEN 'C43'
           -- F收费权->i其他收费权：C4
           WHEN T.C_ASSET_TYPE = 'F' AND T.C_ASSET_TYPE_EX = 'i' THEN 'C4'
           -- F收费权->j应收租金：C51
           WHEN T.C_ASSET_TYPE = 'F' AND T.C_ASSET_TYPE_EX = 'j' THEN 'C51'
           -- K现金及其等价物：A1
           WHEN T.C_ASSET_TYPE = 'K' THEN   'A1'
           -- L贵金属：A2
           WHEN T.C_ASSET_TYPE = 'L' THEN   'A2'
           -- M流动资产：D1
           WHEN T.C_ASSET_TYPE = 'M' THEN   'D1'
           -- N无形资产：D7
           WHEN T.C_ASSET_TYPE = 'N' THEN   'D7'
           -- a应收账款：C99
           WHEN T.C_ASSET_TYPE = 'a' THEN   'C99'
           -- 空:空
           WHEN T.C_ASSET_TYPE IS NULL THEN   NULL
           -- 如果判断不了，默认D99其他
           ELSE               'D99'
           END   C_COLLTYPE ,


```

### 4 pm_projectinfo

#### 1  l_singleset 【单一/集合标志】

```sql
-- 调整前
case  when c_projecttype = '1' or c_projectpropertytype = '1' then 1 
      when c_projecttype is null and c_projectpropertytype is null then null 
      else 2 
end l_singleset

-- 调整后
case when tpi.c_projecttype = '3' then (case when tpi.c_projectpropertytype = '1' then 1 else 2 end)
    else (case when tpi.c_projecttype = '1' then 1 else 2 end)
end l_singleset
```

#### 2 c_specialty【业务特征】



```sql
-- 修改前
(select to_char(wm_concat(distinct decode(tt.column_value,'','','4','1','5','2','6','3','7','4','8','5','9','6','A','98','B','9','E','8','F','10','J','1','99')))
from table (select fn_split((select tpi.c_specialbusintype from dual),',')  from dual t) tt)

-- 修改后
case when tpi.c_two_level in ('1.1','1.3','3.4') then decode(tpi.c_two_level,'1.1','8','1.3','20','3.4','15')
    else 
(select to_char(wm_concat(distinct decode(tt.column_value,'','','3','22','4','1','5','2','6','3','7','4','8','5','9','6','A','98','B','9','E','8','F','10','J','1','99')))
 from table (select fn_split((select tpi.c_specialbusintype from dual),',')  from dual t) tt) end

```

#### 3  c_capuseway 【资金运用方式】

```sql
-- 修改前
(select to_char(wm_concat(decode(tt.column_value ,'0','0','1','1','2','2','3','4','4','5','5','6','6','7','7','8','9','2','10','3','9')))
      from table (select fn_split(tpi.c_fundapplicationway ,',')  from dual t) tt)  c_capuseway

-- 修改后
case when nvl((select param_value from tsys_parameter where param_code ='TSIS_GXHCS'),'0') = 'ZXXT' then
    (select to_char(wm_concat(decode(tt.column_value ,'0','0','1','1','2','2,3','3','4','4','5','5','6','6','7','7','8','9','2','10','3','9')))
        from table (select fn_split(tpi.c_fundapplicationway ,',')  from dual t) tt)
  else 
    (select to_char(wm_concat(decode(tt.column_value ,'0','0','1','1','2','2,3','3','4','4','5','5','6','6','7','7','8','9','2','10','3','9')))
        from table (select fn_split(tpi.c_fundapplicationway ,',')  from dual t) tt)
end     c_capuseway

```

### 5 AM_PACT

#### 1  C_EXITMODE 【退出方式】【@】

> 修改字段-新增字典     10-收购/11非公开上市

需要调整的内容:

- 1 修改注释新增字典。
- 2 调整表结构的长度。