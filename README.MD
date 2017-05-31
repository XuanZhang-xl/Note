# gitTest

嘛,还是用git写笔记最好了

添加 .gitignore文件

.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交.

更新整个.git使.gitignore生效

使用原生git语句是简单,然而使用VisualStudio,SublimeText,WebStorm等工具来提交确实不知道该怎么弄,也没时间一个一个研究过来,就在这儿把git语句总结下,以后都用语句提交好了.

## VisualStudio提交到git
- 先到源代码管理,点击那个小勾(commit),此时右下角那个循环标志会变成上下两个箭头,点击两个小箭头就可以完成上传

## 关联远程数据库,并命名为orgin
- git remote add 命名远程仓库 远程仓库地址
- git remote add origin https://github.com/XuanZhang-xl/Note.git


## 上传文件到本地
- git add readme.txt 
- git commit -m "branch test"

## 同步远程
- git push -u 本地仓库名 远程仓库名
- git push -u origin master
- 加-u参数是第一次提交,以后提交就都可以把-u省去

## 打分支
- git checkout -b dev 创建并切换到dev分支,相当于下边两条语句:
    - git branch dev    创建dev分支
    - git checkout dev  切换到dev分支
- git branch 查看当前分支,*表明是当前分支
- 


## 以后遇到就补充

