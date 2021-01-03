## 1 markdown

### 1.1 自定义模板快捷键
- 文件->首选项->用户片段->搜索markdown.json 

```json
{
    "java code": {
        "prefix": "java",
        "body": [
            "```java",
            "$1",
            "```"
        ],
        "description": "print java code"
	},
	
	"table code": {
        "prefix": "table",
        "body": [
            "|  表头   | 表头  | 表头 | 表头  |",
            "|  ----  | ----  |----  |----  |",
            "| 单元格  | 单元格 |单元格 |单元格 |",
            "| 单元格  | 单元格 |单元格 |单元格 |",
            "| 单元格  | 单元格 |单元格 |单元格 |"
        ],
        
        "description": "table "
    },
    "table code3": {
        "prefix": "table3",
        "body": [
            "|  表头   | 表头  | 表头  ",
            "|  ----  | ----  |----  |",
            "| 单元格  | 单元格 |单元格 |",
            "| 单元格  | 单元格 |单元格 |",
            "| 单元格  | 单元格 |单元格 |"
        ],
        "description": "table "
    },
    "table code4": {
        "prefix": "table4",
        "body": [
		   "|  表头   | 表头  | 表头 | 表头  |",
		   "|  ----  | ----  |----  |----  |",
		   "| 单元格  | 单元格 |单元格 |单元格 |",
           "| 单元格  | 单元格 |单元格 |单元格 |",
           "| 单元格  | 单元格 |单元格 |单元格 |",
           "| 单元格  | 单元格 |单元格 |单元格 |"
        ],
        "description": "table "
    },
    
    // 图片快捷键
	"picture code": {
        "prefix": "pic",
        "body": [
            "![]()",
            "$1",
        ],
        "description": "table "
    },
    "link code": {
        "prefix": "link",
        "body": [
            "[]()",
            "$1",
        ],
        "description": "table "
    }
}
```
### 1.2 vsCode 插件 

#### 1.2.1 Markdown Preview Enhanced
- **简介**: 用于编辑


![预览图片插件](http://cdn.littlegenius.xin/%E4%BD%BF%E7%94%A8vscode%E7%BC%96%E5%86%99markdown%E7%9A%84%E4%B8%80%E4%BA%9B%E5%B0%8F%E6%8F%92%E4%BB%B64.png)

#### 1.2.2 Maridown pdf
![将文档转为pdf插件](http://cdn.littlegenius.xin/%E4%BD%BF%E7%94%A8vscode%E7%BC%96%E5%86%99markdown%E7%9A%84%E4%B8%80%E4%BA%9B%E5%B0%8F%E6%8F%92%E4%BB%B68.png)

#### 1.2.3 markdown-all-in-one
- **简介**：所有你需要写Markdown要用到的（键盘快捷方式，目录，自动预览等）
- **配置**：这个需要配置的不多，基本上默认就好了

#### 1.2.4 markdown toc
- **简介**: 用于生成目录。

#### 1.2.5 markdownlint
- **简介**：用于校验语法
  
#### 1.2.6 Markdown Shortcuts
- **简介**: 这个插件就很强大了，有时候从表里复制过来数据，总不能一个一个的|加吧，所以这个插件帮我们解决复制表格的问题

## 2 docsify 搭建个人博客
### 2.1 环境准备
- (1)安装node 环境
- (2)全局安装 docsify 脚手架
```java
npm i docsify-cli -g
```
### 2.2 使用命令创建工程    
- >docsify init 工程名
- >docsify serve 工程名 启动项目

### 2.3 index.htm 配置工程(直接配置一次复制粘贴就好)

> 可以配置文件搜索，代码高亮，展示层级，项目名称等.... 只要配置都可以找他。
```html
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>docsify-demo</title>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="description" content="Description">
  <meta name="viewport"
    content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <link rel="stylesheet" href="//unpkg.com/docsify/lib/themes/vue.css">
</head>

<body>
  <div id="app"></div>
  <!-- docsify-edit-on-github -->
  <script src="//unpkg.com/docsify-edit-on-github/index.js"></script>
  <script>
    window.$docsify = {
      name: 'tools',
      repo: '',
      maxLevel: 5,//最大支持渲染的标题层级
      subMaxLevel: 3,
      homepage: 'README.md',
      coverpage: true,//配置展示封面
      loadSidebar: true,//配置展示侧边栏
      auto2top: true,//切换页面后是否自动跳转到页面顶部
      //全文搜索
      search: {
        //maxAge: 86400000, // 过期时间，单位毫秒，默认一天
        paths: 'auto',
        placeholder: '搜索',
        noData: '找不到结果',
        // 搜索标题的最大程级, 1 - 6
        depth: 3,
      },
      plugins: [
        EditOnGithubPlugin.create('https://github.com/Snailclimb/JavaGuide-Interview/blob/master/')
      ],
    }
  </script>
  <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
  <!--代码高亮-->
  <script src="//unpkg.com/prismjs/components/prism-java.js"></script>
  <script src="//unpkg.com/prismjs/components/prism-json.js"></script>
   <!--全文搜索,直接用官方提供的无法生效-->
   <script src="https://cdn.bootcss.com/docsify/4.5.9/plugins/search.min.js"></script>
   <!-- 复制到剪贴板 -->
   <script src="//unpkg.com/docsify-copy-code"></script>
   <!-- 图片缩放 -->
   <script src="//unpkg.com/docsify/lib/plugins/zoom-image.js"></script>
   <!-- 字数统计 -->
   <script src="//unpkg.com/docsify-count/dist/countable.js"></script>
</body>
</html>
```
### 2.4 封面 _coverpage.md
```html
<p align="center">
<img src="https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=2481424715,2807309609&fm=26&gp=0.jpg" width="200" height="200"/>
</p>
<h1 align="center">docsify-demo</h1>
```
### 2.5 侧边栏 _sidebar.md 
``` markdown

* [test](hello/test.md)

```
