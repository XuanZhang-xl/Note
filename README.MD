# Note

嘛,还是用git写笔记最好了

添加 .gitignore文件

.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交.

更新整个.git使.gitignore生效

使用原生git语句是简单,然而使用VisualStudio,SublimeText,WebStorm等工具来提交确实不知道该怎么弄,也没时间一个一个研究过来,就在这儿把git语句总结下,以后都用语句提交好了.
