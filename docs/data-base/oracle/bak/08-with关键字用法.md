

### with 关键字初认识

```sql
insert into person_chen (
  pid ,
  name 
  ) with chen as(select t.pid,t.name from person t)
  select 
     chen.pid,
     chen.name from chen;
```

