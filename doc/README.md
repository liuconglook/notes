## Git

### 图形化

~~~shell
# git自带可视化日志
gitk

# 可视化操作等
git-gui

# 解决git乱码问题，项目下.git/config文件，添加：
[gui]
  encoding = utf-8
~~~

### 仓库

~~~shell
# 在当前目录下初始化本地仓库作为项目目录， 可以是一个空目录，可用添加远程仓库拉取代码
git init

# clone远程项目
git clone ssh://git@github.com/root/xxx.git

# 全局配置，本地配置用--local
git config --global user.name xxx
git config --global user.email xxx@gmail.com

# 查看配置
git config --local --list
# 查找
git config --local --list |grep user
~~~

### Git-Flow

~~~shell
V1.0.0.0
# master主版本号 产品方向，架构升级
# 次版本号 一个迭代周期，若干个功能升级
# 功能号（PR） 一个pull request累计一个
# 热修复号 测试或上线bug的修复，修复一次累计一个
~~~

- master：主分支
  - 对外发布
  - 受保护分支（只读，只能从其他分支（release/hotfix）合并）
  - 打标签（方便追溯）

- develop：开发分支
  - 基于master分支的clone。
  - 受保护分支（只读，只能从其他分支合并）
    - feature合并到develop
    - develop拉取release分支，提测
    - release/hotfix上线完毕，合并到develop并推送远程

- feature：功能分支
  - 基于develop分支的clone。
  - 对新需求新功能开发完毕后，合并到develop分支（未正式上线不推送远程）
  - 多功能分支，推送后可选删除。

- release：测试分支
  - 基于feature分支的clone。
  - 写测试代码，发先bug后即在本分支修复。
  - 修复完毕后，合并到develop/master分支并推送，打tag。
  - 可选删除。

- hotfix：补丁分支
  - 基于master分支的clone。
  - 修复完毕后合并到develop/master分支并推送，打tag。
  - 可选删除分支，所有hotfix修改都会进入下一个release。

~~~shell
# localdev:本地分支 develop:远程开发分支 master：远程主分支
# clone远程分支develop
git checkout -b localdev origin/develop

# 添加
git add 文件
# 提交
git commit -m'注释'

# 切换分支
git checkout develop
# 合并分支
git merge --no-ff -m 'xxx' localdev

# 比较分支
git diff develop localdev
# 工作区与当前本地分支比较
git diff HEAD
# 暂存区与当前本地分支比较
git diff --cached

# 推送远程
git push origin develop
~~~

### 分支操作

~~~shell
# 查看本地分支
git branch
# 查看本地和远程分支，v详细
git branch -av

# 创建分支，可以是分支名或tag或commit hash
git checkout -b localdev origin/develop

# 删除本地分支，使用-D没有合并则不会删除
git branch -d xxx
# 删除远程分支
git push origin --delete xxx

# 切换分支
git checkout xxx
~~~

### 里程碑

~~~shell
# 添加tag
git tag v1.0.0.0
# 添加带注释的tag
git tag -a v1.0.0.1 -m 'release v1.0.0.1 version'

# 推送某个tag到远程
git push origin v1.0.0.0
# 推送所有tag到远程
git push origin --tags

# 查看本地某个tag及注释
git show v1.0.0.0
# 查看本地tag
git tag
# 查看所有远程tag
git ls-remote --tags origin

# 删除本地tag
git tag -d v1.0.0.1
# 删除远程tag
git push origin :refs/tags/v1.0.0.1
~~~

### 远程操作

~~~shell
# 拉取到本地仓库
git pull

# 推送 -f用力推
git push

# 查看远程仓库列表
git remote -v

# 添加远程仓库origin
git remote add origin ssh://git@101.52.4.78:8022/root/plctake.git

# 拉取远程仓库代码，拉取到本地origin，可用作查看远程仓库是否更新，diff比较
git fetch origin
~~~

### 解决冲突

~~~shell
# 如果不小心提交到远程进行PR时冲突，可以回滚
git reset --soft xxx

# 查看远程是否更新
git fetch origin

# 暂存本地修改
git stash

# 拉取新代码
git pull origin/develop

# 取出本地修改
git stash apply stash@{1}

# 解决冲突后，提交，pr后,打tag新版本
git commit -m'解决冲突'
~~~

### 辅助操作

~~~shell
# 查看提交日志 -数字表示查看几条
git log -3
git log --oneline
git log --oneline --graph # 版本线 --all 表示包含其他分支
git reflog # 总的提交日志
git log --pretty=oneline

# 查看工作区/暂存区
git status

# 移出暂存区（到工作区），相当于对add的撤回
git restore --staged xxx
# 所有移出暂存区（到工作区）
git reset HEAD

# 将工作区的修改取消，切换分支也会取消修改，手动删除文件也行
git restore xxx

# 恢复到与远程分支一样的状态
git reset --hard ORIG_HEAD
# 恢复到指定commit，不保留本地代码
git reset --hard xxx
# 恢复到指定commit，保留本地代码
git reset --soft xxx

# 暂存 add缓冲区
git stash save '暂存本次修改，先拉取代码'
# 查看暂存
git stash list
# 恢复工作，删除暂存,例如：1
git stash pop stash@{1}
# 恢复工作，保留暂存，例如：1
git stash apply stash@{1}
# 删除暂存,例如：1
git stash drop stash@{1}
# 删除全部暂存
git stash clear

# 撤销pull
# 查看历史log
git reflog
# 撤销，例如：选择HEAD@{1}
git reset --hard HEAD@{1}

# pick保留；drop丢弃
# reword:保留，需修改注释；edit保留修改
# squash与前一个合并；fixup与前一个合并，不保留注释
# exec执行shell命令
# 合并commit, 区间[xxx~xxx]或第一commit到xxx
git rebase -i xxx xxx
# 撤销合并修改
git rebase --abort
~~~

### 参考资料

https://www.cnblogs.com/tomakemyself/p/13829985.html

https://developer.aliyun.com/article/785435?spm=a2c6h.17698244.wenzhang.1.1d323bbbVrJPGl

https://developer.aliyun.com/article/767181?spm=a2c6h.17698244.wenzhang.7.1d323bbb6gFyQI

### Git-Flow实践

#### 准备工作

- GitHub仓库：git-flow
- 开发人员：liu liu@gmail.com、cong cong@qq.com
- 保护分支：master、release、develop

> 设置受保护分支

起初创建项目，只有一个main分支

创建release、develop后，将main分支重命名（或创建master并设置为主分支，删除main分支）。

- 点击`main`分支选择，搜索或创建`release`
- 点击`branches`进入分支页面，即可看见编辑、删除按钮。
- 点击`Settings`=>`Branches`可修改主（默认）分支。

初始化版本（tag）

- 点击`tags`进入页面，选择分支创建对应版本：

- master：V0.0.0

- relese：V0.0.0.RELEASE

- develop：V0.0.0.DEVELOP

> 组织人员设置

开源项目则不需要

https://docs.github.com/cn/organizations/collaborating-with-groups-in-organizations/about-organizations

> 迭代计划

- 目前版本：V0.0.0

- 本次迭代目标：V0.1.0
- 功能项
  - 功能1（由`liu`开发）
  - 功能2（由`cong`开发）

#### 开发功能

- develop_liu（`liu`的专属开发分支）
- develop_cong（`cong`的专属开发分支）
- 其他分支同理，或自定义规则。

~~~shell
# liu 复制项目到本地，cong 复制项目同理
git clone git@github.com:liuconglook/git-flow.git
# 根据远程分支develop创建develop_liu本地分支
git checkout -b develop_liu origin/develop

# 本地配置
git config --local user.name liu
git config --local user.email liu@gmail.com

# 开发，重复以下操作
# 模拟开发
vim feature-1.text
# 添加到缓冲区
git add feature-1.text
# commit提交到本地版本库
git commit feature-1.text -m'add feature-1'
# 可选择的，提交到对应的远程分支
git push origin/develop_liu 
~~~

#### 完成功能

~~~shell
# 当所有都提交（commit）到本地版本库后
# 需提交到对应的远程分支
git push origin/develop_liu

# 进行PR,等待管理员同意后，切换到develop分支，标记版本tag
git checkout origin/develop
# 添加本地版本
git tag -a 0.1.0.DEVELOP -m 'add 0.1.0.DEVELOP'
# 提交到远程版本
git push origin 0.1.0.DEVELOP
~~~

#### PR

告诉管理员（可操作受保护分支），我的功能完成了，拉取我的代码合并吧！

- GitHub：pull requests
- GitLab：merge requests

需在网页上操作。

如果是贡献代码，则需要fork他人项目到Github仓库才能PR。除非分支不受保护，就可以直接push。

#### 解决冲突

当本地版本库不是最新时提交，就会产出冲突。

~~~shell
# 情况一：develop_liu本地分支与远程分支冲突，公司电脑忘提交远程，在家又写了些代码提交远程，回到公司继续提交时就会冲突。
# 情况二：本来我是最新代码，但有人先一步完成合并到develop分支了，这时进行PR就会冲突。
# 情况三：修改同一文件中的同一处代码。

# 所有不管哪种情况，都需要检查下自己代码是否是最新的
# 最好拉取下远程代码
git fetch origin
# 好做分支比较
git diff develop_liu origin/develop_liu

# 暂存本地修改
git stash

# 拉取新代码
git pull origin/develop_liu

# 查看暂存
git stash list
# 取出本地修改
git stash apply stash@{1}

# 可以看到，冲突文件为待add的
git status

# 冲突部分代码通常会被>>>>><<<<<包裹
# 解决冲突后即可add、commit、“完成功能”
~~~

#### 提测

所有功能完成后，由一人进行测试、bug修复，最后打上迭代版本V0.1.0.RELEASE

或省去release分支，由开发人员进行功能测试，自测后直接发布。

~~~shell
# 拉取最新的develop分支
git checkout -b release_liu origin/develop

# 编写测试代码，修改bug，提交
vim feature-1-test.text # 功能1的测试代码
vim feature-2-test.text # 功能2的测试代码
vim feature-1.text # 对功能1进行bug修复
git commit feature-1-test.text -m'add feature-1-test'
git commit feature-2-test.text -m'add feature-2-test'
git commit feature-1.text -m'add feature-1.text'

# 提交到远程，进行PR，合并到release，打tag：0.1.0.RELEASE，有bug则打tag：0.1.0.1.RELEASE
git push origin/release_liu
# PR后，切换到release打tag
git tag -a 0.1.0.1.RELEASE -m 'add 0.1.0.1.RELEASE'
# 提交到远程版本
git push origin 0.1.0.1.RELEASE
~~~

#### 热修复

~~~shell
# 如有修复bug，需再PR，合并到develop，打tag：0.1.0.1.DEVELOP
# 管理员通过cherry-pick，挑选修复bug部分commit的代码进行合并到develop分支
# 拉取release
git checkout -b release_hotfix_liu origin/release
# 提交到远程,进行PR，合并到develop
git push origin release_hotfix_liu
# PR后，切换到develop打tag
git tag -a 0.1.0.1.DEVELOP -m 'add 0.1.0.1.DEVELOP'
# 提交到远程版本
git push origin 0.1.0.1.DEVELOP
~~~

#### cherry-pick

https://blog.csdn.net/FightFightFight/article/details/81039050

可命令行操作，如果是PR，需在对应的远程仓库操作。

~~~shell
# 假如我是管理员，可用对release进行push操作
git checkout release
# 从release_hotfix_liu分支，挑选修复bug的commit进行合并
# -n不会自动合并,-e可修改提交信息
git cherry-pick xxx
# 如果报错，说明有冲突，比较常见，因为修复bug的地方与有bug的地方通常是一个地方
# 解决冲突后commit，再继续，如果使用 -n则不会自动合并，也需要执行下面操作
git commit
git cherry-pick --continue

# 如果不想解决冲突了,可终止操作，保留挑选
git cherry-pick --quit

# 如果不想挑选合并来，可终止操作，恢复到cherry-pick之前
git cherry-pick --abort
~~~

