#### 问题 1 ：项目数量不一致

- 新

  ```sql
  select TP.C_INTRUSTFLAG, -- *1
         TP.C_IS1104，  --是否1104报送 【值为 1】
  from HSSTG.TPROJECT_INFO TP where tp.c_projectcode = '16035';
  ```

- 旧

  ```sqL
  select TP.C_INTRUSTFLAG, -- *1
         TP.C_IS1104，  --是否1104报送 【值为 0】
  from HSSTG.TPROJECT_INFO TP where tp.c_projectcode = '16035';
  ```

  所以导致新库中比旧库中多了一条数据。
  
  #### 问题 2  信政项目，分类错误。
  
  ```sql
  select t.c_platformtype, t.*from trpt_tprojfullfactor_hsam t where t.c_prodcode = 'G041' and t.d_date = date '2020-12-31'; 
  select t.f_xmqk_xz_hjbfss, t.*from trpt_tprojfullfactor_hsam t where t.c_prodcode = 'G041' and t.d_date = date '2020-12-31'; 
  
  select F_XMQK_XZ_HJBFSS from ttf_rpt_resultdtl t  where t.c_prodcode = 'G041' and t.d_date = date '2020-12-31' and t.l_unit = 10000; 
  
  ```
  
  

### AM 合同

```sql
-- 比较临时表
select count(*) from TEMP_TTF_AMDTL t where t.d_date = date '2021-01-31'; -- 511
select count(*) from TEMP_TTF_AMDTL@TTF_OLD t where t.d_date = date '2021-01-31';--509

select 
  t.c_pactid,
	t1.c_pactid
from 
  TEMP_TTF_AMDTL t full join TEMP_TTF_AMDTL@TTF_OLD t1 on t.c_pactid = t1.c_pactid
where 1 = 1
  and ( t.c_pactid is null or t1.c_pactid is null);


/*
 旧库中有，但是新库里面没有
 QS717008, projcode :   H379
 DK11804O  -- projcode: IB96  fundcode: FZ0D0X
*/
select *from hstdc.am_pact t where t.c_pactid = 'QS717008';

-- 中间表
select t.* from TEMP_TTF_AMDTL t where t.c_pactid = 'QS717008'; -- null

-- 合同表
select t.* from hstdc.am_pact t where t.c_pactid = 'QS717008';  -- not null

-- 运用流水
select O.C_PACTID C_STOCK_CODE,
         SUM(NVL(O.F_DLVCAPITAL, 0) * (O.L_STOCKDIR)) F_PACTBAL
    from HSTDC.AM_ORDER O
   WHERE O.L_STOCKDIR <> '3' --排除资产方向为 3-置数
     AND O.C_STATUS IN ('8', '9')
     AND O.D_DELIVERY <= date '2020-01-31'
   GROUP BY O.C_PACTID
  HAVING 
  o.c_Pactid = 'QS717008'
	and SUM(NVL(O.F_DLVCAPITAL, 0) * (O.L_STOCKDIR)) > 0

--------- 新：流水明细
select t.F_DLVCAPITAL,t.l_stockdir ,t.l_funddir,t.c_businame, t.* from hstdc.am_order t where t.c_pactid = 'QS717008'order by t.d_date desc; --确实为　０
select t.F_DLVCAPITAL,t.l_stockdir ,t.l_funddir, t.c_businame,t.* from hstdc.am_order t where t.c_pactid = 'DK11804O' order by t.d_date desc; --确实为　０

-------- 旧：流水明细
SELECT C_STOCK_CODE, SUM(F_UNTRANSFERED_INVEST) F_PACTBAL
    FROM TI_HSAM_TRADBPOSN@TTF_OLD
   WHERE L_TRADE_DATE =
         (SELECT MAX(L_TRADE_DATE)
            FROM TI_HSAM_TRADBPOSN@TTF_OLD
           WHERE L_TRADE_DATE <=
                 TO_NUMBER(TO_CHAR(date '2020-01-31', 'YYYYMMDD')))
   GROUP BY C_STOCK_CODE
  HAVING C_STOCK_CODE = 'QS717008'
	and SUM(F_UNTRANSFERED_INVEST) > 0
;
select t.C_STOCK_CODE ,t.F_UNTRANSFERED_INVEST,t.L_TRADE_DATE,t.* from TI_HSAM_TRADBPOSN@TTF_OLD t where t.C_STOCK_CODE = 'QS717008'order by t.L_TRADE_DATE desc;
select t.C_STOCK_CODE ,t.F_UNTRANSFERED_INVEST,t.L_TRADE_DATE,t.* from TI_HSAM_TRADBPOSN@TTF_OLD t where t.C_STOCK_CODE = 'DK11804O'order by t.L_TRADE_DATE desc;

-- Result 
select *from ttf_rpt_resultdtl t where t.c_pactid = 'QS717008' and t.d_date =date '2020-01-31' ; -- QS 这个合同在旧库中也没有　排除
select *from ttf_rpt_resultdtl@TTF_OLD t where t.c_pactid = 'DK11804O'and t.d_date =date '2020-01-31' ;-- DK11804O 这个合同需要分析原因



/*
新库中有，但是旧库里面没有
QT921001　projcode: IB96  fundcode: FZ0D0X
ZQD2001C  projcode: K693  fundcode: FZ0KVL
ZQD2001E  projcode: K160  fundcode: FZ0IVA
ZQD21002  projcode: K254  fundcode: FZ0J54
*/


--比较临时表
-- 比较中间表
select count(*) from trpt_tprojfullfactor_hsam t where t.d_date = date '2021-01-31'; -- 
select count(*) from trpt_tprojfullfactor_hsam@TTF_OLD t where t.d_date = date '2021-01-31'; -- 604


-- Result 
select 

-- trpt_ttfcol_hsam 数据量

-- 参数　V_TPFSTATMAY　　new:１ old: 1   全要素信保基金统计方式参数[TTF_TPFSTATMAY]:  0-按AM对手合同统计/1-按无交易对手统计(默认)
SELECT * FROM TSYS_PARAMETER WHERE UPPER(PARAM_CODE) = 'TTF_TPFSTATMAY';
SELECT * FROM TSYS_PARAMETER@TTF_OLD WHERE UPPER(PARAM_CODE) = 'TTF_TPFSTATMAY';

-- V_TPFRIVALID : #### 
SELECT PARAM_VALUE  FROM TSYS_PARAMETER WHERE   UPPER(PARAM_CODE) = 'TTF_TPFRIVALID';
SELECT PARAM_VALUE  FROM TSYS_PARAMETER@TTF_OLD WHERE   UPPER(PARAM_CODE) = 'TTF_TPFRIVALID';


--  参数：V_PRTSTATWAY　０　全要素财产信托统计方式参数[TTF_PRTSTATWAY]:  0-按AM对手合同统计/1-按无交易对手统计(默认)
SELECT　PARAM_VALUE  FROM TSYS_PARAMETER WHERE UPPER(PARAM_CODE) = 'TTF_PRTSTATWAY'; 

-- 参数：V_PROMANAGEFLG  0  小贷类统计方式参数[TTF_PROMANAGEFLG]
SELECT PARAM_VALUE FROM TSYS_PARAMETER WHERE UPPER(PARAM_CODE) = 'TTF_PROMANAGEFLG';
SELECT PARAM_VALUE  FROM TSYS_PARAMETER@ttf_old WHERE UPPER(PARAM_CODE) = 'TTF_PROMANAGEFLG';

-- 分拆逻辑　０２
SELECT * FROM TSYS_PARAMETER WHERE UPPER(PARAM_CODE) = 'TTF_SPLITLOGIC' for update;
SELECT * FROM TSYS_PARAMETER@ttf_old WHERE UPPER(PARAM_CODE) = 'TTF_SPLITLOGIC';


-- new
select 
 count(*)
from 
  TEMP_TTF_AMDTL P,HSTDC.CM_RIVAL R
   WHERE P.C_RIVALID = TO_CHAR(R.C_RIVALID) -- -- 502
	 AND NVL(P.C_RIVALID,'!') <> '####' 
  -- AND ( V_PRTSTATWAY = '0' OR ( V_PRTSTATWAY = '1' AND NVL(P.L_PROJKIND,0) <> 3 ) )
  -- AND ( V_PROMANAGEFLG = '0' OR ( V_PROMANAGEFLG = '1' AND nvl(P.C_PROMANAGEFLG,0) <> 8 ) );; 

-- old
select 
count(*)
FROM TEMP_TTF_AMDTL@TTF_OLD P,TI_HSAM_RIVAL@TTF_OLD R,hsstg.tproject_info@TTF_OLD A
		WHERE P.C_RIVALID = TO_CHAR(R.L_RIVAL_ID)
	AND P.C_PRODCODE=A.C_PROJECTCODE -- 500
```

