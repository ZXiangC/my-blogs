### 1 问题 kettle 

![image-20210402160741850](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210402160741850.png)

### 2 stg 层视图

- v_ts2pm_projectlevel

```sql
create or replace view v_ts2pm_projectlevel as
select * from v_ts2pm_projectlevelexp 
where l_busiflag = 0 --业务字段:0-正常子项目/1-非三单分期
```

- 关联视图

```sql
v_ts2pm_projectlevelexp
```

- 修改单记录

![image-20210402160947466](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210402160947466.png)

### 3 基础数据文档

![image-20210402161233242](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210402161233242.png)

