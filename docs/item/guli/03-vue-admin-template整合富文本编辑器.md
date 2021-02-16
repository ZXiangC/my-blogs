









## 一 组件初始化

### 第一步：复制组件

- components :组件中多了一个文件
- static: 多了一个文件

![image-20210216221635669](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210216221635669.png)

### 第二步：添加配置

在 vue-admin-template/build/webpack.dev.conf.js 中添加配置

使在html页面中可是使用这里定义的BASE_URL变量

```js
new HtmlWebpackPlugin({
    ......,
    templateParameters: {
        BASE_URL: config.dev.assetsPublicPath + config.dev.assetsSubDirectory
    }
})
```

![image-20210216222059439](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210216222059439.png)

### 第三步: 引入js 脚本

在 vue-admin-template/index.html 中引入js脚本

```js
<script src=<%= BASE_URL %>/tinymce4.7.5/tinymce.min.js></script>
<script src=<%= BASE_URL %>/tinymce4.7.5/langs/zh_CN.js></script>
```

![image-20210216222723561](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210216222723561.png)

## 二 组件引入



### 第一步：引入组件

```js
import Tinymce from '@/components/Tinymce'

export default {
  components: { Tinymce },
  ......
}
```

![image-20210216225636934](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210216225636934.png)

### 第二步： 引入模板

```js
<!-- 课程简介-->
<el-form-item label="课程简介">
    <tinymce :height="300" v-model="courseInfo.description"/>
</el-form-item>
```

### 第三步：引入样式

```js
<style scoped>
.tinymce-container {
  line-height: 29px;
}
</style>
```

![image-20210216225843692](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210216225843692.png)