---
title: 02-Git的使用
---

[TOC]

## 概述

### 代码管理工具

`svn`-**集成式**，不能离线；一脱离服务器就无法对比、回退代码；每一次提交都会被记录；svn有锁 右键加锁 提交完 释放锁；服务器崩了就都无了。

它是一个过渡产品

`git` -**分布式** ，<u>比svn多一个本地仓库</u> ；分布在每台电脑的本地仓库都有备份；防止服务器宕机造成代码损失。

## 常见操作

### 全局配置用户信息

同样可以用来重新配置修改

```bash
git config --global user.name "Yubei"

git config --global user.email "zhanggeyu030@gmail.com"
```

### 上传代码

打开git命令行窗口     

ps：键盘的上下键可以翻出来敲下的命令

```
新建文件夹->Git Bash Here
```



* [ ] 初始化本地仓库 (显示隐藏目录)


```bash
git init
```

<u>.git 文件夹</u> 代表本地仓库，删了文件夹就会删掉本地仓库 

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317110603510.png" alt="image-20230317110603510" style="zoom:67%;" />

* [ ] **三步上传代码**

首先为本地仓库添加一个源 origin自定义命名 只要提交一次命令

```bash
git remote add origin https://gitee.com/zhang-aoh/test1028.git
```

1. 从代码区添加到暂存区

```bash
git add  文件1 文件2
git add . // 添加所有文件 自动提交有修改的文件
```

2. 从暂存区添加到本地仓库

   > 不添加注释会跳进一个linux编辑器 i编写` esc+:+w+q` 退出

```bash
git commit -m '第一次提交'`// 做个标记提交注释
```

3. 提交推送到远程仓库

```bash
git push 远程仓库地址 分支名称 //第一次需要登录远程仓库的账户密码
git push -u origin master //绑定仓库后首次提交这样写，意思是：(本地)master:(远程)master
git push //之后这样写
```

> 第一次push要弹出**输入用户名密码**  就是码云/GitHub的用户名密码，第二次开始不用打`-u`，**第一次输错**会显示 `authencitated failed `验证失败，解决： win10如何删除凭证：删除之前的记录，打开 控制面板 ->凭据管理器 ->windows凭据，提交成功后，window会记录账号，密码。



* [ ] **克隆拉取仓库**

```bash
git clone 仓库地址
```



* [ ] **拉取一个分支到本地仓库**

```bash
//小义：
//拉取分支代码开始新的工作
git pull 远程仓库 分支
```

### 场景应用：在团队合作中

> ```bash
> //小甲：
> //确保本地有自己的仓库
> 
> //1.拉取别人的代码
> //每天开始工作前，首先使用git pull命令拉取最新的代码。这样你就会得到其他成员在前一天提交的代码更新。
> git status //在拉取最新代码之前，当前是否有未提交的更改。
> //若没有未提交的更改
> git pull //拉取代码
> //！！处理每日开始工作之前的代码冲突
> //在拉取最新代码之后，如果有冲突，Git会在文件中标记冲突的部分。手动编辑这些文件，解决冲突。
> git add //将解决冲突后的文件标记为已解决。
> git commit //提交解决冲突后的代码
> 
> //2.提交自己的代码：
> //你对你的功能进行修改和开发。
> git add //命令将修改的文件添加到暂存区。
> git commit -m "Implement new feature" //提交修改到你的本地仓库。
> 
> //3.！！处理每日结束前的代码冲突
> git pull //在提交你的代码之前，你再次使用git pull命令拉取最新的代码，以确保你的代码与其他人的代码保持同步。
> //如果有冲突，Git会在冲突文件中标记冲突的部分。你打开这些文件，手动解决冲突，并将文件修改为期望的代码状态。
> git add //一旦你解决了所有冲突，你使用git add命令将解决冲突的文件标记为已解决。
> git commit -m "Resolve merge conflicts" //提交解决冲突后的代码。
> 
> //4.分支合并，例如：把自己的分支合并到主分支
> //假设有两个分支：branchA和branchB。
> git checkout branchB //切换到branchB分支
> git merge branchA //将branchA合并到当前分支（branchB）上
> //如果没有冲突，Git会自动合并分支。如果有冲突，需要手动解决冲突，然后使用git add和git commit命令提交合并后的代码。
> ```
>
> 总结为：每天先拉取最新代码-处理冲突-开始完成功能-提交自己的功能前先拉取-处理冲突-最终提交-（分支合并）

### 参考命令

```bash
git status  //   实时告诉你git此时的提交状态是什么

git log // 查看提交记录 查看当前及以前的版本提交记录
git reflog  // 记录操作 复制前面的7位版本号 查看所有的提交记录和回退记录

git reset --hard HEAD^ // 回退到上一个版本 ^可以改为~1、~2....或者更多^^
git reset --hard 3788bb7 //回退指定版本 3788bb7应该是哪一个版本ID ，可以从reflog提取

git diff 文件名 //找不同和仓库中的作比较   不太直观
 
git clone 仓库地址 // 克隆命令

git config --global --unset http.proxy //取消代理
git config --global --unset https.proxy
```

---

**参考命令使用案例**

以下是操作过程

```
1. 添加1.html 添加到暂存区 ，提交到本地仓库
2. 添加2.html 添加到暂存区 ，提交到本地仓库
3. 修改1.html 添加到暂存区 ，提交到本地仓库
4. 添加/image目录，添加两张图片，添加到暂存区 ，提交到本地仓库
```



* [ ] 回退指定版本

```bash
git reset --hard hard 提交版本ID
```

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230716162319049.png" alt="image-20230716162319049" style="zoom:67%;" />

`回退到上一版`

<img src="C:/Users/29439/AppData/Roaming/Typora/typora-user-images/image-20230716162524851.png" alt="image-20230716162524851" style="zoom:67%;" />

`回退到最初版`

<img src="C:/Users/29439/AppData/Roaming/Typora/typora-user-images/image-20230716162844471.png" alt="image-20230716162844471" style="zoom:67%;" />

**存在的问题**：`git log`这个命令没法看之后的版本，我们换用`git reflog ` 可以使用前面的7位版本号

<img src="C:/Users/29439/AppData/Roaming/Typora/typora-user-images/image-20230716163516041.png" alt="image-20230716163516041" style="zoom:67%;" />

这时候版本回到了第四版 ，再次查看`git log` 有了四次记录

![image-20230317104438535](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317104438535.png)

## 仓库分类

| 仓库类型                        | 描述                                                         | 用途                                                     |
| ------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------- |
| 公共仓库（Public Repository）   | 对所有人开放的仓库，任何人都可以访问和克隆。                 | - 开源项目的托管和共享<br>- 提供给其他人查看和贡献代码   |
| 私有仓库（Private Repository）  | 仅限特定人员或团队访问和克隆，具有保密性和安全性。           | - 商业项目的私有开发和托管<br>- 保护保密性代码的安全     |
| 个人仓库（Personal Repository） | 由个人开发者创建和维护的仓库，用于个人项目或学习笔记。       | - 个人项目的管理和版本控制<br>- 个人代码和学习资料的存储 |
| 团队仓库（Team Repository）     | 多人协作开发的中央存储库，用于团队共享和管理代码。           | - 团队成员之间的协作和交流<br>- 问题跟踪和代码审查       |
| 分叉仓库（Fork Repository）     | 从原始仓库（通常是公共仓库）派生出的副本，用于独立开发和贡献代码。 | - 在自己账号下进行独立开发<br>- 向原始仓库提交修改       |

按平台分

> 公共仓库
>
> * [github](https://github.com/)、[Gitee](https://gitee.com/dashboard)、gitlab、gitchina...(分公有私有)
>
> 公司私有仓库
>

## 分支的合并

**为什么要进行分支合并**

1. 创建子分支（Feature Branch）：为了进行并行开发，首先从主分支（例如`master`）创建一个新的子分支（例如`feature-branch`）。每个子分支可以用于独立的功能开发或修复bug。
2. 开发新功能和修复bug：在子分支中，团队成员可以独立地进行新功能的开发或修复bug。每个成员在自己的子分支中进行工作，不会影响到主分支和其他成员的工作。
3. 合并子分支到主分支：当某个子分支开发完成并经过测试验证后，可以将该子分支合并回主分支。这可以通过以下步骤完成：
   * 切换到主分支：`git checkout master`。
   * 使用`git merge`命令将子分支合并到主分支：`git merge feature-branch`。
   * 解决合并冲突（如果有）并提交合并结果。
4. 继续开发新功能：在子分支合并到主分支后，老分支可以继续进行新功能的开发。这样可以保持团队的并行开发进行，不会受到其他分支的影响。
5. 创建新的分支进行实验：如果有需要尝试新的实验性功能或进行其他实验性工作，可以从主分支创建一个新的分支。在这个分支上进行实验，不影响主线开发的稳定性。这样可以实现"0成本影分身"，即在不影响主分支的前提下进行实验性开发。

总结来说，<u>通过创建子分支进行独立的功能开发或修复bug，并定期将子分支合并回主分支，可以实现在只有一个主分支的情况下进行并行开发。同时，根据需要可以创建新的分支进行实验性开发，确保不影响主线开发的稳定性。</u>

### 分支相关命令

1. 显示分支

```bash
git branch //它会显示所有本地分支，并用星号(*)标记当前所在的分支。
git branch -a //会显示本地和远程的分支
```

2. 新建分支 ，拷贝当前分支已经提交过的最新版本中的文件到一个新的分支

```bash
git branch 分支名字 
```

![image-20230716165945645](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230716165945645.png)

蓝色括号表示当前分支，在当前分支（master）下创建子分支porduct，并复制最新提交过的文件到product

3. 进入分支 ，不同分支的工作区都是独立的


```bash
git checkout 分支名称

git checkout -b mingzi //创建新分支 并切换到新分支  
git checkout mingzi //不加-b 就是切换分支 文件夹自动根据分支变的
//先切换到master，合到主分支
```

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317141707018.png" alt="image-20230317141707018" style="zoom:150%;" />

4. 分支开发完

![image-20230317142022560](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317142022560.png)

<u>切换分支之前确保 working tree clean,且**关闭编辑器文件**</u>

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317142320981.png" alt="image-20230317142320981"  />



5. 合并分支

```bash
git merge 分支名
```

合并过程中可能会出现冲突，需要手动打开有冲突的文件并解决，然后重新提交到仓库

<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317142320981.png" alt="image-20230317142320981"  />



<img src="https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317145956834.png" alt="image-20230317145956834"  />

手动打开解决冲突

![image-20230317145915094](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317145915094.png)

修改完成后

![image-20230317150456513](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317150456513.png)

过程中遇到please tell me why...   解决 `:q!`回车退出

6. 合并完成要想删除分支

```bash
//删除本地子分支
git checkout master

git branch -d mingzi  //删除已经合并的分支
git branch -D mingzi  //强制删除分支，不管是否已经合并
git branch -a //检查一遍
```

7. 远程仓库（代码托管平台）

```bash
git push origin master

git push origin mingzi:mingzi //push服务器新分支

git push origin :mingzi //删除仓库新分支
```

![image-20230317155557556](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230317155557556.png)

### 场景应用：基于master分支的代码，开发一个新的特性

如果你直接在master分支上开发这个新特性，是不好的，万一你在开发`特性1`的时候，领导突然又要叫你去开发`特性2`，就不好处理了。难道开发的两个特性都提交到master？一会儿提交特性1的commit，一会儿提交特性2的commit？这会导致commit记录很混乱。

所以，我给你的建议做法是：给每个特性都单独建一个的新的分支。

比如说，我专门给`特性1`建一个分支`feature_item_recommend`。具体做法如下：

（1）基于master分支，创建一个新的分支，起名为`feature_item_recommend`：

```bash
$ git checkout -b feature_item_recommend

Switched to a new branch 'feature_item_recommend'
```

上面这行命令，相当于：


```bash
$ git branch feature_item_recommend    // 创建新的分支

$ git checkout feature_item_recommend  //切换到新的分支
```


（2）在新的分支`feature_item_recommend`上，完成开发工作，并 commit 、push。

（3）将分支`feature_item_recommend`上的开发进度**合并**到master分支：

```bash
$ git checkout master  //切换到master分支

$ git merge feature_item_recommend    //将分支 feature_item_recommend 的开发进度合并到 master 分支

```


合并之后，`master`分支和`feature_item_recommend`分支会指向同一个位置。


（3）删除分支`feature_item_recommend`：

> 既然 特性1 开发完了，也放心地提交到master了，那我们就可以将这个分支删除了。

```bash
git branch -d feature_item_recommend
```

注意，我们当前是处于`master`分支的位置，来删除`feature_item_recommend`分支。如果当前是处于`feature_item_recommend`分支，是没办法删除它自己的。

同理，当我转身去开发`特性2`的时候，也是采用同样的步骤。



* [ ] 合并分支时，如果存在分叉

```bash
$ git checkout master
$ git merge iss53
```



* [ ] 解决合并时发生的冲突

如果 feature1和feature2修改的是同一个文件中**代码的同一个位置**，那么，把feature1合并到feature2时，就会产生冲突。这个冲突需要人工解决。步骤如下：

（1）手动修改文件：手动修改冲突的那个文件，决定到底要用哪个分支的代码。

（2）git add：解决好冲突后，输入`git status`，会提示`Unmerged paths`。这个时候，输入`git add`即可，表示：**修改冲突成功，加入暂存区**。

（3）git commit 提交。

然后，我们可以继续把 feature1 分支合并到 master分支，最后删除feature1、feature2。

**注意**：两个分支的同一个文件的不同地方合并时，git会自动合并，不会产生冲突。

比如分支feture1对index.html原来的第二行之前加入了一段代码。
分支feature2对index.html在原来的最后一行的后面加入了一段代码。
这个时候在对两个分支合并，git不会产生冲突，因为两个分支是修改同一文件的不同位置。
git自动合并成功。不管是git自动合并成功，还是在人工解决冲突下合并成功，提交之前，都要对代码进行测试。

## git协作

* [ ] 没有冲突

复制出来自己远程仓库的地址给到组员
克隆命令
git clone 仓库地址

> 有时候会遇到克隆失败报错
>
> ![image-20230716172742517](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230716172742517.png)
>
> [参考社区解决](https://stackoverflow.com/questions/63727594/github-git-checkout-returns-error-invalid-path-on-windows)

公司内克隆下载失败 

(私有)   管理---仓库成员管理---免费版5个人  直接添加 用户名
协作者能直接上传和下载个人私有代码  ---给与开发者权限
管理员权限能够删除整个项目，权限一删就全拜拜了，历史记录也会下载

```bash
可以看log
q键退出log打印

git init 只有第一个创建项目的人才用 

cd进入含有.log文件的目录

ls  查看当前文件是否有子文件
```

每天开始写代码时养成好习惯 **先pull服务器的最新代码**，没保存到本地仓库就pull代码容易出事！！！如果两人都修改同一文件 就会覆盖，**每天写之前pull!! 然后再push**，你改你的 我改我的文件部分 

```bash
git add.

git commit -m '注释'

git push origin master  //这里就不用添加源了，克隆的时候就添加了  加个-f 如果别人先提交，会覆盖别人的代码，所以提交前先同步一下代码 
git pull origin master  //同步  新的代码合到本地  拉取更新  原则最新的覆盖老的
合并完加条注释

跳进一个linux编辑器 i编写 esc+:+w+q 退出

同步完在最新的仓库代码基础上git push origin -m '11111111'

```



* [ ] 发生代码冲突

日常提交拉取代码时候和别人的代码发生冲突

> ps:两人修改同一文件例如修改了同一路由文件，按正常流程 第二次push时候，pull下最新的代码和本地最新的代码冲突了，有白字提示
>
> ```bash
> CONfLICT(content):Merge conflict in 文件名
> Automatic merge failed  `//自动合并失败
> ```
>
> 工具会都给你留下产生冲突的代码，让你选择进行手动对比；package.json文件就容易冲突



## ！！日常操作积累

### 修改密码（曲线救国）


> 网上查了很久，没找到答案。最终，在cld童鞋的提示下，采取如下方式进行曲线救国。

```bash
# 设置当前仓库的用户名为空
git config  user.name ""
```

然后，当我们再输入`git pull`等命令行时，就会被要求重新输入*新的*账号密码。此时，密码就可以修改成功了。最后，我们还要输入如下命令，还原当前仓库的用户名：

```bash
git config user.name "yubei"
```

### 修改已经push的某次commit的作者信息

> 要修改已经 push 的某次 commit 的作者信息，需要执行以下步骤：
>
> 1. 首先，确保你有足够的权限来更改远程仓库的提交记录。如果你是仓库的拥有者或有管理员权限，那么你可以执行下面的步骤。如果你不是仓库的拥有者或没有足够的权限，你需要联系仓库的管理员或拥有者，请求他们帮助修改作者信息。
>
> 2. 在本地，使用 `git rebase -i` 命令打开交互式 rebase 编辑器，以便修改提交历史。你需要指定要修改作者信息的提交之前的提交哈希。
>
>    ```
>    cssCopy code
>    git rebase -i <commit-hash>
>    ```
>
>    注意：如果你想修改最早的提交，可以使用 `git rebase -i --root`。
>
> 3. 在交互式 rebase 编辑器中，将需要修改的提交行的操作改为 `edit`。
>
> 4. 保存并关闭编辑器，Git 会将你的分支切换到指定的提交。
>
> 5. 执行 `git commit --amend --author="New Author Name <email@example.com>"` 命令，用你想要修改的新作者信息替换 "New Author Name" 和 "[email@example.com](mailto:email@example.com)"。
>
> 6. 使用 `git rebase --continue` 命令继续 rebase 过程。
>
> 7. 如果你修改的是已经 push 过的提交，那么你需要使用 `git push --force` 命令将修改后的提交强制推送到远程仓库。这将会替换原来的提交历史，所以请谨慎使用这个命令，并确保与团队成员沟通。
>
> **注意**：修改提交的作者信息会改变提交的哈希值，因此修改后的提交不再与之前的提交相同。这可能会导致一些问题，例如团队成员在基于旧的提交进行工作，或与其他远程分支存在依赖关系等。所以，在执行这个操作之前，请确保与团队成员进行沟通，并确认修改不会影响到其他人的工作。
>
> 总结起来，要修改已经 push 的某次 commit 的作者信息，你需要使用交互式 rebase 来编辑提交历史，并使用 `git commit --amend --author` 命令修改作者信息。最后，使用 `git push --force` 强制推送修改后的提交到远程仓库。

### 将 `branch1`的某个`commit1`合并到`branch2`当中

切换到branch2中，然后执行如下命令：

```bash
git cherry-pick commit1
```

### 修改GitHub已提交的用户名和邮箱

> 要修改已在 GitHub 上提交的用户名和邮箱，可以按照以下步骤进行操作：
>
> 1. 打开终端或命令行界面，进入你的本地代码仓库所在的目录。
>
> 2. 确认你的 Git 配置信息，包括用户名和邮箱。可以运行以下命令查看：
>
>    ```
>    arduinoCopy codegit config user.name
>    git config user.email
>    ```
>
>    这将显示当前配置的用户名和邮箱。
>
> 3. 如果需要修改用户名，可以运行以下命令：
>
>    ```
>    arduinoCopy code
>    git config --global user.name "New Username"
>    ```
>
>    将 "New Username" 替换为你想要设置的新用户名。使用 `--global` 标志可以将修改应用到全局配置中，这样在其他仓库中也会生效。
>
> 4. 如果需要修改邮箱，可以运行以下命令：
>
>    ```
>    arduinoCopy code
>    git config --global user.email "newemail@example.com"
>    ```
>
>    将 "[newemail@example.com](mailto:newemail@example.com)" 替换为你想要设置的新邮箱地址。同样，使用 `--global` 标志将修改应用到全局配置。
>
> 5. 验证修改是否生效，可以再次运行以下命令：
>
>    ```
>    arduinoCopy codegit config user.name
>    git config user.email
>    ```
>
>    确认修改后的用户名和邮箱是否正确显示。
>
> 6. 最后，你需要使用修改后的用户名和邮箱重新提交之前的提交记录。这可以通过 Git 的 `commit --amend` 命令来实现。运行以下命令：
>
>    ```
>    cssCopy code
>    git commit --amend --reset-author --no-edit
>    ```
>
>    这将使用新的用户名和邮箱重新提交之前的提交记录。
>
> 7. 如果之前的提交已经被推送到 GitHub 上，你需要使用 `git push --force` 命令强制推送修改后的提交。请注意，强制推送会覆盖远程仓库中的历史记录，因此请谨慎使用，并确保与团队成员进行沟通。
>
> 完成上述步骤后，你的 GitHub 提交记录将显示新的用户名和邮箱。
>
> 请注意，这些修改只会影响你之后的提交记录，之前的提交记录不会自动更新。如果你需要对之前的提交记录进行修改，可以考虑使用 `git filter-branch` 或 `git filter-repo` 等工具来进行历史记录重写操作。这些操作可能比较复杂，建议在谨慎了解操作的影响和风险后再进行操作。

### 将Git 项目迁移到另一个仓库

我们假设旧仓库的项目名称叫`old-repository`，新仓库的项目名称叫`new-repository`。操作如下：


（1）创建旧仓库的裸克隆：

```bash
git clone --bare https://github.com/exampleuser/old-repository.git
```
执行上述命令后，会在本地生成一个名叫 `old-repository.git`的文件夹。


（2）迁移到新仓库：

```bash
cd old-repository.git

git push --mirror https://github.com/exampleuser/new-repository.git
```

这样的话，项目就已经迁移到新仓库了。

注意，我们**不需要**手动新建一个空的新仓库，当我们执行上述命令之后，新仓库就已经自动创建好了。

参考链接：

- [复制仓库](https://help.github.com/cn/github/creating-cloning-and-archiving-repositories/duplicating-a-repository)

- [Git 本地仓库和裸仓库](https://moelove.info/2016/12/04/Git-%E6%9C%AC%E5%9C%B0%E4%BB%93%E5%BA%93%E5%92%8C%E8%A3%B8%E4%BB%93%E5%BA%93/)

### 提交代码时，绕过 eslint 检查

需求：提交代码时，绕过 eslint 检查

解决办法：用命令行提交，末尾追加`--no-verify`。例如：

```bash
# 提交代码
git commit -m 'yubei的commit备注' --no-verify

# 推送到远程时，也可以追加 --no-verify，以免远程仓库做了 eslint 限制。
git push origin --no-verify
```

### 切换仓库的源地址

查看源地址：

```bash
git remote -v
```

切换源地址：

```bash
git remote set-url origin https://xxx.git
```

### vue项目提交git步骤

vue项目创建时就会自动有git init 所以就不用再敲这个命令

1. git add .

2. git commit -m 'maizuo' 

   > 提交前运行钩子hook，自动帮我修复格式 相当于`npm run lint`
   > 不提交node_modules推送  --->.gitignore 文件 git忽略

3. git remote add origin 地址

### 运行克隆回来的项目

任何人从git下载的vue react 项目都无法运行 
方法：

```bash
npm i 
打开package.json文件
找到启动的命令
```

### .gitignore忽略文件

在工作目录下，使用任意编辑器创建文件`.gitignore`文件，把要忽略的文件路径写进去即可

路径参考 git status命令下提示 

- [聊下git pull --rebase](https://www.cnblogs.com/wangiqngpei557/p/6056624.html)

### 解决 Git 不区分大小写导致的文件冲突问题

> Git 默认情况下对文件名的大小写是不敏感的，这可能导致在不同操作系统或不同分支中文件名大小写不同的情况下，出现文件冲突的问题。为了解决这个问题，可以尝试以下方法：
>
> 1. 修改本地配置：可以在 Git 的配置中设置 `core.ignorecase` 为 `false`，以区分文件名的大小写。运行以下命令来修改配置：
>
>    ```
>    arduinoCopy code
>    git config --global core.ignorecase false
>    ```
>
>    这样设置后，Git 将会对文件名大小写敏感，避免因文件名大小写不同而导致的冲突问题。
>
> 2. 重命名文件：如果已经出现了文件冲突，可以手动重命名冲突的文件，使其文件名大小写保持一致。通过重命名文件，Git 将会将其视为一个新文件，解决冲突。
>
>    例如，如果冲突的文件名是 `file.txt` 和 `File.txt`，你可以手动将其中一个文件重命名为与另一个文件名大小写相同，例如将其中一个文件重命名为 `file.txt`。
>
>    ```
>    phpCopy code
>    git mv <原文件名> <新文件名>
>    ```
>
>    然后提交这个重命名的更改。
>
> 3. 调整分支命名：如果是因为分支命名导致的文件冲突，可以考虑调整分支命名，使分支命名中的大小写保持一致。
>
>    例如，如果存在一个分支名为 `feature-branch`，而另一个分支名为 `Feature-branch`，可以将其中一个分支重命名为与另一个分支名大小写相同，例如将其中一个分支重命名为 `feature-branch`。
>
>    ```
>    phpCopy code
>    git branch -m <原分支名> <新分支名>
>    ```
>
>    然后推送重命名的分支到远程仓库。
>
> 以上方法可以帮助解决因 Git 不区分大小写导致的文件冲突问题。选择适合你情况的方法，并确保在团队成员之间进行协调和沟通，以避免未来出现类似的问题。

