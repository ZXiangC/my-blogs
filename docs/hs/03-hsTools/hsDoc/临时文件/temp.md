### 系统函数 sys_guid() ;
```sql
 SELECT *
 FROM TTF_RPT_RESULTDTL T 
WHERE 1=1 
 -- AND T.C_PROJCODE = TP.C_PROJCODE
 -- AND TP.C_PROMANAGEFLG <> '12'  
  AND T.D_DATE = DATE '2020-09-30'


-- 查看这几个字段为啥为不能参与计算。  
SELECT 
    to_number(replace(to_char(F_OTHERFEERATE),'%','')),
    to_number(replace(to_char(F_COLLRATE),'%','')),
    to_number(replace(to_char(F_CFCOSTS),'%',''))
FROM 
    TTF_RPT_RESULTDTL  T
WHERE 1=1   
  AND T.D_DATE = DATE '2020-09-30'    

-- 查看消费金融项目编号
select 
 pm.c_projcode
from ttf_rpt_resultdtl t ,HSTDC.Pm_Projectinfo pm
where t.c_prodcode = pm.c_projcode
  and pm.c_promanageflg = '12'
  and t.d_date = date '2020-09-30'
-- 查看消费金融是否汇总
select 
  t.c_projcode,
  t.*
from xfjr_test t  
where exists(
  select 1 from (
    select 
      pm.c_projcode
    from ttf_rpt_resultdtl t ,HSTDC.Pm_Projectinfo pm
    where t.c_prodcode = pm.c_projcode
      and pm.c_promanageflg = '12'
      and t.d_date = date '2020-09-30'
   ) t1
   where t.c_projcode = t1.c_projcode
)
order by t.c_projcode


select 
   pm.c_projcode,
   count(t.c_rivalid)
from ttf_rpt_resultdtl t ,HSTDC.Pm_Projectinfo pm
where t.c_prodcode = pm.c_projcode
  and pm.c_promanageflg = '12'
  and t.d_date = date '2020-09-30'
  group by pm.c_projcode
  
  
  
create table xfjr_temp as 
  select 
    t.*
   from ttf_rpt_resultdtl t ,hstdc.pm_projectinfo tp
  where 1=1 
    and t.c_projcode = tp.c_projcode
    and tp.c_promanageflg = '12' 
    and t.d_date = date '2020-09-30' 
    and t.l_unit =1000
    --and t.
select *from xfjr_temp;

select t.c_projcode,t.d_date,t.l_unit, t.* From ttf_rpt_resultdtl t  order by t.c_projcode;  
select 
---- 第一步
execute immediate 'truncate table xfjr_temp';
insert into xfjr_temp  
  select 
    t.*
   from ttf_rpt_resultdtl t ,hstdc.pm_projectinfo tp
  where 1=1 
    and t.c_projcode = tp.c_projcode
    and tp.c_promanageflg = '12' 
    and t.d_date = date '2020-09-30' ;
    
select t.c_prodcode,t.d_date,t.l_unit from xfjr_temp t order by t.c_projcode; -- 696

-- 第二步  -- 删除条数696
delete from  ttf_rpt_resultdtl t  where t.c_projcode in (select c_projcode from  xfjr_temp) and t.d_date = date '2020-09-30';
commit;


-- 第三步 : 数据汇总
execute immediate 'truncate table xfjr_hz';
insert into xfjr_hz(  C_DTLID,C_RPTID,D_DATE,C_SPLITLOGIC,C_ISSUBPROJECT,L_UNIT,C_DEPTNAME,C_ACCOUNTANT,C_DEALMANAGE,
                      C_PRODCODE,C_PROJCODE,C_SUBPROJCODE,C_MAINFUNDCODE,C_FUNDID,C_PROJECTCODEALIAS,C_OUTCONTRAST,L_PROJKIND,L_FUNCTYPE,L_STRUCTFLAG,L_ISTGCOOP,L_INDUSTRY,C_SPECIALBUSINTYPE,L_ISCMADD,
                      F_ZCYE,F_SSXT_FA,F_SSXT_TA,F_ZCYE_WET,F_SSXT_FA_WET,F_SSXT_TA_WET,F_TRUSTPAY,L_EXISTDAYS,F_DAYAVGSCALE,L_FLAG,C_PACTID,C_RIVALID,C_BUSICODE,L_RIVALTYPE,
                      L_INSEQ,L_MAXINSEQ,F_WEIGHT,F_PACTBAL,C_RINDUSTRYDTL,C_PINDUSTRYDTL,C_USEOFAREACODE,
                      L_SEQNO,C_PROJNAME,F_SSXT_JH,F_SSXT_DY,F_SSXT_CC,F_ZCYE_TZ,F_ZCYE_RZ,F_ZCYE_SW,C_BDATE,C_EDATE,L_PERIOD,C_RUNMODE,
                      F_FS_BANKBS,F_FS_BANKFC,F_FS_BANKARO,F_FS_BANKARN,F_FS_STOCKBS,C_FS_BANKFC_FINAL,F_FS_STOCKFC,F_FS_STOCKAR,C_FS_STOCKFC_FINAL,
                      F_FS_FUNDBS,F_FS_FUNDFC,F_FS_FUNDAR,C_FS_FUNDFC_FINAL,F_FS_INSUREBS,F_FS_INSUREFC,F_FS_INSUREAR,C_FS_INSUREFC_FINAL,
                      F_FS_OFIBS,F_FS_OFIFC,F_FS_OFIAR,C_FS_OFIFC_FINAL,F_FS_NOFIOC,F_FS_NOFINC,F_FS_SELFMKOC,F_FS_SELFMKNC,
                      F_PS_BANKAR,F_PS_OFIAR,F_PS_SELFMK,F_PS_OTHER,F_SECBENF_TOTAL,L_SECBENF_NPQ,F_SECBENF_NPM,L_SECBENF_FIQ,F_SECBENF_FIM,L_SECBENF_OIQ,F_SECBENF_OIM,
                      C_RIVALNAME,F_PACTBAL_N,F_PACTBAL_0,C_LISTEDTYPE,C_PLATFORMTYPE,C_GOVNAME,C_GOVLEVLE,C_ISGOVHOLDING,C_ENTSCALETYPE,C_RINDUSTRYDTL_1,F_CFCOSTS,
                      F_YYFS_DK,F_YYFS_JYX,F_YYFS_KGCSCYDQ,F_YYFS_CQGQTZ,F_YYFS_RZZL,F_YYFS_MRFS,F_YYFS_CFTY,F_YYFS_QT,C_PINDUSTRYDTL_1,C_PINDUSTRYDTL_B,C_AREANAME_KEY,
                      F_XMQK_XZ_PTQYRZ,F_XMQK_XZ_JTJCSS,F_XMQK_XZ_SZJCSS,F_XMQK_XZ_SLJNCJCSS,F_XMQK_XZ_HJBFSS,F_XMQK_XZ_SHSY,F_XMQK_XZ_YQ,F_XMQK_XZ_TDCBZX,F_XMQK_XZ_BZXAJGC,F_XMQK_XZ_QT,
                      F_XMQK_FXZFDC_ZZ,F_XMQK_FXZFDC_SY,F_XMQK_FXZFDC_JSF,F_XMQK_FXZFDC_TDCB,F_XMQK_FXZFDC_QT,F_XMQK_KCNY,F_XMQK_QT,
                      F_YJHKLY_JYXXJL,F_YJHKLY_FDCXMXSSR,F_YJHKLY_XTZCZRHCSBX,F_YJHKLY_TDCRSR,F_YJHKLY_ZXFYFH,F_YJHKLY_ZXSSFH,F_YJHKLY_BOT,F_YJHKLY_CZDD,F_YJHKLY_QT,
                      C_RISKMEASURE,C_RISKMEASURE_NAME,F_COLLRATE,C_EXPYIELDRATE,C_TRUSTPAYRATE,C_MANAGETYPE,F_OTHERFEE,F_OTHERFEERATE,
                      C_ISQDII,C_ISPWFT,C_ISANNUITY,C_ISABS,C_TMANAGER,C_CMANAGER,C_OWERSIGHT,C_USEOFAREA_NAME,
                      L_ISCASH,F_CASH_NOSTD_ASSETBAL,C_CASH_NOSTD_LIQPLAN,
                      L_ISSIT,C_SIT_STRUCTFLAG,C_SIT_ISNEWADD,F_SIT_TX_GPSZ,F_SIT_TX_ZQSZ,F_SIT_TX_QTYE,L_SIT_ISSUNPRIVATE,L_SIT_ISUMBERLLA,L_SIT_UMBERLLA_NUM,C_SIT_ISEXTWAREHOUSE,
                      F_SIT_AS_TOTAL,F_SIT_AS_BANKBS,F_SIT_AS_BANKFC,F_SIT_AS_OFIBS,F_SIT_AS_OFIFC,F_SIT_AS_AGENT,F_SIT_AS_SELFMKOC,F_SIT_AS_SELFMKNC,
                      L_SIT_DEP_ISHAP,L_SIT_DEP_NPNUM,F_SIT_DEP_NPAMT,L_SIT_DEP_FINUM,F_SIT_DEP_FIAMT,L_SIT_DEP_OTNUM,F_SIT_DEP_OTAMT,
                      L_SIT_FCO_ISHAP,L_SIT_FCO_NPNUM,F_SIT_FCO_NPSCA,F_SIT_FCO_NPAMT,F_SIT_FCO_NPBAL,L_SIT_FCO_FINUM,F_SIT_FCO_FISCA,F_SIT_FCO_FIAMT,F_SIT_FCO_FIBAL,
                      L_SIT_FCO_OTNUM,F_SIT_FCO_OTSCA,F_SIT_FCO_OTAMT,F_SIT_FCO_OTBAL,F_SIT_TX_GPX,F_SIT_TX_ZQX,F_SIT_TX_XJ,F_SIT_TX_QT,L_SIT_TX_ISGPX,L_SIT_TX_ISZQX,
                      L_SUBMITFLAG,L_CHECKFLAG,D_DATADATE,C_CUSTNAME ) 
            select
              MAX(T.C_DTLID) C_DTLID , --
              MAX(T.C_RPTID) C_RPTID , --
              MAX(T.D_DATE) D_DATE, --
              MAX(T.C_SPLITLOGIC) C_SPLITLOGIC , --
              MAX(T.C_ISSUBPROJECT) C_ISSUBPROJECT , --
              MAX(T.L_UNIT) L_UNIT , --单位【1元/10000万】
              MAX(T.C_DEPTNAME) C_DEPTNAME , --
              MAX(T.C_ACCOUNTANT) C_ACCOUNTANT , --
              MAX(T.C_DEALMANAGE) C_DEALMANAGE , --
              MAX(T.C_PRODCODE) C_PRODCODE , --
              MAX(T.C_PROJCODE) C_PROJCODE , --
              MAX(T.C_SUBPROJCODE) C_SUBPROJCODE , --
              MAX(T.C_MAINFUNDCODE) C_MAINFUNDCODE , --
              MAX(T.C_FUNDID) C_FUNDID , --
              MAX(T.C_PROJECTCODEALIAS) C_PROJECTCODEALIAS , --
              MAX(T.C_OUTCONTRAST) C_OUTCONTRAST , --
              MAX(T.L_PROJKIND) L_PROJKIND , --项目类型: 1单一资金/2集合资金/3财产及财产权
              MAX(T.L_FUNCTYPE) L_FUNCTYPE , --功能分类: 1融资类/2投资类/3事务管理类
              MAX(T.L_STRUCTFLAG) L_STRUCTFLAG , --是否结构化【业务系统标准: 1-是/0-否】
              MAX(T.L_ISTGCOOP) L_ISTGCOOP , --是否信政合作【业务系统标准: 1-是/0-否】
              MAX(T.L_INDUSTRY) L_INDUSTRY , --投向行业【1基础产业/2房地产/3证券/4金融机构/5工商企业/9其他】
              MAX(T.C_SPECIALBUSINTYPE) C_SPECIALBUSINTYPE , --
              MAX(T.L_ISCMADD) L_ISCMADD , --是否当月新增【业务系统标准: 1-是/0-否】
              SUM(T.F_ZCYE) F_ZCYE , --
              SUM(T.F_SSXT_FA) F_SSXT_FA , --
              SUM(T.F_SSXT_TA) F_SSXT_TA , --
              SUM(T.F_ZCYE_WET) F_ZCYE_WET , --
              SUM(T.F_SSXT_FA_WET) F_SSXT_FA_WET , --
              SUM(T.F_SSXT_TA_WET) F_SSXT_TA_WET , --
              SUM(T.F_TRUSTPAY) F_TRUSTPAY , --
              MAX(T.L_EXISTDAYS) L_EXISTDAYS , --存续天数：单位天
              SUM(T.F_DAYAVGSCALE) F_DAYAVGSCALE , --
              MAX(T.L_FLAG) L_FLAG , --标志【0:正常/-1:FA存在而AM不存在/
              MAX(T.C_PACTID) C_PACTID , --
              MAX(T.C_RIVALID) C_RIVALID , --
              MAX(T.C_BUSICODE) C_BUSICODE , --
              MAX(T.L_RIVALTYPE) L_RIVALTYPE , --对手类型【1个人/2机构】
              MAX(T.L_INSEQ) L_INSEQ , --内部序号【同一项目下合同/对手序号】
              MAX(T.L_MAXINSEQ) L_MAXINSEQ , --最大内部序号【同一项目下合同/对手序号的最大值，用于倒减处理】
              SUM(T.F_WEIGHT) F_WEIGHT , --
              SUM(T.F_PACTBAL) F_PACTBAL , --
              MAX(T.C_RINDUSTRYDTL) C_RINDUSTRYDTL , --
              MAX(T.C_PINDUSTRYDTL) C_PINDUSTRYDTL , --
              MAX(T.C_USEOFAREACODE) C_USEOFAREACODE , --
              MAX(T.L_SEQNO) L_SEQNO , --记录序号【IF 序号展示方式
              MAX(T.C_PROJNAME) C_PROJNAME , --
              SUM(T.F_SSXT_JH) F_SSXT_JH , --
              SUM(T.F_SSXT_DY) F_SSXT_DY , --
              SUM(T.F_SSXT_CC) F_SSXT_CC , --
              SUM(T.F_ZCYE_TZ) F_ZCYE_TZ , --
              SUM(T.F_ZCYE_RZ) F_ZCYE_RZ , --
              SUM(T.F_ZCYE_SW) F_ZCYE_SW , --
              MIN(T.C_BDATE) C_BDATE , --  开始日期取最小
              MAX(T.C_EDATE) C_EDATE , --  结束日期取最大
              MAX(T.L_PERIOD) L_PERIOD , --
              MAX(T.C_RUNMODE) C_RUNMODE , --
              SUM(T.F_FS_BANKBS) F_FS_BANKBS , --
              SUM(T.F_FS_BANKFC) F_FS_BANKFC , --
              SUM(T.F_FS_BANKARO) F_FS_BANKARO , --
              SUM(T.F_FS_BANKARN) F_FS_BANKARN , --
              SUM(T.F_FS_STOCKBS) F_FS_STOCKBS , --
              MAX(T.C_FS_BANKFC_FINAL) C_FS_BANKFC_FINAL , --
              SUM(T.F_FS_STOCKFC) F_FS_STOCKFC , --
              SUM(T.F_FS_STOCKAR) F_FS_STOCKAR , --
              MAX(T.C_FS_STOCKFC_FINAL) C_FS_STOCKFC_FINAL , --
              SUM(T.F_FS_FUNDBS) F_FS_FUNDBS , --
              SUM(T.F_FS_FUNDFC) F_FS_FUNDFC , --
              SUM(T.F_FS_FUNDAR) F_FS_FUNDAR , --
              MAX(T.C_FS_FUNDFC_FINAL) C_FS_FUNDFC_FINAL , --
              SUM(T.F_FS_INSUREBS) F_FS_INSUREBS , --
              SUM(T.F_FS_INSUREFC) F_FS_INSUREFC , --
              SUM(T.F_FS_INSUREAR) F_FS_INSUREAR , --
              MAX(T.C_FS_INSUREFC_FINAL) C_FS_INSUREFC_FINAL , --
              SUM(T.F_FS_OFIBS) F_FS_OFIBS , --
              SUM(T.F_FS_OFIFC) F_FS_OFIFC , --
              SUM(T.F_FS_OFIAR) F_FS_OFIAR , --
              MAX(T.C_FS_OFIFC_FINAL) C_FS_OFIFC_FINAL , --
              SUM(T.F_FS_NOFIOC) F_FS_NOFIOC , --
              SUM(T.F_FS_NOFINC) F_FS_NOFINC , --
              SUM(T.F_FS_SELFMKOC) F_FS_SELFMKOC , --
              SUM(T.F_FS_SELFMKNC) F_FS_SELFMKNC , --
              SUM(T.F_PS_BANKAR) F_PS_BANKAR , --
              SUM(T.F_PS_OFIAR) F_PS_OFIAR , --
              SUM(T.F_PS_SELFMK) F_PS_SELFMK , --
              SUM(T.F_PS_OTHER) F_PS_OTHER , --
              SUM(T.F_SECBENF_TOTAL) F_SECBENF_TOTAL , --
              MAX(T.L_SECBENF_NPQ) L_SECBENF_NPQ , --劣后受益人_自然人个数
              SUM(T.F_SECBENF_NPM) F_SECBENF_NPM , --
              MAX(T.L_SECBENF_FIQ) L_SECBENF_FIQ , --劣后受益人_金融机构个数
              SUM(T.F_SECBENF_FIM) F_SECBENF_FIM , --
              MAX(T.L_SECBENF_OIQ) L_SECBENF_OIQ , --劣后受益人_普通机构个数
              SUM(T.F_SECBENF_OIM) F_SECBENF_OIM , --
              '自然人' C_RIVALNAME , --
              SUM(T.F_PACTBAL_N) F_PACTBAL_N , --
              SUM(T.F_PACTBAL_0) F_PACTBAL_0, --
              MAX(T.C_LISTEDTYPE) C_LISTEDTYPE , --
              MAX(T.C_PLATFORMTYPE) C_PLATFORMTYPE , --
              MAX(T.C_GOVNAME) C_GOVNAME , --
              MAX(T.C_GOVLEVLE) C_GOVLEVLE , --
              MAX(T.C_ISGOVHOLDING) C_ISGOVHOLDING , --
              MAX(T.C_ENTSCALETYPE) C_ENTSCALETYPE , --
              MAX(T.C_RINDUSTRYDTL_1) C_RINDUSTRYDTL_1, --
              SUM(T.F_CFCOSTS) F_CFCOSTS , --
              SUM(T.F_YYFS_DK) F_YYFS_DK , --
              SUM(T.F_YYFS_JYX) F_YYFS_JYX , --
              SUM(T.F_YYFS_KGCSCYDQ) F_YYFS_KGCSCYDQ , --
              SUM(T.F_YYFS_CQGQTZ) F_YYFS_CQGQTZ , --
              SUM(T.F_YYFS_RZZL) F_YYFS_RZZL , --
              SUM(T.F_YYFS_MRFS) F_YYFS_MRFS , --
              SUM(T.F_YYFS_CFTY) F_YYFS_CFTY , --
              SUM(T.F_YYFS_QT) F_YYFS_QT , --
              MAX(T.C_PINDUSTRYDTL_1) C_PINDUSTRYDTL_1, --
              MAX(T.C_PINDUSTRYDTL_B) C_PINDUSTRYDTL_B , --
              MAX(T.C_AREANAME_KEY) C_AREANAME_KEY , --
              SUM(T.F_XMQK_XZ_PTQYRZ) F_XMQK_XZ_PTQYRZ , --
              SUM(T.F_XMQK_XZ_JTJCSS) F_XMQK_XZ_JTJCSS , --
              SUM(T.F_XMQK_XZ_SZJCSS) F_XMQK_XZ_SZJCSS , --
              SUM(T.F_XMQK_XZ_SLJNCJCSS) F_XMQK_XZ_SLJNCJCSS , --
              SUM(T.F_XMQK_XZ_HJBFSS) F_XMQK_XZ_HJBFSS , --
              SUM(T.F_XMQK_XZ_SHSY) F_XMQK_XZ_SHSY , --
              SUM(T.F_XMQK_XZ_YQ) F_XMQK_XZ_YQ , --
              SUM(T.F_XMQK_XZ_TDCBZX) F_XMQK_XZ_TDCBZX , --
              SUM(T.F_XMQK_XZ_BZXAJGC) F_XMQK_XZ_BZXAJGC , --
              SUM(T.F_XMQK_XZ_QT) F_XMQK_XZ_QT , --
              SUM(T.F_XMQK_FXZFDC_ZZ) F_XMQK_FXZFDC_ZZ , --
              SUM(T.F_XMQK_FXZFDC_SY) F_XMQK_FXZFDC_SY , --
              SUM(T.F_XMQK_FXZFDC_JSF) F_XMQK_FXZFDC_JSF , --
              SUM(T.F_XMQK_FXZFDC_TDCB) F_XMQK_FXZFDC_TDCB , --
              SUM(T.F_XMQK_FXZFDC_QT) F_XMQK_FXZFDC_QT , --
              SUM(T.F_XMQK_KCNY) F_XMQK_KCNY , --
              SUM(T.F_XMQK_QT) F_XMQK_QT , --
              SUM(T.F_YJHKLY_JYXXJL) F_YJHKLY_JYXXJL , --
              SUM(T.F_YJHKLY_FDCXMXSSR) F_YJHKLY_FDCXMXSSR , --
              SUM(T.F_YJHKLY_XTZCZRHCSBX) F_YJHKLY_XTZCZRHCSBX , --
              SUM(T.F_YJHKLY_TDCRSR) F_YJHKLY_TDCRSR , --
              SUM(T.F_YJHKLY_ZXFYFH) F_YJHKLY_ZXFYFH , --
              SUM(T.F_YJHKLY_ZXSSFH) F_YJHKLY_ZXSSFH , --
              SUM(T.F_YJHKLY_BOT) F_YJHKLY_BOT , --
              SUM(T.F_YJHKLY_CZDD) F_YJHKLY_CZDD , --
              SUM(T.F_YJHKLY_QT) F_YJHKLY_QT , --
              MAX(T.C_RISKMEASURE) C_RISKMEASURE , --
              MAX(T.C_RISKMEASURE_NAME) C_RISKMEASURE_NAME , --
              SUM(T.F_COLLRATE) F_COLLRATE , --
              MAX(T.C_EXPYIELDRATE) C_EXPYIELDRATE , --
              MAX(T.C_TRUSTPAYRATE) C_TRUSTPAYRATE , --
              MAX(T.C_MANAGETYPE) C_MANAGETYPE , --
              SUM(T.F_OTHERFEE) F_OTHERFEE , --
              SUM(T.F_OTHERFEERATE) F_OTHERFEERATE , --
              MAX(T.C_ISQDII) C_ISQDII , --
              MAX(T.C_ISPWFT) C_ISPWFT , --
              MAX(T.C_ISANNUITY) C_ISANNUITY , --
              MAX(T.C_ISABS) C_ISABS , --
              MAX(T.C_TMANAGER) C_TMANAGER , --
              MAX(T.C_CMANAGER) C_CMANAGER , --
              MAX(T.C_OWERSIGHT) C_OWERSIGHT , --
              MAX(T.C_USEOFAREA_NAME) C_USEOFAREA_NAME , --
              MAX(T.L_ISCASH) L_ISCASH , --现金管理类_是否现金类【全要素标准：0是/1否】
              SUM(T.F_CASH_NOSTD_ASSETBAL) F_CASH_NOSTD_ASSETBAL , --
              MAX(T.C_CASH_NOSTD_LIQPLAN) C_CASH_NOSTD_LIQPLAN , --
              MAX(T.L_ISSIT) L_ISSIT , --证券投资类_是否证券投资类信托【全要素标准：0是/1否】
              MAX(T.C_SIT_STRUCTFLAG) C_SIT_STRUCTFLAG , --
              MAX(T.C_SIT_ISNEWADD) C_SIT_ISNEWADD , --
              SUM(T.F_SIT_TX_GPSZ) F_SIT_TX_GPSZ , --
              SUM(T.F_SIT_TX_ZQSZ) F_SIT_TX_ZQSZ , --
              SUM(T.F_SIT_TX_QTYE) F_SIT_TX_QTYE , --
              MAX(T.L_SIT_ISSUNPRIVATE) L_SIT_ISSUNPRIVATE , --证券投资类_是否为管理型证券投资信托(阳光私募)【全要素标准：0是/1否】
              MAX(T.L_SIT_ISUMBERLLA) L_SIT_ISUMBERLLA , --证券投资类_是否为伞形证券投资信托项目【全要素标准：0是/1否】
              MAX(T.L_SIT_UMBERLLA_NUM) L_SIT_UMBERLLA_NUM , --证券投资类_伞形投资信托项目子单元个数
              MAX(T.C_SIT_ISEXTWAREHOUSE) C_SIT_ISEXTWAREHOUSE , --
              SUM(T.F_SIT_AS_TOTAL) F_SIT_AS_TOTAL , --
              SUM(T.F_SIT_AS_BANKBS) F_SIT_AS_BANKBS , --
              SUM(T.F_SIT_AS_BANKFC) F_SIT_AS_BANKFC , --
              SUM(T.F_SIT_AS_OFIBS) F_SIT_AS_OFIBS , --
              SUM(T.F_SIT_AS_OFIFC) F_SIT_AS_OFIFC , --
              SUM(T.F_SIT_AS_AGENT) F_SIT_AS_AGENT , --
              SUM(T.F_SIT_AS_SELFMKOC) F_SIT_AS_SELFMKOC , --
              SUM(T.F_SIT_AS_SELFMKNC) F_SIT_AS_SELFMKNC , --
              MAX(T.L_SIT_DEP_ISHAP) L_SIT_DEP_ISHAP , --证券投资类_劣后受益人追加保证金_是否发生【全要素标准：0是/1否】
              MAX(T.L_SIT_DEP_NPNUM) L_SIT_DEP_NPNUM , --证券投资类_劣后受益人追加保证金_自然人个数
              SUM(T.F_SIT_DEP_NPAMT) F_SIT_DEP_NPAMT , --
              MAX(T.L_SIT_DEP_FINUM) L_SIT_DEP_FINUM , --证券投资类_劣后受益人追加保证金_金融机构个数
              SUM(T.F_SIT_DEP_FIAMT) F_SIT_DEP_FIAMT , --
              MAX(T.L_SIT_DEP_OTNUM) L_SIT_DEP_OTNUM , --证券投资类_劣后受益人追加保证金_其他机构个数
              SUM(T.F_SIT_DEP_OTAMT) F_SIT_DEP_OTAMT , --
              MAX(T.L_SIT_FCO_ISHAP) L_SIT_FCO_ISHAP , --证券投资类_劣后受益人强制平仓_是否发生【全要素标准：0是/1否】
              MAX(T.L_SIT_FCO_NPNUM) L_SIT_FCO_NPNUM , --证券投资类_劣后受益人强制平仓_自然人个数
              SUM(T.F_SIT_FCO_NPSCA) F_SIT_FCO_NPSCA , --
              SUM(T.F_SIT_FCO_NPAMT) F_SIT_FCO_NPAMT , --
              SUM(T.F_SIT_FCO_NPBAL) F_SIT_FCO_NPBAL , --
              MAX(T.L_SIT_FCO_FINUM) L_SIT_FCO_FINUM , --证券投资类_劣后受益人强制平仓_金融机构个数
              SUM(T.F_SIT_FCO_FISCA) F_SIT_FCO_FISCA , --
              SUM(T.F_SIT_FCO_FIAMT) F_SIT_FCO_FIAMT , --
              SUM(T.F_SIT_FCO_FIBAL) F_SIT_FCO_FIBAL , --
              MAX(T.L_SIT_FCO_OTNUM) L_SIT_FCO_OTNUM , --证券投资类_劣后受益人强制平仓_其他机构个数
              SUM(T.F_SIT_FCO_OTSCA) F_SIT_FCO_OTSCA , --
              SUM(T.F_SIT_FCO_OTAMT) F_SIT_FCO_OTAMT , --
              SUM(T.F_SIT_FCO_OTBAL) F_SIT_FCO_OTBAL , --
              SUM(T.F_SIT_TX_GPX) F_SIT_TX_GPX , --
              SUM(T.F_SIT_TX_ZQX) F_SIT_TX_ZQX , --
              SUM(T.F_SIT_TX_XJ) F_SIT_TX_XJ , --
              SUM(T.F_SIT_TX_QT) F_SIT_TX_QT , --
              MAX(T.L_SIT_TX_ISGPX) L_SIT_TX_ISGPX , --证券投资类_投资结构和性质_是否为偏股型信托产品
              MAX(T.L_SIT_TX_ISZQX) L_SIT_TX_ISZQX , --证券投资类_投资结构和性质_是否为偏债型信托产品
              MIN(T.L_SUBMITFLAG) L_SUBMITFLAG , --报送标志【0未报送/1已报送】
              MAX(T.L_CHECKFLAG) L_CHECKFLAG , --校验标志【-1校验失败/0未校验/1校验通过】
              MAX(T.D_DATADATE) D_DATADATE, --
              null C_CUSTNAME --          
            from xfjr_temp t
           where 1=1
             and t.l_unit = 10000
             group by t.c_projcode;


select *from xfjr_hz;
select *from xfjr_temp;

-- 第四步: 向result 表中插入汇总后数据
insert into ttf_rpt_resultdtl  select *from xfjr_hz;
commit;
```
> oracle8i以后提供了一个生成不重复的数据的一个函数sys_guid()一共32位，生成的依据主要是时间和机器码，具有世界唯一性，类似于java中的UUID（都是世界唯一的）



- 一 个数取值需要确认
  - 1 取汇总值sum
  - 2 取平均值
  - 3 null

劣后受益人_自然人个数

证券投资类_劣后受益人强制平仓_自然人个数

证券投资类_劣后受益人强制平仓_金融机构个数

证券投资类_劣后受益人强制平仓_其他机构个数

- 二  存续天数

存续天数：单位天

- 三 是否类，标志类

  - 1 取 max 

  - 2 取默认值

  - 3 null

    现在代码中逻辑

​              MAX(T.L_PROJKIND) L_PROJKIND , --项目类型: 1单一资金/2集合资金/3财产及财产权
​              MAX(T.L_FUNCTYPE) L_FUNCTYPE , --功能分类: 1融资类/2投资类/3事务管理类
​              MAX(T.L_STRUCTFLAG) L_STRUCTFLAG , --是否结构化【业务系统标准: 1-是/0-否】
​              MAX(T.L_ISTGCOOP) L_ISTGCOOP , --是否信政合作【业务系统标准: 1-是/0-否】
​              MAX(T.L_INDUSTRY) L_INDUSTRY , --投向行业【1基础产业/2房地产/3证券/4金融机构/5工商企业/9其他】
​              MAX(T.C_SPECIALBUSINTYPE) C_SPECIALBUSINTYPE , --
​              MAX(T.L_ISCMADD) L_ISCMADD , --是否当月新增【业务系统标准: 1-是/0-否】

- 四 成本类

  - 取平均值
  - 最大值
  - null

   avg(to_number(replace(to_char(F_CFCOSTS),'%',''))) F_CFCOSTS, --融资成本【年化】

  avg(to_number(replace(to_char(F_COLLRATE),'%',''))) F_COLLRATE,-- 抵质押率

  avg(to_number(replace(to_char(F_OTHERFEERATE),'%',''))) F_OTHERFEERATE, --其他费用率

​      