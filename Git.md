# Git
## :white_check_mark: 什么是 Git ？
1. Git 是一种 *分布式版本控制系统*
2. 工作原理/流程
   
   ![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/cb27aaef-9ae7-489d-99d3-1846cc8ed09e)
   - workspace : 工作区
     - 程序员🧑‍💻进行开发改动的地方。
   - Index/Staging area : 暂存区/缓存区
     - 暂存区 是代码提交更改之前 的临时保存点。   
   - Repository : 仓库区（ 或本地仓库 ）
     - 在本地存储 提交代码更改 的地方。
   - Remote : 远程仓库
     - 类似于GitHub的服务器，用于共享和备份代码。
  ***
  ## :white_check_mark: Git 工作流程
  一般工作流程如下：
  - 克隆 Git资源 作为工作目录
  - 在克隆的资源上添加或者修改文件
  - 如果其他人修改了，你可以更新资源
  - 在提交前查看修改
  - 提交修改
  - 在修改完成后，如果发现错误🙅，可以撤回提交并再次修改后提交
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/cc2b429d-cd15-4376-b08b-d31f7df802e1)
***
## :white_check_mark: Git 基本操作
### 1.git安装及版本的查看
- git的平台安装地址为:http://git-scm.com/downloads
- 查询git是否安装成功可以使用
  ~~~
  git --version
  ~~~
- 配置git的个人用户名称为runoob
~~~
git config --global user.name "runoob" 
~~~
- 配置git的电子邮件地址为 test@runoob.com
~~~
git config --global user.email test@runoob.com 
~~~
根据实际情况配置用户名和邮箱📮
- 查看git的配置信息
~~~
git config --list
~~~
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/dcb21757-b709-4353-8142-2d83e02ce520)


### 2.git init 初始化仓库
- git init 是使用 Git 的第一个命令。
- 在执行完 git init 命令后，Git仓库会生成一个 .git目录。
~~~
git init
~~~
- 使用我们制定目录作为 Git 仓库
~~~
git init newrepo
~~~
- 初始化后，在 newrepo 仓库会有一个.git的目录，所有 Git 需要的数据和资源都存放在这个目录中。

### 3.git clone 克隆远程仓库
- 使用 git clone 从现有 Git 仓库中拷贝项目
克隆仓库的命令格式为：
~~~
git clone <repo>
~~~
- 如果需要克隆到当前终端路径下的 gitDemo文件 下 gitDemo 是终端创建的 可以使用以下命令格式：
~~~
git clone <repo> <directory>
~~~
参数说明:    
1️⃣ repo : Git仓库（ http:xxx地址 ）   
2️⃣ directory : 本地目录 （ gitDemo ）

### 4.创建分支和切换分支
- 创建分支名为 new-branch
~~~
git branch new-branch
~~~
- 切换分支到分支new-branch
~~~
git checkout new-branch
~~~

### 5.文件状态查看和更改
- 文件的状态查看
~~~
git status
~~~
- 将有内容或路径变更的文件README.md添加到暂存区
~~~
git add README.md
~~~
- 将所有有内容或路径变更的文件添加到暂存区
~~~
git add .
~~~
- 将处于暂存区和工作区中的文件README.md从暂存区和工作区中删除
~~~
git rm README.md
~~~
- 强制将处于暂存区和工作区中的文件README.md从暂存区和工作区中删除
~~~
git rm -f README.md
~~~
- 将处于暂存区文件README.md从暂存区中移除，但仍保留在工作区中
~~~
git rm --cached README.md
~~~
- 将暂存区的文件提交并记录此次变更动向
~~~
git commit -m '自定义此次文件主要变更主题'
~~~

### 6. git pull / git push
- git pull 命令用于 从远程获取代码并合并本地的版本
- git pull 合并 远程仓库origin 分支master 到本地仓库分支 new-branch
~~~
git pull origin master:new-branch
~~~
- git push 命令用于 从将本地的分支版本 上传到 远程并合并 git push 本地仓库locaOrigin 的 分支new-branch
- 上传到远程仓库分支 master 中合并
~~~
git push localOrigin new-branch:master
~~~

### 7.远程仓库的相关信息和添加/删除/重命名
- 列出当前仓库中已配置的远程仓库
~~~
git remote
~~~
- 列出当前仓库中已配置的远程仓库，并显示它们的 URL
~~~
git remote -v
~~~
- 添加一个新的远程仓库名为origin且URL为“https://github.com/user/repo.git”，并添加到当前仓库中
~~~
git remote add origin https://github.com/user/repo.git
~~~
- 重命名已配置的远程仓库origin 为 new-origin
~~~
git remote rename origin new-origin
~~~
- 从当前仓库中删除指定的远程仓库new-origin
~~~
git remote remove new-origin
~~~
- 修改指定远程仓库的 URL 为“https://github.com/user/new-repo.git”
~~~
git remote set-url origin https://github.com/user/new-repo.git
~~~
- 显示远程仓库origin的详细信息，包括 URL 和跟踪分支
~~~
git remote show origin
~~~

### 8.使用终端添加内容为“this is test”的文件REMADE.md
~~~
`echo "this is test" >> REMADE.md`
~~~

### 9.与远程仓库交互时遇到的问题
- Push failed remote HTTP Basic Access denied Authentication failed

在本地仓库版本使用git push提交的远程仓库报错：Push failed remote HTTP Basic Access denied Authentication failed

着重注意当前git的个人配置中，账户名，邮箱和密码与与远程仓库是否一致 
![image](https://github.com/vlvvh/C-sharp-learn/assets/160467935/e1fdd117-4bb4-439f-ab05-3539124c7be3)



ps:命令部分来源于Freya。
