### Git提交报错解决

错误：

![截屏2020-09-0500.27.26](https://tva1.sinaimg.cn/large/007S8ZIlly1gif3298o4uj30ve088dow.jpg)

问题：

远程库与本地库不一致造成的，那么我们把远程库同步到本地库就可以了。

输入命令：

```js
git pull --rebase origin master
```

这条指令的意思是把远程库中的更新合并到本地库中，–rebase的作用是取消掉本地库中刚刚的commit，并把他们接到更新后的版本库之中。



> 参考：https://blog.csdn.net/MBuger/article/details/70197532





### gitbook的安装和使用

https://www.ly-blog.top/2019/09/21/gitbook/