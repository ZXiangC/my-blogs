### 1、多表join时条件写在where和on的区别

在开发过程中经常遇到这种情况：
多表关联join时，到底限制的条件是写在where后面效率高还是写在on后面，又或者是先对表过滤使表的数据量减少，到底这三种效率哪种更高，看了一堆网上说的，都没有说到具体点上，现在对这三种情况专门做以下详细说明，你就会明白到底是怎么回事了？

干货总结：(以下只适用于left join，right join，full join，不适合inner join)
1、left join where + 基表过滤条件：先对基表执行过滤，然后进行left join；
2、left join where + 被关联表过滤条件：先执行left join，然后执行过滤条件；
3、left join on+基表过滤条件：满足过滤的left join，不满足的后面补null，然后两集合并一起；
4、left join on+被关联表过滤条件：先执行过滤条件，然后执行left join；

第一种情况：

![img](https://gitee.com/ZXiangC/picture/raw/master/img/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg4ODgwNg==,size_16,color_FFFFFF,t_70)

这两种写法的执行顺序是一样的，都是先执行过滤，然后执行关联；所以运行效率是一样的！

第二种情况：

![img](https://gitee.com/ZXiangC/picture/raw/master/img/2)

第一种执行顺序：<1>先对a表进行过滤，<2>再对过滤后的a表与b表进行关联
第二种执行顺序：<1>先a表和b表进行关联，<2>再对关联的结果执行where后面b表的条件

第三种情况：

![在这里插入图片描述](https://gitee.com/ZXiangC/picture/raw/master/img/3)

第一种执行顺序是：

```sql
<1>先对a表执行过滤条件，
<2>然后过滤后的a表和b表进行关联；
```

第二种执行顺序：

```sql
<1>先使用a.city='深圳'的过滤条件将a表分为两部分，一部分满足过滤条件，一部分不满足过滤条件，
<2>对满足条件的与b表关联，不满足条件的后面字段补null，然后将满足和不满足的两部分集union起来成最后结果
第三种执行顺序：
```

```sql
<1>先对b表进行b.city=‘上海’条件对b表进行过滤，
<2>使用a.city='深圳'条件将a表分为满足和不满足条件的两部分集
<3>对满足集合与过滤后的b表进行关联，不满足集后面字段直接补null，最后将两个集合union起来成最终结果集
```

### 2、sql语句优化

[语句优化](https://www.cnblogs.com/xupccc/p/9661972.html)

[语句优化2](https://blog.csdn.net/qq_38789941/article/details/83744271)

[这哥们最好用](https://blog.csdn.net/qq_39390545/article/details/107020686?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162434649916780262567346%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162434649916780262567346&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-107020686.first_rank_v2_pc_rank_v29&utm_term=sql+%E4%BC%98%E5%8C%96&spm=1018.2226.3001.4187)

[这里面是一个体系](https://blog.csdn.net/qq_39390545/article/details/107786761)

