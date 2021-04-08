# git 笔记

## 1. 基础

### 1.1 命令

> 初始化git--->bash here-->输入:

> > ````sh
> > git init
> > ````

> 添加 到暂存区
>
> > ```` sh
> > git add <fileName>  
> > ````
>
> > ````sh
> > git add .    					 .  表示全部
> > ````

> 提交 到本地版本库
>
> > ```sh
> > git commit -m "提交的信息"
> > ```

> 撤销修改->仅保存,未添加到暂存区,
>
> > ```sh
> > git restore <fileName>
> > ```
> >
> > > 回到上次版本库的状态

> 撤销修改->已添加到暂存区
>
> > ``` sh
> > git restore --staged <fileName> 
> > ```
> >
> > > 将暂存区的修改回退到工作区,变为未添加状态
> > >
> > > > 再次执行第一步,即可回退到上个版本库版本

> 在工作区目录下直接删除文件.git status 查看状态后
>
> > ``` sh
> > git rm <fileName>   			删掉文件
> > ```
> >
> > ``` sh
> > git restore <fileName> 	  		撤销删除
> > ```

>  推送到远程
>
> > ```sh
> > git push origin master   //推送 当前分支 到 远程的master分支 
> > ```
>
> > 本地创建git项目(需先关联远程库),第一次推送需要加 -u   
> >
> > > ```sh
> > > git push -u origin master
> > > ```

> 拉取
>
> > ```sh
> > git pull origin master   //拉取 远程的master分支,到当前分支
> > ```

### 1.2 git工作空间结构

![git提交流程图-本地](gitNotePicture\git提交流程图-本地.jpg)

![git提交成功状态](gitNotePicture\git提交成功状态.jpg)

## 2. 远程仓库

### 2.1 生成sshkey

> ```sh
> ssh-keygen -t rsa -C "youremail@example.com"
> ```

> > 生成文件.../用户/yourUserName/.ssh/下的
> >
> > id_rsa  和 id_rsa.pub
> >
> > > 在github上的用户.setting.sshset添加公钥

### 2.2 关联远程库

> 本地项目->远程库
>
> > ```
> > git remote add origin <project.ssh>
> > git remote add origin <project.https>需要sign in
> > ```

> 远程库-> 本地项目
>
> > 现在远程库创建带有readme.md的文本.
> >
> > 直接clone到本地

## 3. 分支管理

### 3.1 命令

> 查看分支列表
>
> > ```sh
> > git branch 
> > ```

> 选择分支
>
> > ```sh
> > git switch <branchName>
> > ```
> >
> > ```sh
> > git checkout <branchName>
> > ```

> 创建分支
>
> > ```sh
> > git branch <branchName>
> > ```

> 创建并选择分支
>
> > ```sh
> > git switch **-c** <branchName>
> > ```
> >
> > ```sh
> > git checkout **-b** <branchName>
> > ```

> 合并分支
>
> > ```sh
> > git merge <branchName>    //在低版本的分支上 拉取高版本分支
> > ```
> > ```sh
> > git merge --no-ff -m "信息" <branchName>		
> > //禁用Fast forward,可保留合并信息,具体流程看图
> > ```

> 删除分支
>
> > ``` sh
> > git branch **-d** <branchName>
> > ```
>
> 强制删除
>
> > ```sh
> > git branch **-D** <branchName>
> > ```

> 查看日志
>
> ```sh
> git log --graph --pretty=oneline --abbrev-commit
> ```

> 保存现场,去改bug
>
> > ``` sh
> > git stash
> > ```
>
> 查看保存的现场
>
> > ``` 
> > git stash list
> > ```
>
> 恢复现场
>
> > ```sh
> > git stash pop
> > ```
> >
> > ```sh
> > git stash apply stash@{0}  	//恢复指定的版本
> > ```
>
> 同步指定提交(
>
> ​	在保存现场后,改完bug时,bug的提交记录一下commitId
>
> ​	这次提交可以给多个分支同步.
>
> ​	**保存现场的需要先同步,再恢复现场**
>
> )
>
> > ```sh
> > git cherry-pick 4c805e2
> > //4c805e2为改bug的提交时的commitId
> > ```

### 3.2 流程

> 刚开始

![1609731046(1)](gitNotePicture\1609731046(1).jpg)

> 创建分支 dev

![1609731157(1)](gitNotePicture\1609731157(1).jpg)

> 在dev分支commit了一次

![1609731224(1)](gitNotePicture\1609731224(1).jpg)

> 合并分支,fast forward合并

![1609731303(1)](gitNotePicture\1609731303(1).jpg)

> 合并分支,--no-ff 禁用Fast forward模式

![1609738610(1)](gitNotePicture\1609738610(1).jpg)

> 再将分支删除

![1609731442(1)](gitNotePicture\1609731442(1).jpg)

> 同步提交

![1609731812(1)](gitNotePicture\1609731812(1).jpg)

> 合并不上,先解决冲突再次提交

![1609731863(1)](gitNotePicture\1609731863(1).jpg)