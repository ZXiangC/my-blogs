

```sql

-- 1. 查询excel 名称
select 
  CONCAT(t.c_tempcode,'-',t.c_content),
t.*
from gybdss_messagetype t where t.c_content like '存量债券投资信息%';

-- 2. 查询excel 模板字段
select 
t.c_field  ,
CONCAT(t.c_order-1,'.',t.c_field_name),
t.c_field_length
from gybdss_pojo t 
where 1=1 
  and t.c_templatecode = '2620301'
  and t.c_field <> 'C_JRJGDM';
```

