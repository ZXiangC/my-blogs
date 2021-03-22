### 1 从Excle 文件中读取数据

```java
public static void main(String[] args) throws Exception {

        // 1 获取工作薄
        XSSFWorkbook xssfWorkbook = new XSSFWorkbook("E:\\demo.xlsx");

        // 2 获取sheet 页
        XSSFSheet sheet = xssfWorkbook.getSheetAt(0);

        // 3 获取行(通过增强for 循环做)
//        for (Row row : sheet) {
//            // 获取行中每一列的值
//            for (Cell cell : row) {
//                String value = cell.getStringCellValue();
//                System.out.println(value);
//            }
//        }

        // 3 获取行 (由索引操作)
        for (int i = 0; i <= sheet.getLastRowNum(); i++) {
            XSSFRow row = sheet.getRow(i);
            if (row != null) {
                for (int j = 0; j <= row.getLastCellNum(); j++) {
                    XSSFCell cell = row.getCell(j);
                    if(cell!=null){
                        String value = cell.getStringCellValue();
                        System.out.println(value);
                    }
                }
            }

        }

        xssfWorkbook.close();
    }
```

### 2 向Excel 中写入数据

```java
 public static void main(String[] args) throws IOException {
        XSSFWorkbook xssfWorkbook = new XSSFWorkbook();

        XSSFSheet sheet1 = xssfWorkbook.createSheet("表一");

        XSSFRow row = sheet1.createRow(0);

        row.createCell(0).setCellValue("Hello");
        row.createCell(1).setCellValue("world");

        XSSFRow row1 = sheet1.createRow(1);

        row1.createCell(0).setCellValue("关羽");
        row1.createCell(1).setCellValue("张飞");

        FileOutputStream fileOutputStream = new FileOutputStream("E:\\demo2.xlsx");

        xssfWorkbook.write(fileOutputStream);

        fileOutputStream.close();
        System.out.println("写入成功");
    }
```

