## 初始化本地仓 ##
+ 由创建本地仓开始
    + git init // 初始化本地仓，默认分支master
    + git remote add origin "ssh url" // 将本地仓与远程仓关联
        + 注意理解origin，可以理解为远程仓代号
        + git remote -v // 可以查看远程仓
        + 修改关联远程仓
            + git remote remove origin // 删除
            + git remote add origin "url"
    + 初次提交（假定相关内容已进暂存区）
        + git push -u origin master
            + 本地仓与远程仓已关联，初次提交仍需将本地分支与远程分支关联，即 -u
            + -u // 即 --set=upstream
            + 假定将本地分支内容推送至远程，远程不存在该分支，初次推送，需加 -u
        + 上述直接提交可能有误，需将本地分支直接关联到远程分支，然后进行提交
          + git branch --set-upstream-to=origin/<remote-branch> <local-branch>
+ 远程仓开始
    + 打开远程仓，切换分支，复制url（ssh, https均可）
    + git clone "url"
    + 不但本地与远程仓关联，默认分支已关联好。
    + 若本地再创建新分支，注意初次提交 -u 特例。

## 本地仓向远程仓提交（各仓及分支已关联） ##
+ 一般情况
    + git pull origin [branchname]
    + git status  //查看工作区更改
    + git add -A  //添加至暂存区，包括删除及新增
        + git add .   // except delete
        + git add u  // u means update, excepte new file
        + 解除add
            + git reset HEAD .  // 解除暂存区所有文件
            + git reset HEAD [filename]  // 解除暂存区某个文件
    + git diff --cached // 查看暂存区与远程分支区别，未commit,此很重要！！！，一般push前通过编辑器功能查看。
    + git commit -am "commit message"
        + -a 对于同个文件的多次commit，将会被合并
        + 虽未 push origin， 但diff 已失效
    + git commit --graph  // 查看提交记录
    + git push origin [branchname]
+ 撤回
    + 单独分支（开发中属于某个人分支）
        + git reflog  // 查看commit id
        + git reset --hard [commit id]  // commit id 前六位即可
            + head^ head~0 等亦可做为回退标记，建议commit id
            + 若本地未提交，当前已结束
        + git push -f  // 撤回远程，本地版本滞后远程时，远程默认拒绝，需强制更新。
    + 公共分支
        + 假定提交 A1 --> A2 --> B1(AB代表两个不同的开发人员)，A2出现问题，需要回退
            + 使用reset
                + reset 方法最大的问题在于不产生新的版本提交
                + 直接reset 至 A1，对于B而言，本地仓将领先远程仓，一旦B push, 远程仓又将包含A2版本代码
                + 回退需解决的问题：过滤A2版本代码，保留B1代码，产生一次新的版本提交，以领先B本地仓版本。
                + 以上通过reset?? 未实现
            + 使用revert
                + revert 撤销提交， 连续使用两次与非使用前一致， 撤销非最近提交一定会产生冲突。
                + 具体实现如下：
                    + git revert [commit id] // 因为回退，出现冲突无脑保留底部即可。
                    + 不存在项目中把某此提交单独抽取的可能。可以肤浅认为revert 比 reset默认添加一些版本提交
                    + 上述，回避B本地仓push将回滚重置，但仍将B1重置。
                    + 回退者应主动将B1版本加至远程仓。
                    + 对于真实的B开发者，pull的最理想情况，只是将A2涉及代码删除。
                + 考虑到撤回非最近代码必然产生冲突，如不想处理？
                    + 可以将回退至的某分支代码当做当前master，直接暴力重置。
                    + 然后依次添加。不稳定系数仍很高。
    + 实际处理过程中一般，针对当前操作为主。
    + 综上：虽有各种手段，特例下都不算完美，注意各开发者开发模块解耦（！！十分重要）。

## 跨分支合并 ##
+ push
    + git push origin [base_name]:[aimed_name] // base_name: 当前目录， aimed_name:目标目录
    + 该法太暴力，与merge默认状态类似。
    + 接上不用切换分支，实际开发中可以少用。
    + 缺点(为何少用？)
        + 不讨论merge rebase异同的前提下
        + 实际开发过程中，跨分支处理，一般需合并当前分支然后添加新的commit至目标分支上
        + 接上，单纯push显然无法满足要求
+ merge
    + 首先切换至目标分支
    + git merge -m "message" [branchname]
        + 默认merge在当前分支baseline 添加head，如果想保留之前分支的基本信息需添加 --no-ff 参数
            + git merge --no-ff -m "message" [branchename]
        + 上述仍不具备，合并分支前将多次commit合并的功能，不考虑rebase的一般用法如下：
            + 添加 --squash 参数
            + git merge --squash [branchname]
            + git commit -am "message" // 考虑到实际过程中可能会出现冲突，这种方式比较稳拖
            + 遗憾的是，该方式不保留之前分支信息，即当前base_line添加head
+ rebase
    + 一般原则：No one should rebase a shared branch // 被人他人pull过的公共分支不要操作head
    + merge与rebase区别
        + merge 以两个分支的共同祖先，以时间为序进行合并
        + rebase 所谓的两个分支的共同祖先节点，将被rebase命令重设，一般为合并分支的最新提交
        + merge 保留原来提交（与原分支header一致），rebase相当于复制原来分支提交并产生新的提交（header改变）
    + git pull --rebase(-r)
        + 理解从rebase根源理解，rebase改变两个分支共同祖先节点，一般为目标分支最新head
        + 假设master为 a --> b --> c --> d dev由c处分离，而d为非dev分支提交
        + 接上，dev git pull master // 此时将master处的d提交将pull至dev本地仓
        + git checkout master // 切换至公共分支master(本地)
        + git pull // 拉取远程
            + 此时merge 将产生两次commit
            + 一次为 master git pull origin dev 产生的 commit
            + 一次为 默认 master merge dev 提交（系统默认）
        + git pull --rebase
            + 此时将 默认 master merge dev 提交（系统默认）省略。
    + rebase 提交？？？？
## 临时BUG修改 ##
+ git一般分支策略
    + master 与正服保持一致，或与正服编译前代码保持一致
    + dev 开发分支，当前测试代码
    + v1.0 // 开发正式代码，备测
        + v1.0_zys // 开发最新代码
+ 假定 开发者在 v1.0_zys 开发，突发bug
    + 因为在私有分支，可以直接提交一次，目的保存当前代码 // 不建议
    + stash
        + git stash save "stash name" // 默认1
        + git stash pop "stash name" // 启用并删除
        + git stash apply "stash name" // 启用且保存
        + git stash list // 查看所有
        + git stash drop // 删除
    + 建议 master 分支另起bug分支
    + bug分支一般都会即时删除，至于是否保留该分支各记录，意义不大。

## 分支及标签管理 ##
+ 分支管理
    + 切换
        + git checkout -b branchName // -b 如无则创建该分支
    + 本地分支初次提交（将本地与远程关联）
        + git push -u origin branchname
    + 删除本地分支
        + git branch -D branch-name
    + 删除远程分支
        + git push -d origin branch-name
+ 标签管理
    + tag用处：如app各版本，建议tag保存，而非分支保存。部分工作运维通过tag拉远程代码，而非分支
    + 本地增删查
        + git tag -a tag-name -m "message about add tag name" // 增
        + git tag -d tag-name // 删
        + git tag // 查
    + 远程增删查切换
        + git push origin tag-name
        + git push origin -d tag-name // 删，与分支操作命令一样，不建议分支与标签名字一样。如果一样会发生什么？
            + git 原则不允许？？ // 如已存在 v1.0 tag-name 不允许本地v1.0分支提交
        + git tag -l // 本看远程分支
        + git checkout tag-name // 切换

## 补充说明 ##
+ git 操作branchname, 将暗指本地分支
+ 接上，添加origin条件，即为远程分支
+ 对于windows系统, 初次密码写错,将保存在系统凭据中,需删除


