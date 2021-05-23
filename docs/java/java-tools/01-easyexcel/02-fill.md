### 1 简单填充

- 填充时候可以选择填充map  or  pojo

```java
    @Test
    public void simpleFill_01() {
        // 1 定义，并读取模板
        String excelTemplate = ClassUtils.getDefaultClassLoader().getResource("static").getPath() + "/01-fill-simple-template.xlsx";
        // 2 获取目标Excel路径
        String fileName = System.getProperty("user.dir") + "\\src\\main\\resources\\static\\01-fill-simple-data.xlsx";
        /**
         *  3 准备数据
         */
        // 方案1 根据pojo填充
        /*FillData fillData = new FillData();
        fillData.setName("张三");
        fillData.setNumber(5.2);
        EasyExcel.write(fileName).withTemplate(excelTemplate).sheet().doFill(fillData);*/


        // 方案2 根据Map填充
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("name", "张三");
        map.put("number", 5.2);
        EasyExcel.write(fileName).withTemplate(excelTemplate).sheet().doFill(map);
        System.out.printf("填充成功");
    }
```

### 2 一次或者分为多次填充

```java
    @Test
    public void ListFill_02() {
        // 1 定义，并读取模板
        String excelTemplate = ClassUtils.getDefaultClassLoader().getResource("static").getPath() + "/02-fill-list-template.xlsx";
        // 2 获取目标Excel路径
        String fileName = System.getProperty("user.dir") + "\\src\\main\\resources\\static\\02-fill-list-data.xlsx";

        // 方案1 一下子全部放到内存里面 并填充
        // 这里 会填充到第一个sheet， 然后文件流会自动关闭
        /*EasyExcel.write(fileName).withTemplate(excelTemplate).sheet().doFill(initData());
        System.out.printf("填充成功");*/

        // 方案2 分多次 填充 会使用文件缓存（省内存）
        ExcelWriter excelWriter = EasyExcel.write(fileName).withTemplate(excelTemplate).build();
        WriteSheet writeSheet = EasyExcel.writerSheet().build();
        excelWriter.fill(initData(), writeSheet);
        // 千万别忘记关闭流
        excelWriter.finish();
        System.out.printf("填充成功");
    }
```

### 3  pojo 和map 一起填充

```java
    @Test
    public void ListFill_0102() {
        // 1 定义，并读取模板
        String excelTemplate = ClassUtils.getDefaultClassLoader().getResource("static").getPath() + "/03-fill-0102_template .xlsx";
        // 2 获取目标Excel路径
        String fileName = System.getProperty("user.dir") + "\\src\\main\\resources\\static\\03-fill-0102_data.xlsx";

        ExcelWriter excelWriter = EasyExcel.write(fileName).withTemplate(excelTemplate).build();
        WriteSheet writeSheet = EasyExcel.writerSheet().build();
        // 这里注意 入参用了forceNewRow 代表在写入list的时候不管list下面有没有空行 都会创建一行，然后下面的数据往后移动。默认 是false，会直接使用下一行，如果没有则创建。
        // forceNewRow 如果设置了true,有个缺点 就是他会把所有的数据都放到内存了，所以慎用
        // 简单的说 如果你的模板有list,且list不是最后一行，下面还有数据需要填充 就必须设置 forceNewRow=true 但是这个就会把所有数据放到内存 会很耗内存
        // 如果数据量大 list不是最后一行 参照下一个
        FillConfig fillConfig = FillConfig.builder().forceNewRow(Boolean.TRUE).build();
        excelWriter.fill(initData(), fillConfig, writeSheet);

        Map<String, Object> map = new HashMap<String, Object>();
        map.put("date", "2019年10月9日13:28:28");
        map.put("total", 1000);
        excelWriter.fill(map, writeSheet);
        excelWriter.finish();
        System.out.printf("填充成功");
    }
```

### 4 横向填充

```java
@Test
    public void RowFill() {
        // 1 定义，并读取模板
        String excelTemplate = ClassUtils.getDefaultClassLoader().getResource("static").getPath() + "/04-fill-rowFill_template.xlsx";
        // 2 获取目标Excel路径
        String fileName = System.getProperty("user.dir") + "\\src\\main\\resources\\static\\04-fill-rowFill_data.xlsx";

        ExcelWriter excelWriter = EasyExcel.write(fileName).withTemplate(excelTemplate).build();
        WriteSheet writeSheet = EasyExcel.writerSheet().build();
        FillConfig fillConfig = FillConfig.builder().direction(WriteDirectionEnum.HORIZONTAL).build();
        excelWriter.fill(initData(), fillConfig, writeSheet);

        Map<String, Object> map = new HashMap<String, Object>();
        map.put("date", "2019年10月9日13:28:28");
        excelWriter.fill(map, writeSheet);
        // 别忘记关闭流
        excelWriter.finish();
        System.out.printf("填充成功");
    }
```

### 5 多数据源填充

```java
@Test
    public void complexFill() {
        // 1 定义，并读取模板
        String excelTemplate = ClassUtils.getDefaultClassLoader().getResource("static").getPath() + "/05-fill-complexFill_template.xlsx";
        // 2 获取目标Excel路径
        String fileName = System.getProperty("user.dir") + "\\src\\main\\resources\\static\\05-fill-complexFill_data.xlsx";

        ExcelWriter excelWriter = EasyExcel.write(fileName).withTemplate(excelTemplate).build();
        WriteSheet writeSheet = EasyExcel.writerSheet().build();
        FillConfig fillConfig = FillConfig.builder().direction(WriteDirectionEnum.HORIZONTAL).build();
        // 如果有多个list 模板上必须有{前缀.} 这里的前缀就是 data1，然后多个list必须用 FillWrapper包裹
        excelWriter.fill(new FillWrapper("data1", initData()), fillConfig, writeSheet);
        excelWriter.fill(new FillWrapper("data2", initData()), writeSheet);
        excelWriter.fill(new FillWrapper("data3", initData()), writeSheet);
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("date", "2019年10月9日13:28:28");
        excelWriter.fill(map, writeSheet);
        // 别忘记关闭流
        excelWriter.finish();
        System.out.printf("填充成功");
    }
```

### 总结

1.1 填充需要准备三个东西

- (1) 模板
- (2) 目标文件
- (3) 数据