## 	vue 配置
* [笔记中展示vue](https://dream2023.github.io/docsify-plugin-run/#/?id=%f0%9f%8d%8f-%e4%bb%8b%e7%bb%8d)

### 测试

```html run
<template>
  <el-radio v-model="radio" label="1">备选项</el-radio>
  <el-radio v-model="radio" label="2">备选项</el-radio>
</template>

<script>
  export default {
    data () {
      return {
        radio: '1'
      };
    }
  }
</script>
```

