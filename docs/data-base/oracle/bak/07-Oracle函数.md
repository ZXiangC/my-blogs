### Trunc() 函数

```sql
/**************日期********************/
1.select trunc(sysdate) from dual        --2013-01-06 今天的日期为2013-01-06
2.select trunc(sysdate, 'mm') from dual  --2013-01-01 返回当月第一天.
3.select trunc(sysdate,'yy') from dual   --2013-01-01 返回当年第一天
4.select trunc(sysdate,'dd') from dual   --2013-01-06 返回当前年月日
5.select trunc(sysdate,'yyyy') from dual --2013-01-01 返回当年第一天
6.select trunc(sysdate,'d') from dual    --2013-01-06 (星期天)返回当前星期的第一天
7.select trunc(sysdate, 'hh') from dual  --2013-01-06 17:00:00 当前时间为17:35 
8.select trunc(sysdate, 'mi') from dual  --2013-01-06 17:35:00 TRUNC()函数没有秒的精确
/***************数字********************/
/*
TRUNC（number,num_digits） 
 Number 需要截尾取整的数字。 
 Num_digits 用于指定取整精度的数字。Num_digits 的默认值为 0。
*/
9.select trunc(123.458) from dual      --123
10.select trunc(123.458,0) from dual   --123
11.select trunc(123.458,1) from dual   --123.4
12.select trunc(123.458,-1) from dual  --120
13.select trunc(123.458,-4) from dual  --0
14.select trunc(123.458,4) from dual   --123.458
15.select trunc(123) from dual         --123
16.select trunc(123,1) from dual       --123
17.select trunc(123,-1) from dual      --120
```

> 注意事项：TRUNC()函数截取时不进行四舍五入

