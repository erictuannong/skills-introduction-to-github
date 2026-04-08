一、Git 安装
Linux CentOS
bash
sudo yum install git -y
Linux Ubuntu
bash
sudo apt-get install git -y
查看版本
bash
git --version
二、创建本地仓库
1. 创建目录
bash
mkdir gitcode
2. 初始化仓库
bash
git init
初始化完成后，目录下会多出一个隐藏的 .git 目录（不要手动修改其中的内容）

三、配置本地仓库
配置用户名和邮箱（必须配置）
bash
git config user.name "用户名"
git config user.email "电子邮件"
查看配置
bash
git config -l
删除配置
bash
git config --unset user.name
git config --unset user.email
全局配置（所有仓库生效）
bash
git config --global user.name "用户名"
git config --global user.email "电子邮件"
删除全局配置
bash
git config --global --unset user.name
git config --global --unset user.email
四、核心概念：工作区、暂存区、版本库
区域	说明
工作区	电脑上写代码/文件的目录
暂存区（stage/index）	存放在 .git/index 中，临时存储修改
版本库（repository）	.git 目录，永久存储所有版本记录
基本操作流程
bash
# 将文件添加到暂存区
git add 文件名          # 添加指定文件
git add .              # 添加所有文件

# 将暂存区内容提交到版本库
git commit -m "提交说明"

# 查看工作区状态
git status

# 查看提交记录
git log
git log --pretty=oneline    # 简洁显示
查看文件差异
bash
git diff 文件名    # 查看暂存区和工作区的差异
五、版本回退
回退命令
bash
git reset [--soft | --mixed | --hard] [HEAD]
选项	说明
--soft	只回退版本库内容
--mixed	回退版本库和暂存区（默认）
--hard	回退所有区域（慎用）
版本标识
标识	说明
HEAD	当前版本
HEAD^	上一个版本
HEAD^^	上上个版本
commit-id	指定版本
查看操作记录（用于恢复误回退）
bash
git reflog
六、撤销修改
场景	操作
工作区修改未暂存	git checkout -- 文件名
已暂存未提交	git reset HEAD + git checkout -- 文件名
已提交未推送	git reset --hard HEAD^
⚠️ 代码推送到远程仓库后不能再撤销

七、删除文件
bash
# 删除工作区和暂存区文件
git rm 文件名

# 删除版本库文件
git commit -m "删除文件"
八、分支管理
分支基础命令
bash
# 查看分支（*表示当前分支）
git branch

# 创建分支
git branch 分支名

# 切换分支
git checkout 分支名

# 创建并切换分支
git checkout -b 分支名

# 合并分支（需先切换到目标分支）
git merge 被合并的分支名

# 删除分支
git branch -d 分支名

# 强制删除分支（未合并时使用）
git branch -D 分支名
合并冲突解决
当两个分支修改了同一文件的同一位置时，合并会产生冲突。冲突文件会显示：

text
<<<<<<< HEAD
当前分支的内容
=======
被合并分支的内容
>>>>>>> 分支名
解决步骤：

手动编辑冲突文件，保留需要的内容

删除 <<<<<<<、=======、>>>>>>> 标记

执行 git add 和 git commit

推荐合并方式（禁用 Fast-Forward）
bash
git merge --no-ff -m "合并说明" 分支名
使用 --no-ff 可以保留分支合并的历史记录

分支策略建议
分支	说明
master	主分支，保持稳定
develop	开发分支
feature/*	功能开发分支
hotfix/*	紧急修复分支
九、临时保存（stash）
bash
# 保存当前工作区
git stash

# 查看保存列表
git stash list

# 恢复最近的保存并删除
git stash pop
十、远程仓库
克隆远程仓库
HTTPS 方式：

bash
git clone 仓库地址
SSH 方式（需先配置公钥）：

bash
# 生成SSH密钥对
ssh-keygen -t rsa -C "电子邮件"

# 将 id_rsa.pub 内容添加到 Gitee/GitHub 的 SSH 公钥设置中

# 克隆仓库
git clone 仓库地址
远程操作命令
bash
# 推送本地分支到远程
git push origin <本地分支>:<远程分支>

# 拉取远程分支到本地（自动合并）
git pull origin <远程分支>:<本地分支>

# 查看远程仓库信息
git remote -v
📌 注意：

origin 是远程仓库的默认别名

推送前建议先执行 git pull 保持代码同步

发生冲突时，先 pull 解决冲突再 push

建立本地分支与远程分支的关联
bash
# 创建分支时建立关联
git checkout -b dev origin/dev

# 分支已存在时建立关联
git branch --set-upstream-to=origin/dev dev

# 查看分支关联情况
git branch -vv

# 查看所有分支（本地+远程）
git branch -a
十一、配置命令别名
bash
git config --global alias.别名 原名

# 示例
git config --global alias.lpa 'log --pretty=oneline --abbrev-commit'
十二、标签管理
标签操作
bash
# 给当前版本打标签
git tag 标签名

# 给指定版本打标签
git tag 标签名 <commit-id>

# 给标签添加描述
git tag 标签名 -m "描述信息" [commit-id]

# 查看标签详细信息
git show 标签名

# 删除本地标签
git tag -d 标签名
推送标签到远程
bash
# 推送指定标签
git push origin 标签名

# 推送所有标签
git push origin --tags

# 删除远程标签（先本地删除，再推送）
git tag -d 标签名
git push origin :标签名
十三、多人协作场景
场景一：单分支协作
每个开发者克隆仓库并创建本地开发分支

bash
git checkout -b dev origin/dev
开发、提交、推送

出现冲突时：git pull → 解决冲突 → 重新 push

最后通过 Pull Request/Merge Request 合并到 master

场景二：多分支协作
远程创建不同功能分支（如 dev1、dev2）

各开发者拉取对应的分支进行开发

开发完成后提交 Pull Request

代码审核通过后合并到 master

十四、Git 分支设计规范（GitFlow）
分支名称	适用环境	说明
master	生产环境	只读且唯一，只合并 release 分支，不可直接修改
release/*	预发布/测试环境	基于 develop 创建，用于功能测试，上线后可删除
develop	开发环境	基于 master 创建，始终保持最新代码
feature/*	本地开发	基于 develop 创建，功能完成后合并回 develop
hotfix/*	本地紧急修复	基于 master 创建，修复后合并到 master 和 develop
命名规范建议
release/version_publishtime

feature/user_createTime_feature

hotfix/user_createTime_hotfix

十五、常用命令速查表
操作	命令
初始化仓库	git init
添加文件	git add .
提交	git commit -m "说明"
查看状态	git status
查看日志	git log --oneline
查看操作记录	git reflog
查看差异	git diff
创建分支	git branch 分支名
切换分支	git checkout 分支名
合并分支	git merge 分支名
删除分支	git branch -d 分支名
临时保存	git stash / git stash pop
克隆远程	git clone 地址
拉取代码	git pull
推送代码	git push
打标签	git tag 标签名
