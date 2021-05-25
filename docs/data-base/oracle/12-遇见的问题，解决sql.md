### 1 查询表中主键相同的记录

```sql
-- 
select t.c_projectcode
  from hsstg.ts_tcmp_tproject_collect t
 group by t.c_projectcode
having count(*) > 1
```

