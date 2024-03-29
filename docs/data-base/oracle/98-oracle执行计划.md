## Oracle 执行计划

### 1、查看执行计划

   在PL/SQL Developer中写好一段SQL代码后，按F5，PL/SQL Developer会自动打开执行计划窗口，显示该SQL的执行计划。 

![image-20210616152300605](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210616152300605.png)



- Description: 列描述当前的数据库操作。
- Object owner: 列表示对象所属用户。
- Object name: 表示操作的对象。
- Cost :表示当前操作的代价（消耗），这个列基本上就是评价SQL语句的优劣。
- Cardinality:表示操作影响的行数。
- Bytes: 表示字节数,可以理解为占用内存。

​    一般而言，执行计划第一行所对应的COST(即成本耗费)值，反应了运行这段SQL的总体估计成本，单看这个总成本没有实际意义，但可以拿它与相同逻辑不同执行计划的SQL的总体COST进行比较，通常COST低的执行计划要好一些。

### 2、执行计划执行的顺序

​      执行计划按照层次逐步缩进，从左至右看，缩进最多的那一步，最先执行，如果缩进量相同，则按照从上而下的方法判断执行顺序，可粗略认为上面的步骤优先执行。每一个执行步骤都有对应的COST,可从单步COST的高低，以及单步的估计结果集（对应ROWS/基数），来分析表的访问方式，连接顺序以及连接方式是否合理。 

### 3、表的访问方式

​     表的访问方式主要是两种：全表扫描（TABLE ACCESS FULL）和索引扫描(INDEX SCAN)，如果表上存在选择性很好的索引，却走了全表扫描，而且是大表的全表扫描，就说明表的访问方式可能存在问题；若大表上没有合适的索引而走了全表扫描，就需要分析能否建立索引，或者是否能选择更合适的表连接方式和连接顺序以提高效率。

- 1） 全表扫描（Full Table Scans， FTS）

- 2） 通过ROWID的表存取（Table Access by ROWID或rowid lookup）

- 3）索引扫描（Index Scan或index lookup）有4种类型的索引扫描：
  - （1） 索引唯一扫描（index unique scan）
  - （2） 索引范围扫描（index range scan）
  - （3） 索引全扫描（index full scan）
  - （4） 索引快速扫描（index fast full scan）

​      在非唯一索引上都使用索引范围扫描。使用index rang scan的3种情况：

   　  （a） 在唯一索引列上使用了range操作符（> < <> >= <= between）

   　  （b） 在组合索引上，只使用部分列进行查询，导致查询出多行

   　  （c） 对非唯一索引列上进行的任何查询。　　



### 4、表的连接方式

- 1）  嵌套循环（*Nested  Loops* （NL））
           假如有A、B两张表进行嵌套循环连接，那么Oracle会首先从A表中提取一条记录，然后去B表中查找相应的匹配记录，如果有的话，就把该条记录的信息推到等待返回的结果集中，然后再去从A表中提取第二条记录，去在B表中找第二条匹配的记录，如果符合就推到返回的结果集中，依次类推，直到A表中的数据全部被处理完成，将结果集返回，就完成了嵌套循环连接的操作。

-  2）（散列）哈希连接（*Hash Join* （HJ））
          假如有A、B两张表进行哈希连接，那么ORACLE会首先将B表在内存中建立一棵以散列表形式存在的查询二叉树C，然后开始读取A表的第一条记录，从C中去找匹配的记录，如果有，则推到结果集中。再提取A中的第二条记录，如果有，则推到结果集中，以此类推，直到A中没有记录，返回结果集。

- 散列表就是 hash 表。

  - （1）根据散列函数计算 数组的索引位置 。
  - （2） 在索引位置里面进行逐个查询。
  - （3）结合数组，和链表的特点。进行组合。

  ![img](https://gitee.com/ZXiangC/picture/raw/master/img/Center)

- 3）（归并）排序合并连接(*Sort Merge Join* (SMJ) )
          假如有A、B两张表进行排序合并连接，ORACLE会首先将A表进行排序，形成一张临时的“表”C，然后将B进行排序，形成一张临时的“表”D，然后将C与D进行合并操作，返回结果集。


​    如果从预获取的数据量的角度而言，如果B表参与计算的数据量比较小的话，则嵌套循环连接的效率就是比较高的，因为可以很少的IO就可以获取到最终的结果集。但是如果数据量比较大的话，hash join和sort merge join是比较有优势的。


​    如果从索引的角度而言，索引可以提高nested loops的效率，因为从B表获取数据进行操作，就类似于从单表中查询数据一样，table access full和by index的效率肯定是不一样的，但是这个也取决于B的参与计算的数据量，如果B表的数据都在可以被一次抓取的数据块的大小之内的话，那么索引未必会被使用到。


如果从内存的角度上，同样的数据量nested loops的内存占用应该是最小的，sort merge 应该是最大的，而hash join内存消耗在中间。只是一种感官的直觉，具体没有测试过，因为sort merge 需要创建两个排序表，而hash join则需要对B表创建一棵查询树。

​     怎么从hash的角度上来看呢？估计三种表都有hash的使用，使用hash更多的是为了提高查询的效率，比如8=power（2,3），如果使用hash，可能需要创建一棵hash树，就增大了空间的消耗，如果table access full的话，需要最少扫描1次，最多扫描8次。如果使用hash，则最少1次，最多3次，就可以了，使用空间获取时间上的优势。在这个里面，至少感觉到使用到hash的有nested loops中的索引和hash join。

- 4） Cartesian product（笛卡尔积）

  表的每一行依次与另外一表的所有行匹配，一般情况下，尽量避免使用。

![img](https://gitee.com/ZXiangC/picture/raw/master/img/366989-20151008161145128-953950880.png)

### 5、查询当前执行的sql语句

```sql
SELECT b.sid oracleID,
       b.username 登录Oracle用户名,
       b.serial#,
       spid 操作系统ID,
       paddr,
       sql_text 正在执行的SQL,
       b.machine 计算机名
FROM v$process a, v$session b, v$sqlarea c
WHERE a.addr = b.paddr
   AND b.sql_hash_value = c.hash_value
```

