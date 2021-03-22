### 一 参考链接

[参考链接一](https://blog.csdn.net/qq_34129336/article/details/71941526?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param)

[员工练习题](https://blog.csdn.net/zhiweianran/article/details/7815289)

[Scott 用户练习博客，里面有三篇写的蛮好](https://blog.csdn.net/evan_treborn/article/details/19080995)

[存储过程](https://blog.csdn.net/luo609630199/article/details/81046589)

[Cast  when 使用总结](https://blog.csdn.net/changxiangyangy/article/details/86718551)

[Over (partition by ...)](https://blog.csdn.net/qq_43563538/article/details/90405306)

### 二 创建表并插入测试数据

- student  学生表

```sql
create table Student(
   S# varchar2(10),
   Sname varchar2(10),
   Sage date,
   Ssex varchar2(10)
);

insert into Student values('01' , '赵雷' , to_date('1990-01-01','yyyy-mm-dd') , '男');
insert into Student values('02' , '钱电' , to_date('1990-12-21','yyyy-mm-dd') , '男');
insert into Student values('03' , '孙风' , to_date('1990-05-20','yyyy-mm-dd') , '男');
insert into Student values('04' , '李云' , to_date('1990-08-06','yyyy-mm-dd') , '男');
insert into Student values('05' , '周梅' , to_date('1991-12-01','yyyy-mm-dd') , '女');
insert into Student values('06' , '吴兰' , to_date('1992-03-01','yyyy-mm-dd') , '女');
insert into Student values('07' , '郑竹' , to_date('1989-07-01','yyyy-mm-dd') , '女');
insert into Student values('08' , '王菊' , to_date('1990-01-20','yyyy-mm-dd') , '女');

```

- course  课程表

```sql
create table Course(
    C# varchar2(10),
    Cname varchar2(10),
    T# varchar2(10)
    );
    
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');
```

- Teacher ：教师表

```sql
create table Teacher(
    T# varchar2(10),
    Tname varchar2(10)
);

insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');

```

- Sc ： 学生选课表

```sql
create table SC(
    S# varchar2(10),
    C# varchar2(10),
    score number(4,1)
);

insert into SC values('01' , '01' , 80);
insert into SC values('01' , '02' , 90);
insert into SC values('01' , '03' , 99);
insert into SC values('02' , '01' , 70);
insert into SC values('02' , '02' , 60);
insert into SC values('02' , '03' , 80);
insert into SC values('03' , '01' , 80);
insert into SC values('03' , '02' , 80);
insert into SC values('03' , '03' , 80);
insert into SC values('04' , '01' , 50);
insert into SC values('04' , '02' , 30);
insert into SC values('04' , '03' , 20);
insert into SC values('05' , '01' , 76);
insert into SC values('05' , '02' , 87);
insert into SC values('06' , '01' , 31);
insert into SC values('06' , '03' , 34);
insert into SC values('07' , '02' , 89);
insert into SC values('07' , '03' , 98);
```

### 例 1

- 查询"01"课程比"02"课程成绩高的学生的信息及课程分数

方式一

```sql
select  
 s.s#,
 s.sname,
 t1.s1_core,
 t2.s2_core
from 
 student s ,
 (select s1.s# s1_no, sc.score s1_core from student s1 ,Sc sc where s1.s#=sc.s# and sc.c#='01') t1 ,
 (select s2.s# s2_no, sc.score s2_core from student s2 ,Sc sc where s2.s#=sc.s# and sc.c#='02') t2
where s.s#=t1.s1_no 
  and s.s#=t2.s2_no
  and t1.s1_core > t2.s2_core;
```

方法二

```sql
select t.s#,t.sname, t1.score, t2.score 
from 
  student t ,
  sc      t1,
  sc      t2
where t.s#=t1.s# 
  and t.s#=t2.s#
  and t1.c#='01'
  and t2.c#='02'
  and t1.score > t2.score;  
```

### 例 2

-  查询"01"课程比"02"课程成绩低的学生的信息及课程分数

```sql

select 
  t.s# no,
  t.sname name,
  t1.score "01分数" ,
  t2.score "02分数" 
from 
 student t,
 (select s1.s# , s1.sname ,sc1.score from student s1 , Sc sc1 where s1.s# = sc1.s# and sc1.c# = '01') t1,
 (select s2.s#,s2.sname, sc2.score from student s2 , Sc sc2 where s2.s# = sc2.s# and sc2.c# = '02') t2
where t.s#=t1.s#
  and t.s#=t2.s#
  and t1.score<t2.score;
  

```

### 例 3

- 查询平均成绩大于等于60分的同学的学生编号和学生姓名和平均成绩

```sql
select 
  s.s#,
  s.sname,
  t1.a
 from student s,
 (select 
   t.s#,
   round(avg(t.score),2) a
  from sc t
   group by t.s# 
   order by t.s#
 ) t1
where s.s#=t1.s# and t1.a>=60
```

### 例 4

- 查询平均成绩小于60分的同学的学生编号和学生姓名和平均成绩

```sql
select 
  s.s#,
  s.sname,
  t1.a
 from student s,
 (select 
   t.s#,
   round(avg(t.score),2) a
  from sc t
   group by t.s# 
   order by t.s#
 ) t1
where s.s#=t1.s# and t1.a>=60
```

### 例 5

- 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩

```sql
-- 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩
select 
  s.s#,
  s.sname,
  t.c_count,
  t.sum_core
from 
 student s ,
 (
    select 
      sc.s# as s_no,
      count(sc.s#)  as  c_count,
      sum(sc.score) as  sum_core
    from sc 
     group by sc.s#
     order by sc.s#
  ) t
where s.s#=t.s_no
```

### 例 6

- 查询"李"姓老师的数量

```sql
-- 查询"李"姓老师的数量
select 
 count(*)
from teacher t
 where t.tname like '李%';
```

### 例 7

```sql
-- 查询学过"张三"老师授课的同学的信息
-- 方式一
select 
 s.s#,
 s.sname
from 
student s
where s.s# in (
 select 
 sc.s#
 from sc
  where sc.c# =(
   select 
   c.c#
   from course c
    where c.t# = (select t.t# from teacher t where t.tname = '张三')
 )
)
-- 方式二 
select *
from student s,course c,teacher t,sc s1
where s.s#=s1.s# and s1.c#=c.c# and c.t#=t.t# and t.tname='张三';

```

### 例 8

```sql
-- 查询没学过"张三"老师授课的同学的信息

select *from student t where t.s# not in (
  select sc.s# from Sc sc,Course c,Teacher t 
    where sc.c#=c.c# 
    and c.t#=t.t# 
    and t.tname='张三'
)
```

### 例 9 

```sql
-- 查询学过编号为"01"并且也学过编号为"02"的课程的同学的信息

-- 方式一
select distinct *from 
 student t
 where 1=1
   and t.s# in (select sc.s# s1 from sc where sc.c#='01')
   and t.s# in (select sc.s# s2 from sc where sc.c#='02')
   order by t.s#;

-- 方式二
select distinct s.*
from student s, sc s1,sc s2
where s.s#=s1.s# and s.s#=s2.s# and s1.c#=01 and s2.c#=02
order by s.s#;   
   
```

### 例 10

```sql
-- 查询学过编号为"01"但是没有学过编号为"02"的课程的同学的信息

-- 方式一
select distinct *from 
 student t
 where 1=1
   and t.s# in     (select sc.s# s1 from sc where sc.c#='01')
   and t.s# not in (select sc.s# s2 from sc where sc.c#='02')
   order by t.s#;
   
-- 方式二
select s.*
 from student s,sc s1
  where s.s#=s1.s# 
  and s1.c#='01' 
  and s.s# not in (select s.s# from student s,sc s1 where s.s#=s1.s# and s1.c#='02');
```

### 例 11 

```sql
 -- 查询没有学全所有课程的同学的信息
  -- 1.查询课程总数
    select count(*) from course
 --  2.查询学生选课总数
    select sc.s#, count(*) sc_num from sc group by sc.s#
 -- 最终版
 select distinct s.* from 
   student s,
   (select s# , count(*) sc_num from sc group by sc.s#) t
   where 1=1
     and s.s#=t.s#
     and t.sc_num<(select count(*) from course)
     order by s.s#

```

### 例 12 

```sql
-- 12、查询至少有2门课与学号为"01"的同学所学相同的同学的信息


select distinct s.* from student s,sc 
 where s.s#=sc.s#
 and sc.c# in(select sc.c# c_no from sc where sc.s#='01')
 and s.s#<> '01'
 order by s.s#;

select distinct s.* from student s,sc 
  where s.s#=sc.s#
  
  group by s.S#
  order by s.s#
-- 01"的同学所学相同的同学的信息完全相同
select Student.* 
  from Student 
  where S# in
  (
  select distinct SC.S# 
  from SC 
     where S# <> '01' 
     and SC.C# in (select distinct C# from SC where S# = '01') 
     group by SC.S# 
     having count(1) = (select count(1) from SC where S#='01')
  );

--------
with aaa as select kecheng from kechengbiao where xuehao = '01'
select xuehao from kechengbiao group by xuehao having exists ((select count(1) from kechengbiao where kecheng in aaa)>=2)

--with aaa as (select c# from sc where S# = '01')
select S# from sc 
 
  where 
 sc.s#<>'01' group by S#
  having count(1) >=2 and (c#  in (select c# from sc where S# = '01'))
  
  

select *from sc for update order by sc.s#;
```

### 例 13

```sql
-- 01"的同学所学相同的同学的信息完全相同
select Student.* 
  from Student 
  where S# in
  (
  select distinct SC.S# 
  from SC 
     where S# <> '01' 
     and SC.C# in (select distinct C# from SC where S# = '01') 
     group by SC.S# 
     having count(1) = (select count(1) from SC where S#='01')
  );
```

### 例 14 

```sql
-- 查询没学过"张三"老师讲授的任一门课程的学生姓名

-- 方式一
select *from student s
where s.s# not in(
  select sc.s# from sc ,Teacher t ,course c
  where sc.c#=c.c#
  and c.t#=t.t#
  and t.tname='张三'
)

-- 方式二
select distinct s.sname
from student s
  where s.s# not in (
    select s# from sc where c# in(
      select c.c# from course c,teacher t 
       where c.t#=t.t# and t.tname='张三'
       )
    );

```

### 例 15 

```sql
select s.s#,s.sname,round(avg(s1.score),2) 平均成绩
 from student s,sc s1
  where s.s#=s1.s# and s1.score<60
  group by s.s#,s.sname
  having count(s.s#)>=2;
```

### 例 16 

```sql
--16、检索"01"课程分数小于60，按分数降序排列的学生信息
select s.s#,s.sname,sc.score from student s,sc 
  where  s.s#=sc.s#
  and  sc.score<60
  and sc.c#= '01'
  order by sc.score desc;
```

### 例 17  -

```sql
--17、按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩

--方式一
select *
 from student s,
 (select s1.s#,round(avg(s1.score),2) 平均成绩 from sc s1 group by s1.s#) s1,
 sc s2
where s.s#=s1.s# and s.s#=s2.s#(+)
order by s1.平均成绩 desc,s2.score desc; 

-- 方式二
select s.s#,s.sname,
  t1.score 第一门课程成绩 ,
  t2.score 第二课程成绩 ,
  t3.score 第三门课程成绩,
  t4.all_avg 平均成绩
 from student s,
(select sc.s#,sc.c#,sc.score  from sc where sc.c#='01' ) t1,
(select sc.s#,sc.c#,sc.score  from sc where sc.c#='02' ) t2,
(select sc.s#,sc.c#,sc.score  from sc where sc.c#='03' ) t3,
(select sc.s#,avg(sc.score) all_avg from sc group by sc.s#) t4
where s.s#=t1.s#(+)
  and s.s#=t2.s#(+)
  and s.s#=t3.s#(+)
  and s.s#=t4.s#(+)
  order by t4.all_avg desc
```

### 例 18 【decimal】

```sql
--18、查询各科成绩最高分、最低分和平均分：以如下形式显示：课程ID，课程name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率
--及格为>=60，中等为：【70-80】，优良为：【80，90】，优秀为：>=90
select 
  m.C# 课程编号, 
  m.Cname 课程名称,  
  max(n.score) 最高分, 
  min(n.score) 最低分, 
  cast(avg(n.score) as decimal(18,2)) 平均分, 
  cast(
      (select count(1) from SC where C# = m.C# and score >= 60)*100.0
       / 
      (select count(1) from SC where C# = m.C#) as decimal(18,2)
   )  及格率,
  cast(
      (select count(1) from SC where C# = m.C# and score >= 70 and score  < 80 )*100.0 
      / 
      (select count(1) from SC where C# = m.C#) as decimal(18,2)
   ) 中等率, 
  cast(
      (select count(1) from SC where C# = m.C# and score >= 80 and score  < 90 )*100.0 
      /
      (select count(1) from SC where C# = m.C#) as decimal(18,2)
  ) 优良率, 
  cast(
   (select count(1) from SC where C# = m.C# and score >= 90)*100.0 
   /
   (select count(1) from SC where C# = m.C#) as decimal(18,2)
  ) 优秀率
from Course m , SC n 
where m.C# = n.C# 
group by m.C# , m.Cname 
order by m.C#; 
```

> cast(  *  as as decimal(18,4))    18 表示总位数为18【可以短，但是不可以长】，  4 表示其中小数有4 位。 这就是一个保留几位小数的函数。

### 例 19 -【排名】

```sql
--19、按各科成绩进行排序，并显示排名
select s#,c#,score,rank() over(partition by c# order by score desc) 排名 from sc;
```

### 例 20 -【排名】

```sql
--20、查询学生的总成绩并进行排名
select 
 s.s# ,
 s.sname,
 sum(nvl(sc.score,0)) 总成绩 ,
 rank() over(order by sum(sc.score) desc) 排名
from sc ,student s
where sc.s#=s.s#
group by s.s#,s.sname

```

> rank() over( order by ...)  给分数进行排名



### 例  21 【表关联】

```sql
--21、查询不同老师所教不同课程平均分从高到低显示
select
  t.t#,
  t.tname,
  cast(avg(sc.score) as decimal(18,2)) 平均分
from 
 sc,
 course c,
 Teacher t
where 1=1
and sc.c#=c.c#
and c.t#=t.t#
group by t.t#,t.tname
order by 平均分 desc
```

### 例 22 【排名】

```sql
--22、查询所有课程的成绩第2名到第3名的学生信息及该课程成绩
select *
from
 (select s#,c#,score,row_number() over(partition by c# order by score desc) 排名 from sc) t
where t.排名 in(2,3);
```

### 例 23  【cast when】

```sql
--23、统计各科成绩各分数段人数：课程编号,课程名称,[100-85],[85-70],[70-60],[0-60]及所占百分比
select Course.C# 课程编号 , Cname as 课程名称 ,
  sum(case when score >= 85 then 1 else 0 end ) "[85-100]",
  sum(case when score >= 70 and score < 85 then 1 else 0 end) "[70-85]",
  sum(case when score >= 60 and score < 70 then 1 else 0 end) "[60-70]",
  sum(case when score < 60 then 1 else 0 end) "[0-60]"
from sc , Course
where SC.C# = Course.C#
group by Course.C# , Course.Cname
order by Course.C#;
```

### 例 24  【排名】

```sql
--24、查询学生平均成绩及其名次
--方法二，用rank 方法排序
select 
 s#,
 round(avg(score),2) 平均成绩,
 rank() over(order by avg(score) desc) 排名 
from sc 
group by s# 
```

### 例 25  【partition by】

```sql
--25、查询各科成绩前三名的记录
select *
from 
  (select s#,c#,score,row_number() over(partition by c# order by score desc) 排名 from sc) t
where t.排名 < 4;
```

> partition by 是什么鬼

### 例 26 

```sql
--26、查询每门课程被选修的学生数
select 
 sc.c#   课程编号,
 count(1) 选修人数
from 
  sc
group by sc.c#
```

### 例 27 

```sql
--27、查询出只有两门课程的全部学生的学号和姓名
-- 方式一 
select s.s#,s.sname from 
 student s,
 (select sc.s# s#,count(1) cou_m from sc group by sc.s#) t
where
  s.s#=t.s#
 and t.cou_m=3  
-- 方式二 
select s1.s#,s.sname, count(s1.c#) 课程数
from student s,sc s1
where s.s#=s1.s#
group by s1.s#,s.sname
having count(s1.c#)=3;
```

### 例 28  【group by 】

```sql
--28、查询男生、女生人数
-- 方式一 
select 
 t1.c1 男生人数,
 t2.c2 女生人数
from 
 (select count(*) c1 from student s1 where s1.ssex='男') t1,
 (select count(*) c2 from student s2 where s2.ssex='女') t2
 
-- 方式二 
select s.ssex 性别,count(1) 人数
from student s
group by s.ssex;
```

### 例 29  

```sql
--29、查询名字中含有"风"字的学生信息
select *from student t where t.sname like '%风%';
```

### 例 30 【group by 】

```sql
--30、查询同名同姓学生名单，并统计同名人数
select Sname 学生姓名, count(*)  人数 
from Student 
group by Sname 
having count(*) >1;
```

###  例 31 

```sql
--31、查询1990年出生的学生名单(注：Student表中Sage列的类型是datetime)
select *
from student s
where to_char(s.sage,'yyyy')=1990;
-- 
select *from student t where to_char(t.sage,'yyyy') = 1990;
```

### 例 32 

```sql
--32、查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列
select s1.c# 课程编号,round(avg(s1.score),2) 平均成绩
 from sc s1
 group by s1.c#
order by 平均成绩 desc,课程编号;
```

###  例 33

```sql
--33、查询平均成绩大于等于85的所有学生的学号、姓名和平均成绩
-- 方式 1
select *from 
  student s,
  (select 
   sc.s# s# ,
   avg(sc.score) 
   from sc 
   group by sc.s# 
   having avg(sc.score)  >= 85
   ) t
where s.s#=t.s#

-- 方式 2
select 
  s.s# 学号,
  s.sname 姓名,
  round(avg(s1.score),2) 平均成绩
from student s,sc s1
 where s.s#=s1.s#
 group by s.sname,s.s#
 having round(avg(s1.score),2)>=85;
```

### 例 34 

```sql
--34、查询课程名称为"数学"，且分数低于60的学生姓名和分数
-- 方式一
select s.*,t.score from
 student s,
 (
  select 
   sc.s#,
   sc.score
  from 
   sc,
   course c
  where 1=1
  and sc.c#= c.c#
  and c.cname='数学'
  and sc.score < 60
 ) t
where s.s#=t.s# 

-- 方式二
select 
  s.sname 姓名,
  c.cname 课程名称,
  s1.score 分数
from 
 student s,
 course c,
 sc s1
where s.s#=s1.s# 
  and s1.c#=c.c# 
  and c.cname='数学' and s1.score<60;

```

### 例 35

```sql
--35、查询所有学生的课程及分数情况；
-- 方式一
select
  distinct
  s.s#,
  s.sname,
  t1.score  语文成绩,
  t2.score  数学成绩,
  t3.score  英语成绩
from
 sc,
 student s,
 (select sc.s#,sc.score from sc where sc.c#='01') t1,
 (select sc.s#,sc.score from sc where sc.c#='02') t2,
 (select sc.s#,sc.score from sc where sc.c#='03') t3
where 1=1
  and sc.s#=s.s#(+)
  and sc.s#=t1.s#(+)
  and sc.s#=t2.s#(+)
  and sc.s#=t3.s#(+)
order by s.s#

-- 方式2
select 
 s.s# 学号,
 s.sname 学生姓名,
 c.cname 课程名称,
 s1.score 课程分数
from 
 student s,
 course c,
 sc s1
where s.s#=s1.s# and c.c#=s1.c#;
```

### 例 36

```sql
--36、查询任何一门课程成绩在70分以上的姓名、课程名称和分数；
select 
  distinct
  s.s#,
  s.sname,
  t.max_score
from 
  sc,
  student s,
  (select sc.s# ,max(sc.score) max_score from sc group by sc.s#) t
where 1=1
  and sc.s#=t.s#
  and sc.s#=s.s#
  and t.max_score>=70
order by s.s#
```

### 例 37

```sql
--37、查询不及格的课程
select distinct sc.c#,sc.score from sc where sc.score<60
```

### 例 38

```sql
--38、查询课程编号01为且课程成绩在80分以上的学生的学号和姓名；

-- 方式一
select 
 s.s#,
 s.sname
from 
 student s,
 (select sc.s#,sc.score from sc where sc.c#=01) t
where 1=1
 and s.s#=t.s#
 and t.score>=80

-- 方式二 
select 
 s.s# 学号,
 s.sname 姓名,
 s1.c# 课程编号,
 s1.score 分数
from 
 student s,
 sc s1
where 
 s.s#=s1.s# 
 and s1.c#=01 
 and s1.score>=80;
 
```

### 例 39 

```sql
--39、求每门课程的学生人数
select
 sc.c#,
 count(1) 选课人数
from 
  sc
group by sc.c# 
```

### 例 40

```sql
--40、查询选修"张三"老师所授课程的学生中，成绩最高的学生信息及其成绩
select 
  s.s#,
  s.sname,
  c.cname,
  t.t#,
  sc.score
from 
 student s,
 sc,
 teacher t,
 course c,
 (
   select
     max(sc.score) max_score
   from 
    sc,
    teacher t,
    course c
   where 1=1
     and sc.c#=c.c#
     and c.t#=t.t#
     and t.tname='张三'
     order by sc.s#
 ) tt
where 1=1
  and sc.s#=s.s#
  and sc.c#=c.c#
  and c.t#=t.t#
  and sc.score=tt.max_score
```

> 使用多行函数的时候，要么使用group by ，要么只查询一个，把查询的结果作为查询条件去关联表中查询。

### 例 41 (**)

```sql
--41、查询不同课程,成绩相同的学生的学生编号、课程编号、学生成绩
-- 方式二
select
 distinct
 sc1.s# ,
 sc1.c#,
 sc1.score
from 
  sc sc1,
  sc sc2
where 
 sc1.c#<>sc2.c#
 and sc1.score=sc2.score

-- 方式一
select s.*
  from sc s,
  (select C# , score from SC group by C#,score having count(1)>1) s1
where s.c#=s1.c# and s.score=s1.score
order by s.s#;
```

### 例 42  【排名】

```sql
--42、查询每门功成绩最好的前两名
select *
from 
 (select 
   s#,
   c#,
   score,
   row_number() over(partition by c# order by score desc) 排名 from sc
 )
where 排名<3;
```

### 例 43

```sql
--43、统计每门课程的学生选修人数（超过5人的课程才统计）。
-- 要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列

select 
 sc.c#,
 count(1) person_num
from 
 sc
where 1=1
 group by sc.c#
 having count(1)>=5
```

### 例 44

```sql
--44、检索至少选修两门课程的学生学号

select 
  sc.s# 学号,
  count(1) 课程数
from 
 sc
group by sc.s#
 having count(1)>=2
 order by sc.s#
```

### 例 45

```sql
--45、查询选修了全部课程的学生信息
-- 1
select 
 sc.s#
from sc  
 group by sc.s#
 having  count(1)=(select count(*) from course)
 order by sc.s#
-- 2 
 select 
  s.*
 from 
 student s,
 (select 
   s1.s# s,
   count(s1.c#) c 
  from 
    sc s1 
   group by s1.s# 
   having count(s1.c#)=3
 ) s1
where s.s#=s1.s;

```

### 例 46

```sql
--46、查询各学生的年龄
select 
  s.s#,
  s.sname,
  to_char(sysdate,'yyyy')-to_char(s.sage,'yyyy') 年龄
from student s;
```

### 例 47 、48 、49 、50 时间日期使用

```sql
--47、查询本周过生日的学生
select s.*
from student s
where to_char(s.sage,'mmdd') 
 between  to_char(trunc(sysdate,'d'),'mmdd') 
     and  to_char(trunc(next_day(sysdate,'星期六')),'mmdd');

--48、查询下周过生日的学生
select s.*
from student s
where 
 to_char(s.sage,'mmdd') 
  between to_char(trunc(next_day(sysdate,'星期日')),'mmdd') 
      and to_char(trunc(next_day(sysdate,'星期六')),'mmdd');

--49、查询本月过生日的学生
select s.*
from student s
where to_char(s.sage,'mmdd') 
 between to_char(trunc(sysdate,'mm'),'mmdd') 
     and to_char(last_day(sysdate),'mmdd');

--50、查询下月过生日的学生
select s.*
from student s
where to_char(s.sage,'mmdd') 
 between to_char(add_months(trunc(sysdate,'mm'),2),'mmdd') 
     and to_char(add_months(last_day(sysdate),2),'mmdd');
```



### Case When 总结

- Case When 作用一 : **等值转换**

```sql
select 
  s.sname,
  (case 
       when s.ssex ='男' then 1
       when s.ssex ='女' then 0
       else null 
  end) as 性别
from 
 student s
```

- Case When 作用二 ： **范围转换**

```sql
--23、统计各科成绩各分数段人数：课程编号,课程名称,[100-85],[85-70],[70-60],[0-60]及所占百分比
select Course.C# 课程编号 , Cname as 课程名称 ,
  sum(case when score >= 85 then 1 else 0 end ) "[85-100]",
  sum(case when score >= 70 and score < 85 then 1 else 0 end) "[70-85]",
  sum(case when score >= 60 and score < 70 then 1 else 0 end) "[60-70]",
  sum(case when score < 60 then 1 else 0 end) "[0-60]"
from sc , Course
where SC.C# = Course.C#
group by Course.C# , Course.Cname
order by Course.C#;
```

- Case When 作用三：**列转行操作**

```sql
-- 转换后
select 
  s.s#  学号,
  s.sname 姓名,
  max(case when sc.c#='01' then sc.score else 0 end )  语文,
  max(case when sc.c#='02' then sc.score else 0 end )  数学,
  max(case when sc.c#='03' then sc.score else 0 end )  英语
from 
 sc,
 student s
where 1=1 
  and sc.s#=s.s#
  group by s.s#,s.sname
  order by s.s#
 
-- 转换前
select 
 s.s#     学号,
 s.sname  姓名,
 c.cname  课程号,
 sc.score 分数 
from 
  sc,
  student s,
  course c
where s.s#=sc.s#
  and sc.c#=c.c#

```



- 转换前

  ![](03%20sql%20%E8%AF%AD%E5%8F%A5%E7%BB%83%E4%B9%A0.assets/20200924142903956.png)

- 装换后

![](03%20sql%20%E8%AF%AD%E5%8F%A5%E7%BB%83%E4%B9%A0.assets/20200924142928540.png)

### left join 、 Right join 、 Inner Join

[参考链接一](https://blog.csdn.net/zhiweianran/article/details/7843574)

- （+） 操作 [参考链接](https://blog.csdn.net/Ruoyang666/article/details/79129000)