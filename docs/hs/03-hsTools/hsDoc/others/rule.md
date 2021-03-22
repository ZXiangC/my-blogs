?
> 最开始可以参考项目路径下已经写好的 TUSP4.0V202001A\schema\dap-tusp\23.SRDT\01.通用必打脚本\其他通用脚本

## 1 ruleid命名规范(以A010101为例)
- A  : 表示那种类型表
- 01 : A 类型的第一张表
- 01 : A 类型第一张表的第一个字段
- 01 : A 类型第一张表的第一个字段的第一个校验规则。


## 2 校验数据的主要类型
###  2.1 字典类型 
- 转换函数

```sql
-- 枚举（单选）
fc_IsInDictItem(''GBT2659'',t.c_ssgb)=''N''  -- 一般国家使用这个函数
fc_IsIntreeDictItem(''GBT2260_2018'',t.c_ssdq)=''N'' -- 一般地区使用这个函数
-- 枚举（复选）
fn_arrayIndict(t.c_gllx,'','',''srdt_2015'')=''N''
```

- 数据字典

> Excle表中数据字典和我们需要使用的字典可能不一样，主要有如下三种

- Excel 中数据字典编号为 1003 对应我们实际使用的 GBT2659  （字典中存的主要是国家）
- Excel 中数据字典编号为 1001 对应我们实际使用的 GBT2260_2018  （字典中存的主要是中国地区）
- Excel 中数据字典编号为 1001 和1003 对应我们实际使用的 GBT2260_T2659  （国家地区都有）

> 一般的字典，就是srdt_前缀，加上文档的字典编号 例如：'srdt_3001'   在Excel的字典编号是 3001


```sql
-- 查看字典表中数据
select * from tsys_dict_item t where t.dict_entry_code = 'GBT2659'          -- 国家  预览表1003
select * from tsys_dict_tree tt where tt.c_dict_entry_code = 'GBT2260_2018';  -- 只需要地区

select * from tsys_dict_tree t where t.c_dict_entry_code = 'GBT2260_T2659'  -- 国家和地区都有 

select * from tsys_dict_item t where t.dict_entry_code = 'srdt_3001'
```
### 2.2 日期类型 

> 日期类型一般只需要复制，然后改字段名称就可以


### 2.3 年份
```sql
--  表示出生日期在19**年到20**年 其他年份都要么没有出生，要么已经去世，即为不合法数据
regexp_like(t.c_csnf,''^(19|20)[0-9]{2}$'')
```
### 2.4 Excel备注要求

![图片](https://img-blog.csdnimg.cn/20200714205122642.png)

```sql
select t.C_ID c_tablepk,
       ''【c_rzrq入职日期：'' || t.c_rzrq || ''】员工信息表中员工状态为''''在职、停职、退休''''的入职日期不能为空'' c_errorvalue
  from SRDT_YGXXB t
 where t.C_YGZT  in (''2'',''4'',''5'') 
   and t.c_rzrq  is  null
   and t.c_rowflag in (''I'', ''U'', ''D'')
union all
select t.C_ID c_tablepk,
       ''【c_rzrq入职日期：当前长度为'' ||lengthb(t.c_rzrq)|| ''】员工信息表中的入职日期，日期长度不为8'' c_errorvalue
  from SRDT_YGXXB t
 where lengthb(t.c_rzrq)!=8
   and t.c_rzrq is not null
   and t.c_rowflag in (''I'', ''U'', ''D'')
union all
select t.C_ID c_tablepk,
       ''【c_rzrq入职日期：'' || t.c_rzrq || ''】员工信息表中的入职日期，日期不是日期格式数据'' c_errorvalue
  from SRDT_YGXXB t
 where fn_bas_isdate(t.c_rzrq)=''N''
   and t.c_rzrq is not null
   and t.c_rowflag in (''I'', ''U'', ''D'')
```

### 2.5 字符类型
- 这个是最为常见的类型一般只需要判断是否为空，字符长度是否合法就可以。