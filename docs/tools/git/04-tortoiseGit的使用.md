## 01、stash暂存代码

### 1、暂存代码场景

**场景一** 
user0 有新提交
user1 没有pull -> 写新代码 -> pull -> 提示有冲突

解决办法
-> stash save(把自己的代码隐藏存起来) -> 重新pull -> stash pop(把存起来的隐藏的代码取回来 ) -> 代码文件会显示冲突 -> 右键选择edit conficts，解决后点击编辑页面的 mark as resolved ->  commit&push

**场景二**
user0 有新提交
user1 没有pull -> 写新代码 -> commit&push -> 提示有冲突

解决办法一
-> pull -> 代码文件会显示冲突 -> 右键选择edit conficts，解决后点击编辑页面的 mark as resolved ->  commit&push

### 2、参考链接

[暂存代码](https://jingyan.baidu.com/article/636f38bb8fab39d6b846109f.html)