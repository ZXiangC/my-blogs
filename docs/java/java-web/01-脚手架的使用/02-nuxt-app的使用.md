## 一、github

[官网文档说明](https://nuxtjs.org/docs/2.x/get-started/installation)



## 二、初始化项目

https://www.cnblogs.com/bien94/p/12572527.html



## 三、plugins 插件的配置使用

### 1、幻灯片插件

-  **1、安装插件**

  ```js
  npm install vue-awesome-swiper
  ```

- **2、配置插件**

在 plugins 文件夹下新建文件 nuxt-swiper-plugin.js，内容是

```js
import Vue from 'vue'
import VueAwesomeSwiper from 'vue-awesome-swiper/dist/ssr'

Vue.use(VueAwesomeSwiper)
```

- **3 在 nuxt.config.js 文件中配置插件**

将 plugins 和 css节点 复制到 module.exports节点下

```js
module.exports = {
  // some nuxt config...
  plugins: [
    { src: '~/plugins/nuxt-swiper-plugin.js', ssr: false }
  ],

  css: [
    'swiper/dist/css/swiper.css'
  ]
}
```

### 2、谷粒学院里面使用的插件

```js
npm install element-ui
npm install vue-qriously
npm install js-cookie
```

