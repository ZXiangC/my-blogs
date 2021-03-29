> 合并kettle 步骤
>
> 1 查看个数是否一样。
>
> 2 查看个数中映射是否一样。



### (1) tdcktl_am_xj_ktl

![image-20210325143820370](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325143820370.png)

​	

![image-20210329092958054](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210329092958054.png)

###  (2) tdc_amktl_innate 作业

#### 1 新增两个转换

![image-20210325143906292](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325143906292.png)

#### 2   v_ts2am_pactvary_innate

- v_ts2am_pactvary_innate
- tdc_am_pactvary_innate

![image-20210325145516302](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325145516302.png)



![image-20210325145440039](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325145440039.png)

#### 3  v_ts2am_guarantee_innate_xt

- v_ts2am_guarantee_innate_xt
- am_guarantee_innate

![image-20210325145950209](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325145950209.png)

![image-20210325145440039](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325145440039.png)



#### 4  v_ts2am_order_innate_xt

- v_ts2am_order_innate_xt

- am_order_innate

  ![image-20210325151810828](C:/Users/chenzx31065/AppData/Roaming/Typora/typora-user-images/image-20210325151810828.png)

![image-20210325151534646](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325151534646.png)

### (3) tdc_faktl_innate

- 最新



![image-20210326154327906](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210326154327906.png)

- 历史

![image-20210325155722151](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325155722151.png)

#### 1 v_ts2fa_fundinfo_innate_xt

- v_ts2fa_fundinfo_innate_xt
- fa_fundinfo_innate

![image-20210325153048755](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325153048755.png)

![image-20210325145440039](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325145440039.png)



#### 2 v_ts2fa_subject_innate_xt

- v_ts2fa_subject_innate_xt
- fa_subject_innate

![image-20210325153341830](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325153341830.png)

### (4) tdc_topktl 

![image-20210325160737179](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325160737179.png)



![image-20210325160917966](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325160917966.png)



```sql
insert into PM_PROJECTEX(C_PROJCODE,C_RIVALNAME,L_TIMELIMIT)
select 
  t.C_PROJCODE,
  t.C_RIVALNAME,
  t.L_TIMELIMIT -- number(8) 测试数据里面有不合法值
from
hsstg.V_TS2PM_PROJECTEX_XT t
```

### (5) tdc_amktl

![image-20210325164827076](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325164827076.png)

- v_ts2am_guarpact_xt

- am_guarpactam_guarpact
- 全映射 时候有问题。

![image-20210325164226001](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325164226001.png)



### (6)  tdc_tcmpktl

![image-20210325174719714](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210325174719714.png)



### (7) tdc_taktl



![image-20210326094658845](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210326094658845.png)

### (8) tdc_faktl

![image-20210326110304221](https://gitee.com/ZXiangC/picture/raw/master/img/image-20210326110304221.png)

