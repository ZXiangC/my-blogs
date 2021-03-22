## 一 需要操作表的字段解释

### tamnr_checklog

```sql
insert into tamnr_checklog
  (l_logid,  -- 编号
   l_month,  -- 月份
   c_chkbatch, -- 校验批次
   l_ruleid, -- 校验规则编号
   c_tablepk,
   c_errormsg,-- 错误信息
   d_logdate, -- 时间
   c_remark -- 描述
   )
values
  (437,
   202008,
   '1',
   99016,
   null,
   '【资产收益权，合计（760000，g）=总计，总计（b00000，h）】校验失败',
   to_date('22-08-2020 10:12:46', 'dd-mm-yyyy hh24:mi:ss'),
   null);

```

###  tamnr_rule

```sql
insert into tamnr_rule
  (l_ruleid,      -- 编号id
   c_chklevel,    -- 校验级别【1】
   c_chktype,     -- 校验类型【0】
   c_ruledesc,    -- 规则描述
   c_chkmode,     -- 校验模式【0】
   c_chktable,    -- 校验表名
   c_chkfield,    -- 校验字段
   c_chksql,      -- 校验脚本
   c_wheresql,    -- 【null】
   l_isenable,    -- 是否启用【1】
   l_isdef,       -- 是否自定义【0】
   l_order,       -- 排序【null】
   c_version,     -- 版本【null】
   c_errorcode,   -- 错误码【null】
   c_remark       -- 备注【null】
  )
values
  (12003,
   '1',
   '0',
   '【总计（200000）=公开募集（231000）+非公开募集（232000）】校验',
   '0',
   'tamnr_raisepaybala',
   'f_balance',
   'select '''' c_tablepk,'''' c_chkfield,''0'' l_chkresult,''【总计（200000）=公开募集（231000）+非公开募集（232000）】校验失败'' c_errormsg from (select sum(f_resident+f_gengov+f_nonfinent+f_fininst+f_specific+f_oversea)-sum(f_public+f_nonpublic) as balance from tamnr_raisepaybala ) where balance <> 0',
   null,
   1,
   0,
   null,
   null,
   null,
   null);

```

### tamnr_messagetype

```sql
insert into tgybdss_messagetype
  ( c_msgtype     varchar2(32) not null, -- 报文类型
    c_msgtypename varchar2(64) not null, -- 类型名称
    c_content     varchar2(255),         -- 报文内容
    l_order       number(2),             -- 排序
    c_freq        varchar2(1),           -- 报送频度【7116：R-实时/H-小时/D-天/W-周/M-月】
    l_interval    number(4),             -- 报送间隔
    c_tempcode    varchar2(64),          -- 模板名称
    c_msgtable    varchar2(32))
values
  ('1-3',
   '表1-3',
   '资管产品只数情况统计模板',
   3,
   'm',
   1,
   'amnr_queryprodamount',
   'tamnr_prodamount');
```

