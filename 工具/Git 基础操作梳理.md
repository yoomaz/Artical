### 前言

关于代码仓库，在几年前，可能大家使用的还是 SVN 比较多，但是最近几年，GIt 开始更加流行起来，与 SVN 相比，Git 是一个分布式版本控制系统，它在本地也有一个仓库，在不需要联网的情况下也可以进行代码提交的工作，只是不能 push 到远程，他的主要特点如下：

+ 更快的速度
+ 对非线性开发模式的强力支持（允许上千个并行开发的分支）
+ 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）
+ 使用简单并且方便（最重要！）

虽然很多 IDE 已经集成了图形化的操作界面，例如 Android Studio 和 IDEA 等，还有专门的 Git 图形化工具 SourceTree，但是对于想更深入使用 Git 的我们来说，非常有必要掌握一些常用底层的 Git 命令，使用这些命令，和图形 IDE 一起使用，非常的高效。

### 详细命令

Git 的操作命令众多，这里记录了日常开发中 95% 经常使用的命令。

1. `$ git --config`

   这个是对 Git 进行配置的命令，专门用来配置和读取相应的工作环境变量，正式因为这些变量，决定了 Git 在工作时候的工作方式和行为，分为系统级配置、用户级配置、项目级配置，根据配置的等级，这些变量存放在三个不同位置位置：

   + /etc/gitconfig ：使用 --system 选项，因为是在系统级别目录下的，存储的配置信息对所有用户都有效。
   + ~/.gitconfig : 使用 --global 选项，在用户目录下，只对当前用户有效
   +  .git/gitconfig ：使用 --local 选项，只对当前项目有效，

   这三个的优先级是依次增高的，对于相同的配置信息，后者的配置会覆盖前者。

   常见的配置信息有, 用来配置 git 提交时候的用户名和邮箱

   ```bash
   // 配置用户名
   $ git config --global user.name "zhulei" 
   // 配置邮箱
   $ git config --global user.email zhuleineuq@gmail.com
   // 修改这个项目的git信息，很常用
   $ git config --local user.name "zhulei"
   $ git config --local user.email zhuleineuq@gmail.com
   ```

   有时候我们要检查已有项目的配置信息，可以使用如下命令：

   ```bash
   $ git config --list 
   // 显示这个项目的 git 信息
   $ git config --local --list  
   // 显示用户的 git 信息
   $ git config --global --list  
   ```

   如果出现每次 `git pull` 或者 `git push` 都要输入账号和密码，可以用下面命令在本地保存密码：

   ```bash
   $ git config credential.helper store
   ```

2. `$ git --init`

   如果一个项目不是 Git 工程，可以使用这个命令把这个项目初始化，变成 GIt 工程

   ```bash
   $ git init  // 初始化仓库
   $ git add *.c // 向git中添加文件
   ```

3. `$ git clone`

   例如在远程已经有一个 git项目，我们想把它拷贝到本地，可以使用这个命令, 例如 github 上面，一个项目的地址是 `https://github.com/tangyxgit/FFmpegVideo.git`

   ```
   // 代码拷贝到本地
   $ git clone https://github.com/tangyxgit/FFmpegVideo.git
   // 拷贝的同时改一个名字
   $ git clone https://github.com/tangyxgit/FFmpegVideo.git MyFFmpegVideo
   ```

4. `$ git status`

    检查文件的状态

5. `$ git commit`

   提交代码，后面 -m 指这次提交包含的信息

   ```
   $ git commit -m 'message'
   ```

6. `$ git log`

   查看提交记录

7. `$ git remote`

   操作远程仓库

   ```
   // 只可以看到仓库的名字
   $ git remote 
   // 可以查看到仓库的名字和对应的地址
   $ git remote -v  
   
   // 远程仓库的重命名
   $ git remote rename pb paul 
   // 远程仓库的删除
   $ git remote rm paul  
   // 添加远程仓库
   $ git remote add origin git://github.com/paulboone/ticgit.git
   ```

8. `$ git fetch [remote-name]`

   从远程仓库中拉取数据,如果本地没有该分支，则直接拉下来，如果是本地已经存在的分支，例如origin，则是把最新的代码拉下来，但是这个时候并不合并处理

9. `$ git pull`

   git pull 命令自动抓取数据下来，然后将远端分支自动合并到本地仓库中当前分支

10. `$ git push`

    推送数据到远程仓库

    远程分支：我们用 (远程仓库名)/(分支名) 这样的形式表示远程分支。比如我们想看看上次同 origin 仓库通讯时 master 分支的样子，就应该查看 origin/master 分支

    在推送前，可以使用 **$ git remote -v**  可以查看到仓库的名字和对应的地址

    ```
    // 推送到 地址是 origin 的 master 分支
    $ git push origin master
    // 将本地代码强制推到远程，也就是用本地代码覆盖远程, 本地回退，然后也想服务端同步本地回退，可以用这个
    $ git push -f  origin master
    ```

11. `$ git branch`

    操作分支

    ```
    // 创建新的分支
    $ git branch testing 
    
    // 删除分支
    $ git branch -d hotfix 
    
    // 查看本地分支，注意看分支前面有 * 字符的，表示当前所在的分支
    $ git branch
    // 查看所有远程分支名字
    $ git branch -r  
    // 查看所有分支名字，包括本地分支
    $ git branch -a 
    ```

12. `$ git checkout`

    切换分支

    ```
    // 切换分支
    $ git checkout testing
    // 创建分支并切换
    $ git checkout -b iss53 
    // 跟踪远程分支：在通过跟踪远程分支创建的本地分支里，可以直接使用 git pull 和 git push来进行拉和推的操作
    $ git checkout --track
    ```

13. `$ git merge`

    合并分支

    ```
    $ git merge hotfix
    // 中断 merge
    $ git merge --abort
    ```

14. `$ git rebase`

    分支的衍合	

    把一个分支中的修改整合到另一个分支的办法有两种：merge 和 rebase.有了 rebase 命令，就可以把在一个分支里提交的改变移到另一个分支里重放一遍。

    ```
    // 找到两个分支的共同点，把当前分支在共同点之后的变化，在master当前所在位置重新执行一遍
    $ git rebase master 
    // 这个命令比较复杂，因为当前是在 client 内，所以是取出 client 分支，找出 client 分支和 server 分支的共同祖先之后的变化，然后把它们在 master 上重演一遍
    $ git rebase --onto master server client 
    ```

15. `$ git reset`

    先使用 git log 查看序号

    ```
    // 直接回滚到这个节点，不保留更改
    $ git reset --hard f13210f0cdcc1fd27009d953edc0a23f65e6f2a9  
    // 回滚到这个节点，保留修改，一般是重新提交代码使用
    $ git reset --soft f13210f0cdcc1fd27009d953edc0a23f65e6f2a9 
    ```

16. `$ git tag`

    ```
    $ git tag  // 显示当前分支所有的 tag
    $ git tag -l '2.0.*'  // 对结果进行模糊匹配过滤
    $ git tag 2.0.8  // 打一个简单tag
    $ git tag -a 2.0.8 -m 'tag message'  // 带 Message 的tag
    $ git show 2.0.7  // 显示这个 tag 的详细信息
      graypn@Graypn:~/Code/Project_LongGe/apollo$ git show 2.0.7
      tag 2.0.7
      Tagger: zhulei <zhuleineuq@gmail.com>
      Date:   Thu Aug 31 11:54:38 2017 +0800
    
      2.0.7正式版本
    
      commit a1ca647e43dff98d8879563b8492c75bf6ae3fd6
      Author: zhulei <zhuleineuq@gmail.com>
      Date:   Thu Aug 31 09:42:31 2017 +0800
    
    $ git tag -a v1.2 9fceb02  // 后期根据某个提交补上tag
    	1.首先用 $ git log --pretty=oneline 显示简要的git提交信息，如下：
    	aa4d54677ee33ff72b9a3e81663a2c59657f2a0b feat:提升版本号
    	670c59f5e1f7cbd0e40310e6201b6a92284f5b3d fix:修复一个产品id传递的问题
    	a1ca647e43dff98d8879563b8492c75bf6ae3fd6 feat:提升版本号
    	2.然后用 git tag -a [tag] [完整提交号或者前几位] 补上 tag
    
    $ git push origin 2.0.8  // 默认情况下，git push 并不会把标签传送到远端服务器上，只有通过显式命令才能分享标签到远端仓库。其命令格式如同推送分支，运行 git push origin [tagname]
    	也可以用 $ git push origin --tags 推上去所有的 tag
    	
    
    $ git tag // 查看本地所有tag
    $ git tag v1 // 打tag
    $ git tag -a v1 -m '描述'
    $ git push --tags 或者 $ git push origin --tags// 推送tag到服务端
    $ git show v1 // 查看某一个具体 tag 的详情
    
    $ git reset --hard d5a65e  回退到某一次提交
    $ git  reset  --hard HEAD 回退到上次提交
    ```

17. `$ git stash`

    把当前未提交的代码压入一个缓存栈内

    使用场景：当前正在一个分支上进行开发时候，需要切换到另一个分支做一些操作，这个时候可以先用 git stash把当前操作缓存起来，再切换到另一个分支进行操作，操作完成后再切换回来，然后用 `$ gtit stash apply` 把上次缓存的代码取出来。

    `$ git stash list`

    看到所有缓存栈内的缓存记录。

    使用场景：在多个分支使用 `$git stash` 后，使用 `$git stash apply ` 获取上次缓存的操作已经不适用了，这个时候需要用 `$git stash list` 看所有缓存的记录:

    ```
    stash@{0}: WIP on btzz_1.3.1: a8a0560 feature-白条1.3.2
    stash@{1}: WIP on feature-kexin: d755dc5 feature-氪信原生埋点
    stash@{2}: WIP on btzz_1.3.1: 71450ab feature-白条原生首页和超市UI改版
    stash@{3}: WIP on feature-kexin: 7cc1a73 2.3.2
    stash@{4}: WIP on feature_user_center: c56bdeb feature-关于我们原生页面
    stash@{5}: WIP on feature_user_center: 1e21336 feature-个人中心原生UI
    stash@{6}: WIP on release-btzz-1.3.0: 8814922 Merge remote-tracking branch 'origin/master' into release-btzz-1.3.0
    stash@{7}: On master: Uncommitted changes before Update at 2017/11/3 下午2:25
    stash@{8}: WIP on master: f45170d fix-修复线上几个bug
    ```

    ```
    $ gtit stash
    $ gtit stash apply
    $ git stash list
    ```

### 常用命令

1. 拉取分支

   ```
   $ git clone git://github.com/schacon/grit.git mygrit
   ```

2. 修改在项目中的用户名和邮箱地址

   ```
   $ git config --local --list  //显示这个项目的git信息
   $ git config --local user.name "John Doe"
   $ git config --local user.email johndoe@example.com
   ```

3. 查看分支的状态

   ```
   $ git status
   ```

4. 提交代码

   ```
   $ git commit -m 'message'
   ```

5. 查看远程的分支详细情况，包括本地分支 push 和 pull 操作对应的目标分支

   ```
   $ git remote show origin
   ```

6. 修改仓库地址

   ```
   方式一：直接修改
   $ git remote set-url origin http://xxxx.git
   
   方式二：先删除后添加
   $ git remote remove xxx
   $ git remote add xxx
   ```

### 公司中拉取新分支进行开发步骤

1. 切换到 develop 分支并更新到最新

   ```
   $ git checkout develop
   $ git pull
   ```

2. 创建本地分支

   ```
   $ git checkout -b develop_xxx
   ```

3. 推送到远程主机，在远程主机创建新分支, 并跟踪

   ```
   git branch --set-upstream-to origin/feature-talkback
   ```

### 如何撤销一个合并

你应该始终牢记，你可以在任何时间执行撤销操作，并返回到你开始合并之前的状态。要对自己有信心，你不会破坏项目中的任何东西。只要在命令行界面中键入 `git merge --abort` 命令，你的合并操作就会被安全的撤销。

当你解决完冲突，并且在合并完成后发现一个错误，你仍然还是有机会来简单地撤销它。你只须要键入 `git reset --hard` 命令，系统就会回滚到那个合并开始前的状态，然后重新开始吧！

```
git merge --abort
```

