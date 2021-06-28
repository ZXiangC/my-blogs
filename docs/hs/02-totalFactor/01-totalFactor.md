### 1 全要素全部过程

```java
SP_RPT_CBRC_TPROJDCFMONITOR
SP_RPT_CBRC_TPROJFULLFACTORCHE
sp_rpt_cbrc_tProjFullFactorCol
SP_RPT_CBRC_TPROJFULLFACTORQRY
SP_RPT_TTFCOL_HSAM
SP_RPT_TTFCOL_HSFA
SP_RPT_TTFCOL_HSTA
SP_RPT_TTFCOL_MULT
SP_RPT_TTFCOL_TCMP
SP_RPT_TTFGETRESULT
SP_TTF_COLRISKSOURCE
```

### 全要素整体分析

```sql
-- 17365
select t.c_projcode, t.*from hstdc.pm_projectinfo t where t.c_fullname like '%伯睿1号股权投资单一资金信托%';

-- 资金端

-- 一 产品  GM00BV
select *from hstdc.ta_fundinfo t where t.c_projcode = '17365';


select *From hstdc.ta_benefproof t;
select *from hstdc.ta_transfer t where t.c_fundcode = 'GM00BV';
select *from hstdc.ta_fundagency t;

-- 1.1 可以查产品的受益权 GM00BV_0001
select t.c_brightid, t.* from hstdc.ta_beneficial t where t.c_fundcode = 'GM00BV';

-- 1.2 可以查询该产品所有的认购合同  88239
select
 t.c_pactid,
 t.c_bailorid,
 t.c_benefid,
 t.* from hstdc.ta_pact t 
where 1=1 
  and t.c_fundcode = 'GM00BV';

-- 1.2.1 认购合同里面可以查委托人，收益人信息
select 
 t.c_benefid,
 t.*
from 
  hstdc.ta_pact t,
  hstdc.cm_client t1
where 1= 1
  and t.c_benefid = t1.c_clientid;
 
-- 1.2.2 认购合同流水
select 
 t.c_busiflag, -- 业务标识
 t.l_funddir,  -- 资金方向 
 t.f_tshares,  -- 份额
 t.L_FUNDDIR * NVL(t.F_TSHARES, 0) ,-- 带方向的份额
 t.*from hstdc.ta_order t 
where 1=1 
  and t.c_pactid = '88239';
  
  
select *from hstdc.ta_prescale t where t.c_fundcode = 'GM00BV'  ;
select *From hstdc.ta_prescale_exp t ;
```

