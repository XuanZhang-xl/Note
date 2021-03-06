## Git的使用命令

**尽量不要用IDE集成的git管理工具来操作,直接写git命令更佳,因为如果用git管理工具,那么换个工具就又要重新学过**  


- [IDE提交到git](#1)  
- [把当前目录变成git可管理的仓库](#2)  
- [把文件添加/提交到版本库(本地)](#3)  
- [查看状态](#4)  
- [版本回退](#5)  
- [checkout检出](#6)  
- [删除文件](#7)  
- [SSH/关联远程数据库](#8)  
- [同步远程](#9)  
- [分支](#10)  
- [标签管理](#11)  
- [自定义Git](#12)  

### <span id = '1'>IDE提交到git</span>
- VisualStudio提交到git
    - 先到源代码管理,点击那个小勾(commit),此时右下角那个循环标志会变成上下两个箭头,点击两个小箭头就可以完成上传
    - 严格上来说 vscode 并没有自带 Git , 而是自带了使用 Git 的功能。因为 vscode 使用的是你本地的 Git 。你的问题,**能不能合并,删除.答案是:不行的.**,要么用功能齐全的 SourceTree 之类的 GUI 软件，要么用命令。编辑器的 Git 功能，主要是方便提交代码用的。

### <span id = '2'>把当前目录变成git可管理的仓库</span>
- `git init` 瞬间Git就把仓库建好了,而且告诉你是一个空的仓库(empty Git repository),目录下多了一个.git的隐藏目录，这个目录是Git来跟踪管理版本库的,没事千万不要手动修改这个目录里面的文件.
- 创建Git版本库时，Git自动创建了唯一一个master分支

### <sapn id = '3'>把文件添加/提交到版本库(本地)</span>
- 步骤一: `git add 文件名1 文件名2 文件名3`
    - 将需要提交的文件/修改通通放到暂存区(Stage)
    - 将Untracked的文件变为Tracked
    - add后版本号不会变
- 步骤二: `git commit -m "注释内容"`
    - 一次性提交暂存区的所有修改到本地仓库。
    - commit后版本号改变
- 注意:
    - add可以一次add很多文件,文件名中间用空格隔开,commit一次性把所有add的文件提交.
    - 提交更新也是使用add

### <span id = '4'>查看状态</span>
- `git status` 查看当前仓库状态
- `git diff HEAD -- 文件名` 查看这个文件和仓库中的有什么变化,建议每次提交前都先使用此命令查看
- `git log` 显示从最近到最远的**提交**日志
    - `git log --pretty=oneline` 一般使用这个命令查看日志,简介清晰,但是没有时间
    - log命令只显示到当前HEAD所在的版本
    - `git log --graph --pretty=oneline --abbrev-commit` 这个命令可以查看分支/合并分支的日志
    - `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"` 查看日志最终形态,只要输入`git lg`就好.
- `git branch` 列出所有分支,前面带*表明是当前分支
- `git remote` 查看远程仓库的信息.
- `git remote -v` 查看远程仓库详细信息,如果没有推送权限,就看不到push的地址
- `git tag` 查看所有标签,按字母排序
- `git show 标签名` 查看标签详细信息

### <span id = '5'>版本回退</span>
**----------->请先保存当前版本再进行以下操作,否则出事概不负责!<------------**  
**----------->请先保存当前版本再进行以下操作,否则出事概不负责!<------------**  
**----------->请先保存当前版本再进行以下操作,否则出事概不负责!<------------**  
**----------->版本回退后,Ctrl+z无效!!!!!!!!!!!!!!!!!!!!!!!!!<------------**  
**----------->版本回退后,Ctrl+z无效!!!!!!!!!!!!!!!!!!!!!!!!!<------------**  
**----------->版本回退后,Ctrl+z无效!!!!!!!!!!!!!!!!!!!!!!!!!<------------**  


- `git reset --hard HEAD^` 回退到上一个版本,如果回退到上上个版本,则写HEAD^^
- `git reset --hard HEAD~100` 回退到100个版本以前

- `git reset --hard 版本号` 到指定版本号的版本,版本号没有必要写全,git会自动去找
- 如果log中找不到想要的版本号(版本在当前HEAD之前),则使用命令:
    - `git reflog` 记录le每一次命令,并且有每个命令的版本号的前7位,获取到版本号前7位后使用reset来进行版本设置.
- `git reset HEAD file` 可以把暂存区的修改撤销掉(unstage),也就是可以取消add命令的效果

### <span id = '6'>checkout检出</sapn>
**----------->请先保存当前版本再进行以下操作,否则出事概不负责!<------------**  
**----------->请先保存当前版本再进行以下操作,否则出事概不负责!<------------**  
**----------->请先保存当前版本再进行以下操作,否则出事概不负责!<------------**  



- `git checkout -- 文件名` 分两种情况:
    - 一种是文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态.
    - 一种是文件已经添加到暂存区后,又作了修改,**撤销修改就回到添加到暂存区后的状态**,如果要丢弃工作区的修改,先使用`git reset HEAD file`清空暂存区,再使用`git checkout -- 文件名`.
    - 总之,就是让这个文件回到最近一次git commit或git add时的状态  
**注意:上面的checkout有--,下面的没有**  
- `git checkout 分支` 请看[分支](#10)

### <span id = '7'>删除文件</span>
- 如果是手动删除或`rm`命令删除,则有两个选择
    - 如果确实要删掉: `git rm 文件名` 并且提交: `git commit -m '删除了文件'`
    - 如果是误删除了: `git checkout -- 文件名` 则检出文件就可以了



### <span id = '8'>SSH/关联远程数据库</span>
- ssh-keygen -t rsa -C "邮箱地址" 然后一路回车即可.在用户主目录下,会生成.ssh目录,里面有id_rsa(私钥)和id_rsa.pub(公钥)这两个文件,然后请参考[ssh攻略](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000)
- **`git remote add 命名远程仓库 远程仓库地址`**    如:
    - `git remote add origin https://github.com/XuanZhang-xl/Note.git`  远程仓库命名为orgin
    - **问:https://github.com/XuanZhang-xl/Note.git与git@github.com:XuanZhang-xl/Note.git有什么区别?**
    - **答:Git支持多种协议，默认的git://使用ssh,但也可以使用https等其他协议,使用https除了速度慢以外,还有个最大的麻烦是每次推送都必须输入口令,但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https.**

### <span id = '9'>同步远程</span>
- `git push -u 本地仓库名 远程仓库名` 把本地仓库推送到远程,如:
    - `git push -u origin master` 实际上是把当前分支master推送到远程.
    - 加-u参数是第一次提交,Git不但会把本地的master分支内容推送的远程新的master分支,还会把本地的master分支和远程的master分支关联起来,以后提交就都可以把-u省去.
    - **`git push origin master`**
- `git clone 远程仓库` 克隆一个远程仓库到本地
- **问:要是本地的仓库本来就有东西,会变成怎么样?**
- **答:自己测**


### <span id = '10'>分支</span>
- `git checkout -b 分支名` 创建并切换到新建的分支,相当于下边两条语句:
    - `git branch 分支名`    创建新分支
    - `git checkout 分支名`  切换到新分支
- `git branch` 列出所有分支,前面带*表明是当前分支
- `git merge` 命令用于合并指定分支到当前分支,这种合并是“快进模式(Fast-forward)”，也就是直接把master指向dev的当前提交，所以合并速度非常快。
    - 如果合并的两个分支的版本号不一致,且都做了修改,那么就有可能产生冲突(conflict),git会把冲突的代码都列出来,供我们解决冲突.解决完后,**需要再次add与commit**才能结束此次合并.
- `git branch -d 分支名` 删除分支
- `git branch -D 分支名` 强行删除分支
- `git merge --no-ff -m "merge with no-ff" 分支名` 不用快进模式进行合并.
    - 加上--no-ff参数就可以用普通模式合并,合并后的历史有分支,能看出来曾经做过合并,而fast forward合并就看不出来曾经做过合并.
- 分支策略
    - master分支应该是非常稳定的,也就是仅用来发布新版本,平时不能在上面干活.
    - 干活都在dev分支上,也就是说,dev分支是不稳定的,到某个时候,比如1.0版本发布时,再把dev分支合并到master上,在master分支发布1.0版本.
    - 你和你的小伙伴们每个人都在dev分支上干活,每个人都有自己的分支,时不时地往dev分支上合并就可以了.
    ![分支策略](pic/gitbranchstrategy.png)
- bug分支,每个bug都可以通过一个新的临时分支来修复,修复后,合并分支,然后将临时分支删除,通常命名为`issue-bug代号`
    - **`git stash`** 如果工作到一半,不能继续做,也不想提交,则使用此命令吧当前代码存储起来,那么当前工作区会回到这个版本的初始状态
    - `git stash list` 查看有没有保存的工作.
    - `git stash apply` 恢复之前的工作,但是stash内的内容并不删除,通过`git stash drop`来删除
    - **`git stash pop`** 恢复之前的工作并删除stash
    - 可以多次使用stash,恢复的时候，先用`git stash list`查看，然后使用命令`git stash apply stash@{0}`恢复指定的stash.
- feature分支,每添加一个新功能,最好新建一个feature分支,在上面开发,完成后,合并,最后删除该feature分支.

### <span id = '11'>标签管理</span>
- 发布一个版本时,我们通常先在版本库中打一个标签(tag),这样,就唯一确定了打标签时刻的版本.将来无论什么时候,取某个标签的版本,就是把那个打标签的时刻的历史版本取出来.所以,标签也是版本库的一个快照,tag就是一个让人容易记住的有意义的名字,**它跟某个commit绑在一起**.
- 标签一般存储在本地的,一个版本可以打多个标签
- `git tag` 查看标签
- `git show 标签名` 查看某标签详细信息
- `git tag v1.0` 打一个v1.0的新标签
- `git tag 标签名 版本号` 对某一版本打标签
- `git tag -a v0.1 -m "version 0.1 released" 3628164` 对指定版本创建带有说明的标签,用-a指定标签名, -m指定说明文字.
- `git tag -d 标签名` 删除标签
- 远程标签的操作
    - `git push origin v1.0` 推送v1.0版本到远程
    - `git push origin --tags` 一次性推送全部尚未推送到远程的本地标签
    - `删除远程标签`
        - `git tag -d v0.9` 先删除本地标签
        - `git push origin :refs/tags/v0.9` 固定格式就是这样

### <span id = '12'>自定义Git</span>
- `git config --global color.ui true` 让git显示颜色
    - --global参数是全局参数,也就是这些命令在这台电脑的所有Git仓库下都有用.
- .gitignore
    - 如果想添加一个文件到Git,但发现添加不了,原因很有可能是这个文件被.gitignore忽略了.
    - 如果确实想添加这个文件,`git add -f 文件名` -f表示强制添加
    - 如果觉得是.gitignore的问题,`git check-ignore -v 文件名` 检查到底是哪条.gitignore阻止了文件的添加
    - [.gitignore配置大本营](https://github.com/github/gitignore),直接copy里面合适的文件就可以了.
- 配置命令的别名
    - `git config --global alias.st status` 使用st代替status,以后只要写`git st`就可以代表`git status`
    - `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"` 查看日志最终形态,只要输入`git lg`就好.

- 配置文件
    - 配置Git的时候，加上--global是针对当前用户起作用的,如果不加,那只针对当前的仓库起作用,每个仓库的Git配置文件都放在.git/config文件中. 别名就在[alias]后面,要删除别名,直接把对应的行删掉即可.
    - 当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中,修改同上.