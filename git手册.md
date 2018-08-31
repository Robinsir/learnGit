#Git 翻查手册

##一、git 基本指令
- git init 初始化仓库
- git clone 克隆仓库
- git commit -a -m 追踪并提交（需要添加注释）
- git status 当前状态
- git diff 查找不同
- git log  查看历史版本
   - git log --pretty=oneline 单行显示模式
   - git log --graph 合并分支图
   - git reflog 查看命令历史
- git reset --hard HEAD^ HEAD表示当前版本/也可以用ID替代
   - git reset HEAD<file>可以放弃暂存区修改
   - git reset --hard HEAD 快速回到上一次提交
- git checkout 切换分支
  - git checkout -b 创建并切换分支
  - git checkout -- <file> 放弃存储区某个文件
- git rm 删除一个文件
- git branch 创建分支
- git merge 合并分支
- git checkout -b dev origin/dev 同步分支
- git rebase吧本地push分叉整理成直线
- git tag 打标签
  - git tag -d 删除标签
  - git push origin /--tags (表示所有标签) 推送标签
- git remote add/rm 关联远程库
##二、git使用方法
  git有个最佳实践，master是主分支，用来做正式发布版之后的保留历史，其他分支包括dev用来做正常开发，多个feature用来做某些特性功能，release用来做发布版历史，每次发布都是用release打包，hotfix用来做发布版之后的一些及时迭代修复bug的工作。
##三、多人协作的工作模式：

1. 首先，可以试图用git push origin <branch-name>推送自己的修改；

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3. 如果合并有冲突，则解决冲突，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

5. 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

##四、git远程操作
1. 生成公钥ssh-keygen -t rsa -C "youremail@example.com"
2. 把公钥放入Github中
3. 先把创建的东西pull下来
4. 如果是要合并两个项目：--allow-unrelated-histories
git pull origin master --allow-unrelated-histories