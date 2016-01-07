# githug 闯关
[githug](https://github.com/Gazler/githug)是一个用来锻炼使用git的小工具，做的还挺好的，下面记录一下每一关的做法，也当做git的笔记了。

1. 初始化一个空的仓库
    
    ```
    cd project_dir
    git init
    ```
2. 设置git用户名和邮件

    ```
    git config user.name libenchao --global
    git config user.email libenchao@gmail.com --global
    
    ```
    git config 还有很多其他命令，例如```git config -l```可以查看所有当前的配置等。具体的可以直接敲一下```git config```看一下git的提示。
3. 将新添加的文件或者修改的文件添加到staging area（暂存区）中
    
    ```
    git add README
    ```
4. 将暂存区中的文件commit到git中

    ```
    git commit -m 'add README'
    ```
5. 从github中clone一个项目到本地

    ```
    git clone https://github.com/Gazler/cloneme
    ```
6. 从github中clone一个项目到本地，并重新命名

    ```
    git clone https://github.com/Gazler/cloneme my_cloned_repo
    ```
7. 修改gitignore文件，使其忽略`.swp`文件

    ```
    echo '*.swp' >> .gitignore
    ```
8. 修改gitignore文件，使其忽略所有`.a`文件，但是不忽略`lib.a`文件
    
    ```
    echo '*.a' >> .gitignore
    echo '!lib.a' >> .gitignore
    ```
9. 查看当前untracked的文件
    
    ```
    git status
    ```
    其中标红的是untracked，标绿色的是staged的。
10. 查看当前staged的文件
    
    ```
    git status
    ```
    跟上一题是一样的。
11. 找到已经从文件系统中删除，但是git中还没有删除的文件，并删掉
    
    ```
    git status
    git rm deleteme.rb
    ```
12. 有一个文件意外的被暂存（staged）了，现在将其从git的暂存区去掉

    ```
    git rm --cached deleteme.rb
    ```
13. 当前有些文件已经修改，但是我们需要将这些文件暂时保存起来，而不是提交。然后可能会切换分支等操作。

    ```
    git stash
    ```
    将当前没有被tracked的工作都保存到某个地方，感觉这就是一个栈。然后再之后可以再通过```git stash pop```将之前的工作找回来。
14. 给一个已经被git跟踪的文件重命名

    ```
    git mv oldfile.txt newfile.txt
    ```
15. 将之前的`.html`文件都移动到新建的```src```中

    ```
    mkdir src
    git mv *.html src/
    ```
16. 利用```git log```最近一次```commit```的```hash code```

    ```
    git log
    ```
17. 给当前的commit打一个tag

    ```
    git tag new_tag
    ```
    另外，```git tag```可以查看当前的tags。
18. 将当前本地没有提交到远程的tag提交到远程
    
    ```
    git push origin --tags
    ```
    感觉这个其实有点难了，不是很常用tag。然后查了一下，有很多tag的操作命令，下面简要介绍一下：
    
    ```
    查看版本：$ git tag
    创建版本：$ git tag [name]
    删除版本：$ git tag -d [name]
    查看远程版本：$ git tag -r
    创建远程版本(本地版本push到远程)：$ git push origin [name]
    删除远程版本：$ git push origin :refs/tags/[name]
    合并远程仓库的tag到本地：$ git pull origin --tags
    上传本地tag到远程仓库：$ git push origin --tags
    创建带注释的tag：$ git tag -a [name] -m 'yourMessage'

    ```
19. 当前有一个文件没有被commit，但是我们想将该文件commit到上次的commit中，而不是单独的再进行一次commit。

    ```
    git add *
    git commit --amend -m 'amend'
    ```
20. 当前有一些可以提交的东西，但是我们想将其提交时间更改为明天

    ```
    git commit --date=2016-01-08 -m 'tommorow'
    ```
    虽然做对了，但是不知道这个日期格式应该长什么样子。不过我感觉，这种东西不应该常用，commit日期不就是用来记录真实的时间的么。
21. 有两个已经staged的文件，但是现在想将他们分开commit，所以需要先将其中一个unstage

    ```
    git reset to_commit_second.rb
    ```
22. 我们提交的太快了，所以现在需要撤销上次的commit，但是要保留index（stage）

    ```
    git reset --soft HEAD^
    ```
    尝试了好多```git reset```，一直都不行。最后就突然一下加上HEAD^就好了。虽然有点不明所以，但是至少是通过了。回头一定要好好的研究一下git，感觉这个东西实在是有点深奥啊。
23. 有一个文件被修改了，但是我们不想要这个修改，所以想将文件恢复到上次commit的时候。

    ```
    git checkout config.rb
    ```
24. 查看远程仓库

    ```
    git remote -v
    ```
25. 查看远程仓库的url

    ```
    git remote -v
    ```
26. 从远程仓库中pull代码

    ```
    git pull https://github.com/pull-this/thing-to-pull
    ```
27. 将本地仓库添加一个远程仓库

    ```
    git remote add origin https://github.com/githug/githug
    ```
28. 本地master分支和远程origin/master分支分叉了，现在将本地会退到origin/master，然后将其push到remote

    ```
    git fetch remote master
    git reset FETCH_HEAD
    git add *
    git commit -m 'add files'
    git push
    ```
    其实不是特别理解，只是误打误撞就做出来了。
29. 本地有一个文件被修改了，使用```git diff```看哪一行被修改了。

    ```
    git diff app.rb
    ```
    可以看到diff的结果中有+++、---等符号。
30. 在config.rb文件中有个人放了一个密码在里面。找到那个人

    ```
    git blame config.rb
    ```
    可以看到每一行代码是谁改的.
31. 创建一个分支

    ```
    git branch test_code
    ```
32. 创建和切换分支

    ```
    git branch my_branch
    git checkout my_branch
    ```
33. checkout一个tag。

    ```
    git checkout v1.2
    ```
34. 同时存在一个v1.2的tag和一个branch，现在要切换到v1.2的tag

    ```
    git checkout tags/v1.2
    ```
35. 上一次commit之前忘记了创建分支了，现在需要在上一次commit之前创建一个分支

    ```
    git reset HEAD^
    git branch test_branch
    git add *
    git commit -m 'add files'
    ```
36. 删除一个branch

    ```
    git branch -d delete_me
    ```
37. 将本地的一个分支直接传送到远端

    ```
    git push --set-upstream origin test_branch
    ```
38. 有一个feature分支，现在想将其与master分支合并

    ```
    git merge feature
    ```
39. 将远端的内容下载本地，但是不merge

    ```
    git fetch
    ```
40. 现在有一个分支feature，现在需要将feature的base调整到master的现在的版本上。

    ```
    git checkout feature
    git rebase master
    ```
41. 将仓库打包，并且去除冗余的信息

    ```
    git gc
    ```
    本来```git repack```是打包的，但是网上说gc可以自动去除重复的
42. 在new_feature分支有，有一个commit我们想将其也commit到现在的master分支上

    ```
    git checkout new-feature
    git log
    git checkout master
    git cherry-pick ca32a6dac7b6f97975edbe19a4296c2ee7682f68
    ```
43. 查看代码中有多少“TODO”

    ```
    git grep "TODO"
    ```
44. 修改倒数第二次提交的message

    ```
    git rebase -i HEAD~~  #将需要修改的那个提交的pick改为edit
    git commit --amend    #修改message
    git rebase --continue
    ```
45. 将最近几个commit变成一个commit

    ```
    git rebase -i HEAD~2    #在弹出的界面中将第二个pick修改为squash，保存后就可以自动将最后两个commit合并。
    ```
46. 将long-feature-branch分支合并到master分支，并将其所有的commit作为一个commit

    ```
    git merge --squash long-feature-branch
    git commit -a -m 'merge'
    ```
47. 将两个commit重排序

    ```
    git rebase -i HEAD~~     #然后手工将两行commit顺序修改，然后保存退出即可
    ```
48. 不知道在最近的哪一次commit中引入了一个错误，我们想要找到那个commit

    ```
    git bisect start
    git bisect bad
    git bisect good 18ed2ac1522a014412d4303ce7c8db39becab076
    ```
    就是一个二分查找，告诉它当前版本是good还是bad，然后它就可以帮你一直查找下去。
49. 一个文件修改了两次，但是现在只想将第一次修改的内容添加到staged中

    ```
    git add -p feature.rb
    然后e进行编辑
    ```
50. 想切换会上次的branch，但是忘记了上次是哪一个branch了

    ```
    git reflog
    ```
    可以看到branch的切换记录
51. 有一次commit现在想撤销掉，但是不是最后一次commit。只想撤销这一次，别的都保留着。

    ```
    git revert 50210090d2333b341f533adf40eba709ffca75f5
    ```
52. 不小心从将当前的commit reset了，又想回到reset之前的那个commit

    ```
    git reflog  #查看reset之前的那个版本的hash code
    git checkout fc97597
    ```
53. merge两个branch，并解决冲突

    ```
    git merge mybranch  #在master branch下
    然后边界poem.txt文件，解决冲突
    ```
54. 要添加另一个git项目到本项目里，但是不要本项目的git统计该项目

    ```
    git submodule add https://github.com/jackmaney/githug-include-me ./githug-include-me
    ```
55. 给githug项目贡献代码