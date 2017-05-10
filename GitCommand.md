# Git 命令总结

### 一、配置Git 用户名及email

git config --global user.name 'xxx'  #配置用户名
git config --global user.email 'xxx'  #配置email

###二、常用命令
git init
git add <hello.txt>  # 把所有要提交的文件修改放到暂存区
git commit -m 'add a file' # 把暂存区的所有内容提交到当前分支
git status #掌握工作区状态
git diff #查看文件修改内容
git log #查看提交历史
        git log --pretty=oneline
git reset --hard HEAD^ #回退到上一个版本
        HEAD^^(上上版本),HEAD~100(往上100个版本)
        commit id(版本号) 可回到指定版本
git reflog #查看历史命令
工作区（Working Directory）
版本库（Repository） #.git
        stage(index) 暂存区
        master Git自动创建的分支
        HEAD 指针
git diff HEAD -- <file> #查看工作区和版本库里最新版本的区别
git checkout -- <file> #用版本库的版本替换工作区的版本，无论是工作区的修改还是删除，都可以'一键还原'丢弃工作区的修改（让文件回到最近一次的git commit或git add时的状态）
git reset HEAD <file> #把暂存区的修改撤销掉，重新放回工作区。
git rm <file> #删除文件，若文件已提交到版本库，不用担心误删，但是只能恢复文件到>最新版本

ssh-keygen -t rsa -C 'user@example.com' #创建SSH Key
git remote add origin git@github.com:username/repostery.git #关联本地仓库，远程>库的名字为origin
        #第一次使用Git的clone或者push命令连接GitHub时需确认
git push -u origin master #第一次把当前分支master推送到远程，-u参数不但推送，而>且将本地的分支和远程的分支关联起来
git push origin master #把当前分支master推送到远程
git clone git@github.com:username/repostery.git #从远程库克隆一个到本地库
        #git支持多种协议，包括https，但通过试试支持原生git协议速度最快

分支
git checkout -b dev #创建并切换分支
        #相当于git branch dev 和git checkout dev
git branch #查看当前分支，当前分支前有个*号
git branch <name> #创建分支
git checkout <name> #切换分支
git merge <name> #合并某个分支到当前分支
git branch -d <name> #删除分支
git log --graph #查看分支合并图
git merge --no-ff -m 'message' dev #禁用Fast forward合并dev分支
        #本次合并要创建新的commit，所以要加上-m参数，把commit描述写进去
        #Fast forward合并不可查看合并记录
git stash #隐藏当前工作现场，等恢复后继续工作
git stash list #查看stash记录
git stash apply #仅恢复现场，不删除stash内容
git stash drop #删除stash内容
git stash pop #恢复现场的同时删除stash内容
git branch -D <name> #强行删除某个未合并的分支
        #开发新feature最好新建一个分支
git remote #查看远程仓库
git remote -v #查看远程库详细信息

git pull #抓取远程提交
git checkout -b branch-name origin/branch-name #在本地创建和远程分支对应的分支
git branch --set-upstream branch-name origin/branch-name #建立本地分支和远程分支
的关联
##标签
git tag v1.0 #给当前分支最新的commit打标签
git tag v0.9 36df530 #给历史提交的commit打标签
git tag -a v0.1 -m 'version 0.1 released' 3628164 #-a指定标签名，-m指定说明文字
git tag -s <tagname> -m 'blabla' #可以用PGP签名标签
git tag #查看所有标签
git show v1.0 #查看标签信息
git tag -d v0.1 #删除标签
git push origin <tagname> #推送某个标签到远程
git push origin --tags #推送所有尚未推送的本地标签删除远程标签  
git tag -d v0.2 #先删除本地标签

                         git push origin :refs/tags/v0.2 #删除远程标签
##自定义git
git config --global color.ui true
编写.gitignore文件来忽略某些文件，此文件本身要放到版本库内，并可对其做版本管理
git add -f hello.pyc #-f参数强制添加到Git
git check-ignore -v hello.pyc　＃检查.gitignore文件的规则
简写命令
git config --global alias.co checkout #简写checkout命令
git config --global alias.st status
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD' #撤销暂存区的修改
git config --global alias.last 'log -1' #查看最近一次的提交
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
配置文件
--global参数时针对当前用户起作用，如果不加，仅针对当前仓库起作用
每个仓库的Git配置文件在 .git/config 文件中
当前用户的Git配置文件在用户主目录下的 .gitconfig 文件中

##搭建Git服务器
1、安装git sudo apt install git
2、创建git用户 sudo adduser git
3、创建证书登录，将所有需要登录的用户的公钥导入到/home/git/.ssh/authorized_keys>文件，每行一个
4、初始化Git仓库
        在仓库目录下输入命令 sudo git init --bare sample.git 创建裸仓库（没有工>作区）
        把owner改为git sudo chown -R git:git sample.git
5、禁用shell登录，修改/etc/passwd文件
        git:x:1001:1001:,,,:/home/git:/bin/bash
        改为：
        git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

6、克隆远程仓库
git clone git@server:/srv/sample.git
