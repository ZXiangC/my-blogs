

### StringUtils

- jion

```sql
  public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("11");
        list.add("22");
        list.add("33");
        // 11,22,33
        String join = StringUtils.join(list.toArray(), ","); // import org.apache.commons.lang3.StringUtils;
        System.out.println(join);// 11,22,33
    }
```

