### Scott 用户表结构

[Scott 用户表结构](https://blog.csdn.net/flyer_tang/article/details/80597395)

- dept 表数据

```sql
prompt Importing table dept...
set feedback off
set define off

insert into dept (DEPTNO, DNAME, LOC)
values (10, 'ACCOUNTING', 'NEW YORK');

insert into dept (DEPTNO, DNAME, LOC)
values (20, 'RESEARCH', 'DALLAS');

insert into dept (DEPTNO, DNAME, LOC)
values (30, 'SALES', 'CHICAGO');

insert into dept (DEPTNO, DNAME, LOC)
values (40, 'OPERATIONS', 'BOSTON');

prompt Done.

```

-- emp 表数据

```sql
prompt Importing table emp...
set feedback off
set define off
insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7369, 'SMITH', 'CLERK', 7902, to_date('17-12-1980', 'dd-mm-yyyy'), 1000.00, 300.00, 30);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7499, 'ALLEN', 'SALESMAN', 7698, to_date('20-02-1981', 'dd-mm-yyyy'), 1600.00, 300.00, 30);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7521, 'WARD', 'SALESMAN', 7698, to_date('22-02-1981', 'dd-mm-yyyy'), 1250.00, 500.00, 30);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7566, 'JONES', 'MANAGER', 7839, to_date('02-04-1981', 'dd-mm-yyyy'), 2975.00, null, 20);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7654, 'MARTIN', 'SALESMAN', 7698, to_date('28-09-1981', 'dd-mm-yyyy'), 1250.00, 1400.00, 30);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7698, 'BLAKE', 'MANAGER', 7839, to_date('01-05-1981', 'dd-mm-yyyy'), 2850.00, 300.00, 30);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7782, 'CLARK', 'MANAGER', 7839, to_date('09-06-1981', 'dd-mm-yyyy'), 2450.00, null, 10);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7788, 'SCOTT', 'ANALYST', 7566, to_date('19-04-1987', 'dd-mm-yyyy'), 10000.00, null, 20);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7839, 'KING', 'PRESIDENT', null, to_date('17-11-1981', 'dd-mm-yyyy'), 5000.00, null, 10);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7844, 'TURNER', 'SALESMAN', 7698, to_date('08-09-1981', 'dd-mm-yyyy'), 1500.00, 0.00, 30);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7876, 'ADAMS', 'CLERK', 7788, to_date('23-05-1987', 'dd-mm-yyyy'), 1100.00, null, 20);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7900, 'JAMES', 'CLERK', 7698, to_date('03-12-1981', 'dd-mm-yyyy'), 950.00, 300.00, 30);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7902, 'FORD', 'ANALYST', 7566, to_date('03-12-1981', 'dd-mm-yyyy'), 3000.00, null, 20);

insert into emp (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
values (7934, 'MILLER', 'CLERK', 7782, to_date('23-01-1982', 'dd-mm-yyyy'), 1300.00, null, 10);

prompt Done.

```

- salGrade 工资表 表数据

```sql
prompt Importing table salgrade...
set feedback off
set define off
insert into salgrade (GRADE, LOSAL, HISAL)
values (1, 700, 1200);

insert into salgrade (GRADE, LOSAL, HISAL)
values (2, 1201, 1400);

insert into salgrade (GRADE, LOSAL, HISAL)
values (3, 1401, 2000);

insert into salgrade (GRADE, LOSAL, HISAL)
values (4, 2001, 3000);

insert into salgrade (GRADE, LOSAL, HISAL)
values (5, 3001, 9999);

prompt Done.

```

- 工资等级表（暂时没有数据）

### Scott 用户例题

[参考链接一](https://blog.csdn.net/zhiweianran/article/details/7815289)

### 例 1

```sql
-- 1.取得每个部门最高薪水的人员名称

-- 1
select 
  a.empno,
  a.ename,
  a.sal,
  a.deptno 
from emp a 
join (select deptno, max(sal) max_sal from emp group by deptno ) b 
  on a.deptno= b.deptno and a.sal = b.max_sal;
-- 2
select 
  t.empno,
  t.ename,
  t.sal,
  t.deptno 
from emp t,
     (select deptno, max(sal) max_sal from emp group by deptno) tt
where t.deptno= tt.deptno
  and t.sal=tt.max_sal
```

### 例 2

```sql
-- 2.哪些人的薪水在部门的平均薪水之上

select 
  t.empno,
  t.ename,
  t.deptno,
  t.sal,
  trunc(tt.avg_sal,2) "部门平均工资"
from 
  emp t,
  (select t.deptno deptno , avg(t.sal) avg_sal from emp t group by t.deptno) tt
where 1=1
  and t.deptno=tt.deptno
  and t.sal>tt.avg_sal
  order by t.deptno
```

### 例 3 

```sql
-- 3.取得部门中（所有人的）平均的薪水等级

-- 第一步：获得所有人的薪水等级
select
   sal.grade,
   emp.empno,
   emp.ename,
   emp.deptno
  from emp ,salgrade sal
  where emp.sal between sal.losal and sal.hisal

-- 第二步：将第一步的结果用部门进行分组。然后获取等级的平均值
select 
 t.deptno,
 trunc(avg(t.grade),2)  "部门平均等级"
from 
(
  select
   sal.grade,
   emp.empno,
   emp.ename,
   emp.deptno
  from emp ,salgrade sal
  where emp.sal between sal.losal and sal.hisal
) t
group by t.deptno
order by t.deptno
```

### 例 4

```sql
-- 方式一
select a.empno, a.ename, a.sal
  from (select empno, ename, sal from emp order by sal desc) a
 where rownum < = 1;

```



### 例 5 

```sql
-- 取得平均薪水最高的部门的部门编号。

-- 方法1
select *
  from (select t.deptno, avg(t.sal) avg_sal
          from emp t
         group by t.deptno
         order by avg_sal desc) tt
 where rownum <= 1

-- 方法2
select a.deptno, avg(sal) avg_sal
        from emp a
       group by deptno
      having avg(sal) = (select max(avg(sal)) from emp m group by deptno);
          
-- 方法3       
select m.deptno from 
(select  avg(sal) avg_sal,e.deptno from emp e group by deptno) m 
right join 
(select max(avg_sal) max_sal from (select  avg(sal) avg_sal,e.deptno 
                                   from emp e group by deptno)
 ) n
on m.avg_sal = n.max_sal    

```

###  例 6 

```sql
--6.求平均薪水的等级最低的部门的部门名称。

select 
  tt.deptno,
  avg(tt.grade) avg_grade
from(
   select 
   t.empno,
   t.ename,
   t.deptno,
   sa.grade  grade
  from emp t,salgrade sa
  where 1=1
    and t.sal between sa.losal and sa.hisal
) tt
group by tt.deptno
order by avg_grade

```

