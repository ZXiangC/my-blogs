## 1 表操作

### 1.1 表创建

- 创建方式一: 直接创建

```sql
create table person(
    pid number(20),
    name varchar2(10)
)
```

- 创建方式二: 利用现有表创建新表

```sql
create table chen_test as select *from chen_tusp_conver;
```

### 1.2 表修改

```sql
-- 增加一个属性
alter table person add(sex varchar2(40)); -- 写法一
alter table person add sex varchar2(40);  -- 写法二

-- 修改属性
alter table person modify(pid varchar2(40));-- 写法一
alter table person modify sex varchar2(50); -- 写法二

-- 删除一个属性
alter table person drop column sex;
```

### 1.3 表删除

- (1) 删除表中所有记录

```sql
delete from person
```

- (2) 删除表结构

```sql
drop table person
```

- (3) 先删除表再创建表

```sql
truncate table person
```

> 效果等同于删除表中数据。（这种方式会先删除索引，然后再删除表中数据，在有索引并且数据量大的时候效率提高）

## 2 函数

### 2.1 序列

- (1) 创建、查询、删除序列

```sql
-- 创建序列
create sequence seq_chen_tusp_conver
increment by 1
start with 1
maxvalue 999999999;
-- 查询序列
select seq_chen_tusp_conver.nextval from sys.dual;
-- 删除序列
DROP SEQUENCE seq_chen_tusp_conver;
```

>序列常用方法:(1)nextval :取得序列的下一个内容   (2)currval :取得序列的当前内容 

- 2 案例

```sql
insert into person values(s_person.nextval,'zhaoyun', 1, to_date('1999-12-22', 'yyyy-MM-dd'));
insert into person values(s_person.nextval,'zhangfei', 1, to_date('1999-12-22', 'yyyy-MM-dd'));
```

- 3 springDataJpa 配置oracle数据库主键自增

```java
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "mseq")
@SequenceGenerator(name = "mseq", sequenceName = "seq_chen_tusp_conver", allocationSize = 1)
private Integer l_id; // 主键
```

> SpringDataJpa 不支持oracle IDENTITY（自增）策略。 要实现自增只能靠序列

```java
SpringDataJpa 主键生成策略:
    TABLE, //使用一个额外的数据库表来保存主键
    SEQUENCE,//使用序列的方式，且其底层数据库要支持序列，一般有postgres、Oracle等
    IDENTITY,//主键由数据库生成，一般为自增型主键，支持的有MySql和Sql Server
    AUTO//由程序来决定主键规则
```

### 2.2 字符串函数

#### substr() 字符串截取函数

- 语法：

```sql
 语法：SUBSTR(string，start， [length])

 string：表示源字符串，即要截取的字符串。
 start：开始位置，从1开始查找。如果start是负数，则从string字符串末尾开始算起。
 length：可选项，表示截取字符串长度。
```

- 案例：

```sql
SELECT SUBSTR('Hello SQL!', 1) FROM dual     --截取所有字符串，返回'Hello SQL!'
SELECT SUBSTR('Hello SQL!', 2) FROM dual     --从第2个字符开始，截取到末尾。返回'ello SQL!'
SELECT SUBSTR('Hello SQL!', -4) FROM dual    --从倒数第4个字符开始，截取到末尾。返回'SQL!'
SELECT SUBSTR('Hello SQL!', 3, 6) FROM dual  --从第3个字符开始，截取6个字符。返回'llo SQ'
SELECT SUBSTR('Hello SQL!', -4, 3) FROM dual --从倒数第4个字符开始，截取3个字符。返回'SQL'
```

#### instr() 字符串定位函数

- 语法:

```sql
--格式一：
instr( string1, string2 )    // instr(源字符串, 目标字符串)

--格式二：
instr( string1, string2 [, start_position [, nth_appearance ] ] )   // instr(源字符串, 目标字符串, 起始位置, 匹配序号)
```

- 案例:

```sql
格式一:
 1 select instr('chen','e')from dual;  // 3

格式二:
 1 select instr('helloworld','l',2,2) from dual;  --返回结果：4    也就是说：在"helloworld"的第2(e)号位置开始，查找第二次出现的“l”的位置
 2 select instr('helloworld','l',-1,1) from dual;  --返回结果：9    也就是说：在"helloworld"的倒数第1(d)号位置开始，往回查找第一次出现的“l”的位置
 3  select instr('helloworld','l',-2,2) from dual;  --返回结果：4    也就是说：在"helloworld"的倒数第2(l)号位置开始，往回查找第二次出现的“l”的位置
```

#### replace() 字符串替换函数

- 语法:

```sql
replace(目标串,目标串中内容，替换内容)
```

- 案例:

```sql
select replace('chenzhixiang','chen','666') from dual; -- Result :666zhixiang;
select replace('chenzhixiang','cen','666')  from dual; -- Result: chenzhixiang; 没有匹配到cen这个串，就不会替换。
```

#### translate() 字符创转换函数

- 语法:

```sql
2.1.4 字符串替换、转换函数
转换函数：translate 针对单个字符
  语法：TRANSLATE(char, from, to)
  1. 用法：返回将出现在from中的每个字符替换为to中的相应字符以后的字符串。若from比to字符串长，那么在from中比to中多出的字符将会被删除。
  2. 三个参数中有一个是空，返回值也将是空值。
```

- 案例：

```sql
select translate('aabcdefg','ac','00') from dual;  -- Result:00b0defg。a对应0 c对应0 。然后依次转换。
```

#### length() 获得字符串长度函数

- 语法：

```sql
length('字符串')
```

- 案例:

```sql
select length('chen') from dual;-- Result 4 
```


#### upper() lower()大小写转换函数

- 案例

```sql
select upper('chen') from dual;-- 小写变大写。-- Result:CHEN
select lower('CHEN') from dual;-- 大写变小写。-- Result:chen
```

#### || 字符串拼接

- 案例

```sql
select 'chen'||'zhi'||'xiang' from dual; --Result：chenzhxiaing   || 运算符。也可以用于拼接字符串
```

### 2.3 数值函数

#### round()函数： 四舍五入

- 案例：

```sql
select round(3.56788) from dual;      -- Result :4       默认情况四舍五入
select round(3.56788，2) from dual;   -- Result :3.57    保留两位后四舍五入
select round(356.788，-2) from dual;  -- Result :400     从小数点左边开始精确
```

#### trunc() 保留小数点函数

- 案例

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

#### mod() 取余函数

- 案例：

```sql
select mod(3,2) from dual; --Result: 1   3/2=1 ...1 
```

#### greatest() 取最大值

-- 案例

```sql
-- 数值类
-- 1.1 都为数值类型
select greatest(2, 5, 12, 3, 16, 8, 9) a from dual;-- 16 
-- 1.2 部分为数值型，但是字符串可以根据expr_1的数据类型通过隐式类型转换转成数值型：
SELECT GREATEST(2, '5', 12, 3, 16, 8, 9) A FROM DUAL; -- 16
-- 1.3 部分为数值型，但是字符串不能通过隐式类型转换成数值型会报错，因为字符串A不能转换成数值型：
SELECTGREATEST(2, 'A', 12, 3, 16, 8, 9) A FROM DUAL;  -- 报错

-- 字符类
-- 2.1 按首字母进行比较（如果相等则向下比较）全部为字符型，取出最大值G：
SELECT GREATEST('A', 'B', 'C', 'D', 'E', 'F','G') A FROM DUAL;
-- 2.2 部分为字符型，会把非字符型转换成字符型
SELECT GREATEST('A', 6, 7, 5000, 'E', 'F','G') A FROM DUAL;
```

### 2.4 多行函数

#### count()函数 : 统计表中总记录数

- 案例

```sql
select count(*) from emp;
select count(ename) 总记录数 from emp;
```

#### min()、max()、avg()、sum() 最大值、最小值,平均值、求和值。

- 案例

```sql
--eg: 查询雇员表中工资最少的
select min(sal) from emp;
--eg: 查询雇员表中工资最多的
select ename,job from emp where sal=(select max(sal) from emp);
--eg: 查询雇员表中工资最的平均值
select avg(sal) from emp;
--eg: 查询雇员表工资一个月总共发了多少
select sum(sal) from emp;
```

### 2.5 开窗函数

[第一次参考链接](https://www.cnblogs.com/mzq123/p/10200780.html)

```sql
-- 第一次参考链接表结构。
create table t_score(
   stuId varchar2(20),
   stuName varchar2(50),
   classId number,
   score float
);
-- 插入测试数据
insert into t_score (STUID, STUNAME, CLASSID, SCORE)
values ('1001', '小王', 1, 92);

insert into t_score (STUID, STUNAME, CLASSID, SCORE)
values ('1002', '赵云', 1, 92);

insert into t_score (STUID, STUNAME, CLASSID, SCORE)
values ('1003', '小李', 1, 90);

insert into t_score (STUID, STUNAME, CLASSID, SCORE)
values ('2001', '小钱', 2, 92);

insert into t_score (STUID, STUNAME, CLASSID, SCORE)
values ('2002', '小顺', 2, 100);
```

[第二次参考链接（比较全）](https://blog.csdn.net/jerrytomcat/article/details/82790543)	

```sql
-- 第二次参考链接表结构，和数据。
create table student_over(

   name varchar2(20),
   city varchar2(20),
   age  int ,
   salary int
)

-- 插入测试数据
--插入数据
INSERT INTO student_over(name,city,age,salary)
VALUES('Kebi','JiangSu',20,3000);
INSERT INTO student_over(name,city,age,salary)
VALUES('James','ChengDu',21,4000);
INSERT INTO student_over(name,city,age,salary)
VALUES('Denglun','BeiJing',22,3500);
INSERT INTO student_over(name,city,age,salary)
VALUES('Yangmi','London',21,2500);
INSERT INTO student_over(name,city,age,salary)
VALUES('Nana','NewYork',22,1000);
INSERT INTO student_over(name,city,age,salary)
VALUES('Sunli','BeiJing',20,3000);
INSERT INTO student_over(name,city,age,salary)
VALUES('Dengchao','London',22,1500);
INSERT INTO student_over(name,city,age,salary)
VALUES('Huge','JiangSu',20,2800);
INSERT INTO student_over(name,city,age,salary)
VALUES('Pengyuyan','BeiJing',24,4500);
INSERT INTO student_over(name,city,age,salary)
VALUES('Baoluo','London',25,8500);
INSERT INTO student_over(name,city,age,salary)
VALUES('Huting','ChengDu',25,3000);
INSERT INTO student_over(name,city,age,salary)
VALUES('Hurenxiang','JiangSu',23,2500);
```

### 2.6 列转行的函数 

- wm_concat(**column**) 

>```sql
>select *from student;
>```
>
>![01](00%20Oracle.assets/20201002162115379.png)
>
>```sql
>select wm_concat(sname) from student;
>```
>
>- 查询结果
>
>  > - 这是一个多行行数。
>  > - 相当于把多行的数据整合到一行。并用 ， 隔开。
>
>![02](00%20Oracle.assets/2020100216232535.png)



- xmlagg(xmlparse(content **字段名**||',' wellformed) order by **字段名**).getclobval()  别名

> ```sql
> select 
>   xmlagg(xmlparse(content s.sname||',' wellformed) order by s.sname).getclobval() as 姓名
> from student s;sql
> ```
>
> - 查询结果
>
> > - 这是一个多行行数。
> > - 也是相当于把多行的数据整合到一行。并用 ， 隔开。
>
> ![](00%20Oracle.assets/20201004180514203.png)

- listagg(**字段名**,',')within group(order by **字段名**) name from scott.emp;

>
>
>```sql
>select listagg(ename,',')within group(order by sal)name from scott.emp;
>```
>
>- 查询结果
>
>![](00%20Oracle.assets/20201004181858991.png)
>
>```sql
>-- 和分组函数一起使用
>select 
> deptno,
> listagg(ename,',')within group(order by sal)name 
>from scott.emp 
>group by deptno;
>```
>
>![](00%20Oracle.assets/20201004182035425.png)
>
>```sql
>-- 和 over 分析函数一起使用
>select deptno,
>       ename,
>       sal,
>       listagg(ename,',')within group(order by sal)over(partition by deptno)name 
> from scott.emp;
>```
>
>![](00%20Oracle.assets/20201004182134102.png)



### 2.7 填充函数： lpad()  &&  rpad()

> 函数参数：lpad( **string1**, **padded_length**, [ pad_string ] )
>
> 函数参数：rpad( **string1**, **padded_length**, [ pad_string ] )
>
> - String1: 源字符串
> - padded_length: 即最终结果返回的字符串的长度；
>   1. 如果最终返回的字符串的长度比源字符串的小，那么此函数实际上对源串进行截取处理，与substr(string,number1,number2)的作用完全相同;
>   2. 如果padded_length比源字符串的长度长，则用pad_string进行填充，确保返回的最终字符串的长度为padded_length;
>
> - pad_string : 填充内容

- 案例

```sql
select lpad('123456',2) from dual  

--结果为 12


select lpad('123456',7,'0') from dual

--结果为 0123456
--注意在左侧填充 lpad中的l为left，左侧的意思


select rpad('123456',7,'0') from dual

--结果为 1234560
--rpad 填充在右侧，r为right 右侧

```

### 2.8  空值处理函数 ： nvl() coalesce()  

> 函数形式：**NVL**(expr1,expr2)
>
> - 如果expr1为NULL，则该函数显示expr2的值；
>
> 函数形式：**NVL2**(expr1,expr2,expr3)
>
> - 如果expr1的值为NULL，则该函数显示expr3的值；不为NULL，显示expr2的值；
>
> 函数形式：**NULLIF**(expr1,expr2)
>
> - 如果expr1=expr2,返回NULL；若不等，则返回第一个表达式的值；
>
>   ```sql
>   select nullif(1,2) from dual;  -- 结果 1
>   select nullif(1,1) from dual;  -- 结果 null
>   ```

> 函数形式：COALESCE ( **expression**,**value1**,value2……,valuen) 
>
> - 函数的第一个参数expression为待检测的表达式，而其后的参数个数不定。
> -  COALESCE()函数将会返回包括expression在内的所有参数中的第一个非空表达式。
>   1. 如果expression不为空值则返回expression；否则判断value1是否是空值。
>   2. 如果value1不为空值则返回value1；否则判断value2是否是空值。
>   3. 如果value2不为空值则返回value2；……以此类推，如果所有的表达式都为空值，则返回NULL。

```sql
select COALESCE(rpad(pid,6,'0'),rpad(name,6,'0'),rpad(age,6,'0')) from person;
```

### 2.9 dbms_lob 中substr,append,write用法

[参考链接](https://blog.csdn.net/daxiang12092205/article/details/20283359)

- 准备工作

```sql
-- 建表
create table bak_DBMS_LOB_0302(
  bak_id number(4),
  bak_comment clob
);
commit;

-- 插入测试数据
delete from bak_dbms_lob_0302;
insert into bak_dbms_lob_0302(bak_id,bak_comment) values(1,'a');
insert into bak_dbms_lob_0302(bak_id,bak_comment) values(2,'ab');
insert into bak_dbms_lob_0302(bak_id,bak_comment) values(3,'abcdefgccccccc');
insert into bak_dbms_lob_0302(bak_id,bak_comment) values(4,'a   bcdefg');
```

#### 1 dbms_lob .substr()

> 1. 判定长度  DBMS_LOB.GETLENGTH(col1)
> 2. 获取文本  DBMS_LOB.SUBSTR(col1,n,pos)
> 3. DBMS_LOB.SUBSTR(col1,10,1)表示从第1个字节开始取出10个字节
> 4. DBMS_LOB.SUBSTR(CLOB_VAR,32767)表示截取CLOB变量保存的全部数据
> 5. DBMS_LOB.FILECLOSE(IMG_BFILE)关闭文件

- 长度

>```sql
>-- 1 求长度
>select dbms_lob.getlength(bak_comment) from bak_dbms_lob_0302;
>select length(bak_comment) from bak_dbms_lob_0302;
>-- 一个作用于正常数据，一个使用clob 。
>```

- 字符串截取

>```sql
>-- 1 字符串截取
>select substr('chenzx',1,3) from dual;  -- 结果 che
>select dbms_lob.substr('chenzx',1,2) from dual; -- 结果 h
>-- 总结： 这两个substr 的参数顺序是不一样的。注意
>```
>
>```sql
>-- 2 取clob块中所有内容
>select dbms_lob.substr(bak_comment,32767) from bak_dbms_lob_0302;
>```

#### 2 dbms_lob .append()

> dbms_lob.append 和 dbms_lob.write
> append存储过程用于将一个大对象添加到另一个大对象中，此时是将源对象的内容全部添加过去。
>
> 
>
> append存储过程的语法如下：
>
> ```sql
> dbms_lob.append(
>   dest_lob in out nocopy blob,
>   src_lob in  blob);
> dbms_lob.append(
>   dest_lob in out nocopy clob character set any_cs,
>   src_lob in clob character set dest_lob%charset);
> ```
>
> 其中，各个参数的含义如下：
>
> - dest_lob是被源lob添加到的目标lob的定位器。
> - src_lob是源lob的定位器。
> - any_cs用来指定字符集。

- 案例

```sql
declare
  src_lob clob;
  dest_lob clob;
  write_amount integer:= 18;
  writing_position integer;
  buffer_text varchar2(20) := 'added text to clob';
begin
  select bak_comment into dest_lob from bak_dbms_lob_0302 where bak_id = 1 for update; -- 注意 for update 一定需要。
  select bak_comment into src_lob from bak_dbms_lob_0302 where bak_id = 2;
  -- 字符串合并后自动更新到数据表中
  dbms_lob.append(dest_lob,src_lob);   -- dest_lob = dest_lob + src_lob 。然后将 dest_lob 数据更新到数据库。
  dbms_output.put_line(src_lob);
  dbms_output.put_line(dest_lob);
  commit;
  
  end;
/
```

#### 3 dbms_lob .write()

> write存储过程
> write存储过程能够将数据写入大型对象中。写的位置是从大型对象开始处的某个绝对偏移地址，数据从缓冲区参数被写入。写操作将覆盖已经在大型对象偏移地址处存在的任何长度为指定的数据。如果输入数多于在缓冲区的数据，将产生一个错误。如果输入数量小于在缓冲区的数据，那么只有缓冲区的数据字节活字符被写给大型对象。
>
> write存储过程的语法如下：
>
> ```sql
> dbms_lob.write(
>  lob_loc in out nocopy blob,
>  amount in binary_integer,
>  offset in integer,
>  buffer in raw);
> dbms_lob.write(
>  lob_loc in out nocopy clob character set any_cs,
>  amount in binary_integer,
>  offset in integer,
>  buffer in varchar2 character set lob_loc%charset);
> ```
>
> 其中各个参数的含义如下：
>
> 1. lob_loc是要操作的大型对象定位器。
> 2. amount是要写道大型对象中去的字节数量。
> 3. offset是指定将数据写入到大型对象什么位置的偏移地址。
> 4. buffer是写入到大型对象的数据缓冲区。
> 5. any_cs指定要使用的字符集。

- 案例

```sql
declare
  src_lob clob;
  dest_lob clob;
  write_amount integer:= 18;
  writing_position integer;
  buffer_text varchar2(20) := 'added text to clob';
begin

  select bak_comment into dest_lob from bak_dbms_lob_0302 
    where bak_id = 2 for update;
  writing_position := dbms_lob.getlength(dest_lob) + 1;
  -- 将指定长度的字符串添加到字段末尾，并更新到数据表中
  dbms_lob.write(dest_lob,write_amount,writing_position,buffer_text);
  dbms_output.put_line(dest_lob);
  dbms_output.put_line(write_amount);
  dbms_output.put_line(writing_position);
  commit;

end;
/
```



## 3 Sql 编程

### 3.1 变量

- 定义后直接设置默认值

```sql
-- 变量的定义以及赋值
declare
    i      number(10) := 10;       -- 第一类: 定义后直接赋值
    s      varchar2(10) := '祥子';
    p_name person.name%type;       -- 第二类: 引用已有类型
```

- 定义显示游标游标

```sql
 -- 定义
 type cur_type is ref cursor;
 cur cur_type;
 
 -- 使用
 open rules for v_rulesql;
```

- 使用自定义存储结构

```sql
-- 定义
CREATE OR REPLACE PACKAGE xpp AS
  TYPE t_detail IS REF CURSOR;
END xpp;

-- 使用
out_cursor   out xpp.t_detail
```



### 3.2 if 判断

```sql
--案例需求
---- 1.输入一个年龄参数
------如果 age<18 岁 输出未成年
------如果 age<40 输出中年人。
------如果 age>40 输出老年人。

declare
    i_age number(3) := &i_age;

begin
    if i_age < 18 then
        dbms_output.put_line('未成年');
    elsif i_age < 40 then
        dbms_output.put_line('中年人');
    else
        dbms_output.put_line('老年人');
    end if;
end;

```

### 3.3 循环

```sql
-- 需求： 控制台打印1-10

-- while 循环

declare
    i number(2) := 1;
begin
    while i<11 loop
      dbms_output.put_line(i);
      i :=i+1;
    end loop;
end;

-- for 循环
declare
  s number(3) :=100;
begin
    for i in 1 .. s loop
        dbms_output.put_line(i);
    end loop;
end;

-- exit 循环
declare
    i number(2) := 1;
begin
    loop
        exit when i > 10;
        dbms_output.put_line(i);
        i := i + 1;
    end loop;
end;
```

### 3.4 存储过程

- 案例: 需求：给指定员工涨100工资  => 只有入参

```sql
create or replace procedure p_chen1(eno emp.empno%type) is
begin
    update emp t set sal = sal + 100 where t.empno = eno;
    commit;
end;
```

> 测试

```sql
-- 测试存储过程
declare
begin
    p_chen1(7788);
end;
```


- 案例: 需求：求指定员工的年薪。 => 含有in 入参和out 出参

```sql
create or replace procedure p_yearsql(eno     emp.empno%type,
                                      yearsal out number) is
    n_sal  emp.sal%type;
    n_comm emp.comm%type;
begin
    select t.sal, nvl(t.comm,0) into n_sal, n_comm from emp t where t.empno = eno;
    yearsal := n_sal + n_comm;
end;
```

> 测试

```sql
declare 
   yearsal number(10);
begin
    p_yearsql(7788,yearsal);
    dbms_output.put_line(yearsal);
end;
```

### 3.5 存储函数

> 1. 存储过程和存储函数的参数都不可带参数 。
> 2. 存储函数的返回值类型不可以带长度。


- 案例: 通过存储函数实现计算指定员工的年薪

```sql
create or replace function f_yearsal(eno emp.empno%type) return number is
    s number(10);
begin
    select t.sal * 12 + nvl(t.comm, 0)
      into s
      from emp t
     where t.empno = eno;
    return s;
end;
```

> 测试

```sql
-- 测试
-- 存储函数的返回值需要接收。
declare
 s number(10);
begin
    s :=f_yearsal(7788);
    dbms_output.put_line(s);
end;

```

####  sp_srdt_checklogexport

```sql
CREATE OR REPLACE PROCEDURE sp_srdt_checklogexport(return_code  out number,
                                                  return_str   out varchar2
                                                  ) is
/****************************************************************************************
    名  称：EAST4.0数据校验汇总
    功  能：EAST4.0数据校验汇总
    创建者：zhangpeng21195
    描  述:
    日  期：2020-09-23
    版  本: 20200923 add  by zhangpeng21195:
****************************************************************************************/
vsql                    varchar2(4000);         --拼接的sql语句
vsql2                   varchar2(4000);         --拼接的sql语句
vsql3                   varchar2(4000);         --拼接的sql语句
vsql4                   varchar2(4000);         --拼接的sql语句
vsql5                   varchar2(4000);         --拼接的sql语句
vsql6                   varchar2(4000);         --拼接的sql语句
vfieldname              varchar2(4000);         --拼接的sql语句
vfieldvalue             varchar2(4000);         --拼接的sql语句
vfieldcaption           varchar2(4000);         --拼接的sql语句
vkeystring              varchar2(4000);         --拼接的sql语句
vxtcpdm                 varchar2(255);          --产品代码
vxtcpqc                 varchar2(255);          --产品名称
vxtjlgh                 varchar2(255);          --产品经理工号
vygxm                   varchar2(255);          --产品经理名称
vssbm                   varchar2(255);          --所属部门
vkhbh                   varchar2(255);          --理财账号/交易对手客户编号
vkhqc                   varchar2(255);          --客户名称
v_stat                  number;
l_cursor                number;
cursor v_cur_checklog is
    select m.c_ruleid,m.c_tablepk,m.c_nullvalue,m.c_lengthvalue,m.c_errorvalue,n.c_tablename,n.c_displaycaption,n.c_tabletitle,n.c_fieldname
    from (
        select xmlagg(xmlparse(content b.c_ruleid||',' wellformed) order by b.c_ruleid).getclobval() as c_ruleid,b.c_tablepk,a.c_checktable,xmlagg(xmlparse(content (case when instr(b.c_errorvalue, '不能为空') > 0 then b.c_errorvalue||',' else null end) wellformed) order by b.c_errorvalue).getclobval() as c_nullvalue, xmlagg(xmlparse (content (case when instr(b.c_errorvalue, '字节长度不能大于') > 0 then b.c_errorvalue||',' else null end) wellformed) order by b.c_errorvalue).getclobval() as c_lengthvalue, xmlagg(xmlparse(content b.c_errorvalue||',' wellformed) order by b.c_errorvalue).getclobval() as c_errorvalue
        from srdt_rule a,srdt_checklog b
        where a.c_ruleid = b.c_ruleid
            and a.l_isenable = 1
        group by b.c_tablepk,a.c_checktable) m, srdt_checklogconfig n
    where m.c_checktable = n.c_tablename;
type ref_col is ref cursor;
c_col ref_col;
c_fieldvalue ref_col;
c_xtcpdm ref_col;
c_khbh ref_col;
begin
    --1 获取校验规则数据 '||''''||in_checktable|| '''';
    vsql:='truncate table srdt_checklogexport';

    execute immediate vsql;

    for checklog in v_cur_checklog loop
        vsql2 := 'select t1.column_value field_name,t2.column_value field_caption from (select rownum rn,t.* from table(fn_split('''||checklog.c_fieldname||''',''|'')) t ) t1 '
            ||'left join (select rownum rn,tt.* from table(fn_split('''||checklog.c_displaycaption||''',''|'')) tt) t2 on  t1.rn = t2.rn';
        vsql3 := '';        
        vfieldname := '';
        vfieldvalue := '';
        vfieldcaption := '';
        vkeystring := '';
        open c_col for vsql2;
        loop
            fetch c_col into vfieldname,vfieldcaption;
            exit when c_col%notfound;
            if nvl(vfieldname,'#') <> '#'  then
                vsql3 := 'select '||vfieldname||' from '||checklog.c_tablename||' where c_id = '''||checklog.c_tablepk||'''';
            end if;
            open c_fieldvalue for vsql3;
            loop
              fetch c_fieldvalue into vfieldvalue;
              exit;
            end loop;
            close c_fieldvalue;
            vkeystring := vkeystring||vfieldcaption||':'||vfieldvalue||'; ';
        end loop;
        close c_col;
        
        vxtcpdm :='';
        vxtcpqc :='';
        vxtjlgh :='';
        vygxm   :='';
        vssbm   :='';
        vkhbh   :='';
        vkhqc   :='';
        if instr('B01,B02,B03',dbms_lob.substr(checklog.c_ruleid,3,1)) > 0 then
            vsql6 :='select c_tano, c_clientname from hstdc.cm_client t where t.c_clientid = (select c_khbh from '||checklog.c_tablename||' where c_id = '''||checklog.c_tablepk||''')';
            open c_khbh for vsql6;
            loop
                fetch c_khbh into vkhbh,vkhqc;
                exit when c_khbh%notfound;                
            end loop;
            close c_khbh;
        elsif instr('B04,B05,B06,B07',dbms_lob.substr(checklog.c_ruleid,3,1)) > 0 then
            vsql6 :='select c_rivalid, c_rivalname from hstdc.cm_rival t where t.c_rivalid = (select c_jydsbh from '||checklog.c_tablename||' where c_id = '''||checklog.c_tablepk||''')';
            open c_khbh for vsql6;
            loop
                fetch c_khbh into vkhbh,vkhqc;
                exit when c_khbh%notfound;                
            end loop;
            close c_khbh;
        end if;
        if instr('B,I',dbms_lob.substr(checklog.c_ruleid,1,1)) = 0 then
            vsql4 :='select a.c_xtcpdm,a.c_xtcpqc,a.c_xtjlgh,b.c_staffname,c.c_deptname from srdt_xtcpjbxx a,hstdc.bd_staff b,hstdc.bd_dept c where a.c_xtjlgh = b.c_staffid and b.c_deptid = c.c_deptid and a.c_xtcpdm = (select c_xtcpdm from '||checklog.c_tablename||' where c_id = '''||checklog.c_tablepk||''')';
            open c_xtcpdm for vsql4;
            loop
                fetch c_xtcpdm into vxtcpdm,vxtcpqc,vxtjlgh,vygxm,vssbm;
                exit when c_xtcpdm%notfound;                
            end loop;
            close c_xtcpdm;
        end if;
        
        
        l_cursor := dbms_sql.open_cursor;
        vsql5 := 'insert into srdt_checklogexport(c_id,c_xtcpdm,c_xtcpqc,c_xtjlgh,c_xtjl,c_ssbm,c_khbh,c_khqc,c_ruleid,c_tablepk,c_checktable,c_keystring,c_nullvalue,c_lengthvalue,c_errorvalue) values(:c_id,:c_xtcpdm,:c_xtcpqc,:c_xtjlgh,:c_xtjl,:c_ssbm,:c_khbh,:c_khqc,:c_ruleid,:c_tablepk,:c_checktable,:c_keystring,:c_nullvalue,:c_lengthvalue,:c_errorvalue)';
        dbms_sql.parse(l_cursor, vsql5, dbms_sql.native);
        dbms_sql.bind_variable(l_cursor, ':c_id', sys_guid());
        dbms_sql.bind_variable(l_cursor, ':c_xtcpdm', vxtcpdm);
        dbms_sql.bind_variable(l_cursor, ':c_xtcpqc', vxtcpqc);
        dbms_sql.bind_variable(l_cursor, ':c_xtjlgh', vxtjlgh);
        dbms_sql.bind_variable(l_cursor, ':c_xtjl', vygxm);
        dbms_sql.bind_variable(l_cursor, ':c_ssbm', vssbm);
        dbms_sql.bind_variable(l_cursor, ':c_khbh', vkhbh);
        dbms_sql.bind_variable(l_cursor, ':c_khqc', vkhqc);
        dbms_sql.bind_variable(l_cursor, ':c_ruleid', checklog.c_ruleid);
        dbms_sql.bind_variable(l_cursor, ':c_tablepk', checklog.c_tablepk);
        dbms_sql.bind_variable(l_cursor, ':c_checktable', checklog.c_tabletitle);
        dbms_sql.bind_variable(l_cursor, ':c_keystring', vkeystring);
        dbms_sql.bind_variable(l_cursor, ':c_nullvalue', checklog.c_nullvalue);
        dbms_sql.bind_variable(l_cursor, ':c_lengthvalue', checklog.c_lengthvalue);
        dbms_sql.bind_variable(l_cursor, ':c_errorvalue', checklog.c_errorvalue);
        v_stat := dbms_sql.execute(l_cursor);  --执行
        dbms_sql.close_cursor(l_cursor);   --关闭游标
        commit;
    end loop;

exception
    when others then
        --系统自动异常捕捉
        return_code := -1;
        return_str  := '[sp_srdt_validaterules-east4.0校验]报异常错误:' || chr(13) || sqlerrm;
        rollback;
        raise_application_error(-20018,return_str);
end sp_srdt_checklogexport;
/
```



## 4 动态Sql

###  参考一

- 表结构

```sql
-- 表结构
create table test(
  n_id   number, 
  v_name  varchar2(50), 
  d_insert_date date
 );
 
alter table test add constraint pk_id  primary key(n_id);
```

- 例1 ：利用动态sql 向测试表中插入数据。

```sql
declare
   v_cursor   number;
   v_sql      varchar2(200);
   v_id       number;
   v_name     varchar2(50);
   v_date     date;
   v_stat     number;
begin
   
   v_id := 1;
   v_name := '测试 insert';
   v_date := sysdate;
   v_cursor := dbms_sql.open_cursor;  --打开游标
   v_sql := 'insert into test(n_id, v_name, d_insert_date) values(:v_id,:v_name,:v_date)';
   dbms_sql.parse(v_cursor, v_sql, dbms_sql.native);  --解析SQL
   dbms_sql.bind_variable(v_cursor, ':v_id', v_id);   --绑定变量
   dbms_sql.bind_variable(v_cursor, ':v_name', v_name);
   dbms_sql.bind_variable(v_cursor, ':v_date', v_date);
   
   v_stat := dbms_sql.execute(v_cursor);  --执行
   dbms_sql.close_cursor(v_cursor);   --关闭游标
   commit;
end;
```

- 例2 ：更新测试表中数据

```sql
declare
   v_cursor   number;
   v_sql      varchar2(200);
   v_id       number;
   v_name     varchar2(50);
   v_stat     number;
begin
    v_name := '测试 update';
    v_id := 1;
    v_cursor := dbms_sql.open_cursor;
    v_sql := 'update test set v_name = :v_name, d_insert_date = :v_date where n_id = :v_id';
    dbms_sql.parse(v_cursor, v_sql, dbms_sql.native);
    dbms_sql.bind_variable(v_cursor, ':v_name', v_name);
    dbms_sql.bind_variable(v_cursor, ':v_date', sysdate);
    dbms_sql.bind_variable(v_cursor, ':v_id', v_id);
    v_stat := dbms_sql.execute(v_cursor);
    dbms_sql.close_cursor(v_cursor);
    commit;
end;
```

- 例3： 删除测试表中数据

```sql
declare
    v_cursor   number;
    v_sql      varchar2(200);
    v_id       number;
    v_stat     number;
begin

   v_id := 1;
   v_sql := 'delete from test where n_id = :v_id';
   v_cursor := dbms_sql.open_cursor;
   dbms_sql.parse(v_cursor, v_sql, dbms_sql.native);
   dbms_sql.bind_variable(v_cursor, ':v_id', v_id);
   v_stat := dbms_sql.execute(v_cursor);
   dbms_sql.close_cursor(v_cursor);
   commit;
end;
```

- 例4 

```sql
declare
   v_cursor    number;
   v_sql       varchar2(200);
   v_id        number;
   v_name      varchar2(50);
   v_date      varchar2(10);
   v_stat      number;
begin

    v_sql := 'select n_id, v_name, to_char(d_insert_date, ''yyyy-mm-dd'') from test';
    v_cursor := dbms_sql.open_cursor;              --打开游标
    dbms_sql.parse(v_cursor, v_sql, dbms_sql.native);  --解析游标
    dbms_sql.define_column(v_cursor, 1, v_id);         --定义列
    dbms_sql.define_column(v_cursor, 2, v_name, 50);   --注意：当变量为varchar2类型时，要加长度
    dbms_sql.define_column(v_cursor, 3, v_date, 10);
    v_stat := dbms_sql.execute(v_cursor);        --执行SQL
    loop
        exit when dbms_sql.fetch_rows(v_cursor) <= 0;  --fetch_rows在结果集中移动游标，如果未抵达末尾，返回1。
        dbms_sql.column_value(v_cursor, 1, v_id);   --将当前行的查询结果写入上面定义的列中。
        dbms_sql.column_value(v_cursor, 2, v_name);
        dbms_sql.column_value(v_cursor, 3, v_date);
        dbms_output.put_line(v_id || ':' || v_name || ':' || v_date);
    end loop;
end;
```

## Oracle 性能优化

- 使用 hint  进行性能优化。

```
6.推荐的书：
梁敬彬老师的《收获,不止Oracle》（这本书技术知识点可能讲得不多不细，但是我个人觉得里边的一些方法论对于从事IT行业很受用），
还有盖国强老师的书（写得很好），
其它还有一些入门书随意看看如“Oracle PL/SQL从入门到精通”这类的；学习网站呢比较常用的就是itpub了。


```

[参考链接一](https://blog.csdn.net/xiaoxing1521025/article/details/17080343?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

[参考链接二](https://blog.csdn.net/yh_zeng2/article/details/83552333?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160180284119725255500950%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160180284119725255500950&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v28-1-83552333.pc_first_rank_v2_rank_v28&utm_term=oracle+hint&spm=1018.2118.3001.4187)

[参考链接三](https://blog.csdn.net/czmmiao/article/details/84182211?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160180284119725255500950%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160180284119725255500950&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-84182211.pc_first_rank_v2_rank_v28&utm_term=oracle+hint&spm=1018.2118.3001.4187)

### 1 Oracle 备份与还原

```sql
-- 创建表空间
create    tablespace   TPFS
datafile 'E:\oracle\dmp\tpfs\TPFS.dbf' size 1024M
autoextend on next 32M
extent management local;

-- 创建用户并授权
create user tpfs identified by tpfs default tablespace TPFS;
grant connect,resource,create table,create view to tpfs;
grant dba to tpfs;

-- 删除用户
drop user  tpfs; --仅仅是删除用户，
drop user  tpfs cascade ;--会删除此用户名下的所有表和视图。

-- 删除表空间
DROP TABLESPACE TPFS INCLUDING CONTENTS AND DATAFILES;

--cmd 直接在win10命令行中执行（使用管理员权限）
-- 导入
imp tpfs/tpfs@orcl file=E:\oracle\dmp\tpfs\tpfs.dmp fromuser=tpfs touser=tpfs
-- 
```

### 2 创建db link 

```sql
-- 创建 db link 【connect to 用户名 identified by 密码  Host : 10.20.31.80】
create database link TUSP_KF
connect to tusp_kf identified by htdc1SJYY
using '(DESCRIPTION =
            (ADDRESS_LIST =
                (ADDRESS = (PROTOCOL = TCP)(HOST = 172.28.1.30)(PORT = 1521)) 
             )
             (CONNECT_DATA =
                (SERVER = DEDICATED ) 
                (SERVICE_NAME = orcl)  
              )
 )';
 
drop database link  TUSP_KF; 
```



## 遇见问题

### 1 插入数据库中文乱码问题

- (1) 查询客服端和服务端编码

```sql
-- 查询服务端编码
select userenv('language') from dual;
select * from v$nls_parameters a where a.PARAMETER = 'NLS_CHARACTERSET';
-- 查询客户端编码
select * from v$nls_parameters

-- 查询服务端编码
select * from nls_database_parameters t where t.PARAMETER = 'NLS_CHARACTERSET';
```

> 然后修改本机环境变量，和数据库服务端保持一致就可以！

### 2 锁表问题

- 1 查询被锁的表

```sql
Select t2.username,t2.sid,t2.serial#,t2.logon_time from v$locked_object t1,v$session t2 where t1.session_id=t2.sid;
```

![2020.7.9](00%20Oracle.assets/20200709194526522.png)

- 2 kill 锁住的id

```sql
alter system kill session '17, 15453'; 
```

> 注意：要查看kill的session 是否是自己的，防止kill他人的进程。

### 3 连接不了Oracle 数据库
#### 3.1 Listener 配置文件

```sql
-- 2020-10-28 号配置
# listener.ora Network Configuration File: E:\oracle\oracle_home\product\11.2.0\dbhome_1\NETWORK\ADMIN\listener.ora
# Generated by Oracle configuration tools.

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (SID_NAME = CLRExtProc)
      (ORACLE_HOME = E:\oracle\oracle_home\product\11.2.0\dbhome_1)
      (PROGRAM = extproc)
      (ENVS = "EXTPROC_DLLS=ONLY:E:\oracle\oracle_home\product\11.2.0\dbhome_1\bin\oraclr11.dll")
    )
    (SID_DESC =
	  (GLOBL_DBNAME =orcl)
      (SID_NAME = ORCL)
      (ORACLE_HOME = E:\oracle\oracle_home\product\11.2.0\dbhome_1)
    )
  )

LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = chenzx31065)(PORT = 1521))
    )
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
    )
  )

ADR_BASE_LISTENER = E:\oracle\oracle_home


```

#### 3.2 tnsnames

```sql

-- 2020-10-28 号配置
# tnsnames.ora Network Configuration File: E:\oracle\oracle_home\product\11.2.0\dbhome_1\network\admin\tnsnames.ora
# Generated by Oracle configuration tools.

ORACLR_CONNECTION_DATA =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SERVICE_NAME = orcl)
    )
  )

ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```

### 4 用户名、密码忘了

#### 4.1 步骤

- 第一步：cmd 打开窗口
- 第二步：输入：sqlplus /nolog
- 第三步：输入：connect  /as sysdba
- 第四步：修改用户名密码： alter user sys identified by chen1996;

 ####  4.2 截图

![image-20210311230855508](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210311230855508.png)

```

![2020.7.9](00%20Oracle.assets/20200709194526522.png)

- 2 kill 锁住的id

```sql
alter system kill session '17, 15453'; 
```

> 注意：要查看kill的session 是否是自己的，防止kill他人的进程。

### 3 连接不了Oracle 数据库
#### 3.1 Listener 配置文件

```sql
-- 2020-10-28 号配置
# listener.ora Network Configuration File: E:\oracle\oracle_home\product\11.2.0\dbhome_1\NETWORK\ADMIN\listener.ora
# Generated by Oracle configuration tools.

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (SID_NAME = CLRExtProc)
      (ORACLE_HOME = E:\oracle\oracle_home\product\11.2.0\dbhome_1)
      (PROGRAM = extproc)
      (ENVS = "EXTPROC_DLLS=ONLY:E:\oracle\oracle_home\product\11.2.0\dbhome_1\bin\oraclr11.dll")
    )
    (SID_DESC =
	  (GLOBL_DBNAME =orcl)
      (SID_NAME = ORCL)
      (ORACLE_HOME = E:\oracle\oracle_home\product\11.2.0\dbhome_1)
    )
  )

LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = chenzx31065)(PORT = 1521))
    )
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
    )
  )

ADR_BASE_LISTENER = E:\oracle\oracle_home


```

#### 3.2 tnsnames

```sql

-- 2020-10-28 号配置
# tnsnames.ora Network Configuration File: E:\oracle\oracle_home\product\11.2.0\dbhome_1\network\admin\tnsnames.ora
# Generated by Oracle configuration tools.

ORACLR_CONNECTION_DATA =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SERVICE_NAME = orcl)
    )
  )

ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```

### 4 用户名、密码忘了

#### 4.1 步骤

- 第一步：cmd 打开窗口
- 第二步：输入：sqlplus /nolog
- 第三步：输入：connect  /as sysdba
- 第四步：修改用户名密码： alter user sys identified by chen1996;

 ####  4.2 截图

![image-20210311230855508](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210311230855508.png)

### 4.3 修改其他用户的用户名密码

```sql
-- 修改用户名密码
alter user chen identified by chen;
-- 解锁用户
alter user scott account unlock;

mvn install:install-file -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.1.0 -Dpackaging=jar -Dfile=D:\16-tempFile\oracle_lib\lib\ojdbc6.jar
```

#### 5 Sql 语句执行的优先级

```sql
1. from语句
2. where语句(结合条件)
3. start with语句
4. connect by语句
5. where语句
6. group by语句
7. having语句
8. model语句
9. select语句
10. union、minus、intersect等集合演算演算
11. order by语句
```

