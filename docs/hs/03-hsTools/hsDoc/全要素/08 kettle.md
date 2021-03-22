### 一 全要素 kettle 位置

- kettle  应该放在什么位置。
- kettle  如何配置到前台页面。

#### 1 kettle 配置表结构

- tdatacenter_single_table

![image-20210112172044541](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210112172044541.png)

#### 2 监管AMD 采集

- 执行的语句

```sql
select t.c_taskcode,
       t.c_taskname,
       (select r.user_name from tsys_user r where r.user_id = t.c_creator) c_creator,
       t.d_creatdate,
       (select r.user_name from tsys_user r where r.user_id = t.c_transuser) c_transuser,
       t.l_state,
       decode(t.l_state, '4', '<font color="red">转换失败', '转换成功') l_state_name,
       to_char(t.d_zxdate, 'YYYY-MM-DD HH24:mi:SS') d_zxdate,
       to_char(t.d_begdate, 'YYYY-MM-DD HH24:mi:SS') d_begdate,
       to_char(t.d_enddate, 'YYYY-MM-DD HH24:mi:SS') d_enddate,
       decode(t.l_state, '4', '<font color="red">' || t.c_msg, t.c_msg) c_msg,
       t.c_taskpath,
       t.l_order,
       t.c_producttype,
       t.c_type,
       t.c_kind
  from tdatacenter_single_table t
 where t.c_producttype is not null
   and t.c_enable = '1'
   and t.C_type IN ('1', '2')
   and instr(t.c_producttype, '1') > 0 -- 产品来源 1/全要素 2/资金申报 3/合同登记 4/east 5/1104  6/east2.0 7/基础数据 9/信保基金 A/征信报文报送 B/广义信贷 C/新监管指标 如果是多个系统，可以采用#拼接
 order by t.l_order, t.c_taskcode
```





