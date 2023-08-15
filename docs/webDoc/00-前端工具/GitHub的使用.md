# GitHub 添加 wiki

目前大家在 GitHub 上发布的项目，一般使用 Markdown 来编写项目文档和 README.md 等。Markdown 一般情况下能够满足我们的文档编写需求，如果使用得当的话，效果也非常棒。不过当项目文档比较长的时候，阅读体验可能就不是那么理想了，这种情况我想大家应该都曾经遇到过。

GitHub 每一个项目都有一个独立完整的 Wiki 页面，我们可以用它来实现项目信息管理，为项目提供更加完善的文档。我们可以把 Wiki 作为项目文档的一个重要组成部分，将冗长、具体的文档整理成 Wiki，将精简的、概述性的内容，放到项目中或是 README.md 里。

## 一. Wiki 简介

> Wiki 是一种在网络上开放且可供多人协同创作的超文本系统，由沃德·坎宁安于 1995 年首先开发，这种超文本系统支持面向社群的协作式写作，同时也包括一组支持这种写作。Wiki 站点可以有多人（甚至任何访问者）维护，每个人都可以发表自己的意见，或者对共同的主题进行扩展或者探讨。

上面这段描述引用自 [百度百科](https://link.juejin.cn?target=http%3A%2F%2Fbaike.baidu.com%2Fitem%2Fwiki%2F97755)，嗯，实际上百度百科本身也是一个 Wiki，最著名的 Wiki 大概是是 [维基百科](https://link.juejin.cn?target=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%E7%BB%B4%E5%9F%BA%E7%99%BE%E7%A7%91) 了吧。

然后 Wiki 页面效果大概可以参考 [Kingfisher](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fonevcat%2FKingfisher%2Fwiki)，看起来还是非常棒的：

![Kingfisher 的 Wiki 页面](../../图床/1604e8c38a485ac5tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

## 二. Wiki 的开启和关闭

GitHub 项目的 Wikis 功能默认是开启的，如果你没有找到 Wiki 选项卡，可能是因为该项目关闭了 Wikis 选项，在项目 Setting 中将其选中即可，如图所示：

![Wikis 开关](../../图床/1604e8c38bd25414tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

如果在之后某一天决定不再继续使用 Wikis 也可以通过取消该功能的勾选将其关闭，即使已经添加了 Wiki 页面也可以。并且会保存之前的 Wiki 页面内容，即关闭 Wiki 功能并不会清除内容，还可以随时再打开。

## 三. 创建和编辑页面

GitHub 的 Wiki 页面在如图所示选项卡下，默认应该是开启的，但是是空的，我们可以点击中间那个绿色的 `Create the first page` 按钮创建一个页面。

![创建 Wiki 页面](../../图床/1604e8c38afd48bftplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

如果你没有找到 Wiki 选项卡，可能是因为该项目关闭了 Wikis 选项，在项目 Setting 中将其选中即可，参考上文内容。

点击 `Create the first page` 按钮后会进入 Create new page 页面：

![Create new page](../../图床/1604e8c38950af54tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

从上往下进行介绍，顶部的输入框是页面标题；Edit mode 控制编辑页面的标记语言类型，这里默认的是 Markdown，支持的类型如下图所示：

![Edit mode 下拉列表](../../图床/1604e8c38d9e9900tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

中间的是页面内容，我们可以用 Edit mode 选择的语法在这里编写页面内容；底部编辑框用来输入本次编辑保存时的提交信息；编辑完成后点击 `Save Page` 按钮即可保存，唔，保存前可以先切换到 Preview 选项卡下进行预览，看一下效果是否是自己想要的。

然后保存我们新建的页面，大概会是如下效果：

![新建页面完成](../../图床/1604e8c38b95eee6tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

点击右上角的 `Edit` 按钮可以对当前页面进行编辑，也可以点击 `New Page` 按钮继续添加新的页面。

唔，这里有一点需要注意的是，默认的主页标题必须为 Home，如果不存在标题为 Home 的页面，切换到项目的 Wiki 选项卡时，会显示一个所有页面组成的列表。所以我们的主页必须以 Home 为标题。

![image.png](../../图床/1604e8c3bb00c450tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

目前好像没什么内容，感觉比较空额，不过没关系，接下来我们会一步步完善。

## 四. 添加页脚

点击 Wiki 页面底部的 `Add a custom footer` 按钮，进入新建页脚页面，如图所示：

![Add a custom footer](../../图床/1604e8c3b8891323tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

新建页脚页面实际上就是一个普通的 Create new page 页面，不过标题需要设为 \_Footer 并且不能修改（如果修改了就不会被当作页脚来处理了）。

我们可以参考 Kingfisher 的页脚代码，放置多个超链接在这里供读者在阅读完某一页后快速跳转到关键的章节或页面去，具体代码和效果如下：

```ruby
[Installation](https://github.com/onevcat/Kingfisher/wiki/Installation-Guide) - [Cheat Sheet](https://github.com/onevcat/Kingfisher/wiki/Cheat-Sheet) - [FAQ](https://github.com/onevcat/Kingfisher/wiki/FAQ) - [API Reference](http://onevcat.github.io/Kingfisher/)
复制代码
```

![Kingfisher 页脚效果](../../图床/1604e8c3ba3f6641tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

当然也可以放一些奇怪的东西，比如，这样的：

![+1s](../../图床/1604e8c3bbe62153tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

如上图所示，点击页脚右侧的编辑按钮，就可以对页脚进行编辑啦，很方便。

## 五. 添加侧边栏

点击右侧的 `Add a custom sidebar` 按钮可以添加侧边栏，和页脚同理，页面名为特殊的 \_Sidebar：

![Add a custom sidebar](../../图床/1604e8c3bce1dbfctplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

我们可以参考 Kingfisher 的侧边栏实现，代码和效果如下：

```markdown
## Getting Started

- [Getting Started with Kingfisher](https://github.com/onevcat/Kingfisher/wiki/Getting-Started-with-Kingfisher)
  - [Install Kingfisher](https://github.com/onevcat/Kingfisher/wiki/Installation-Guide)
  - [Cheat Sheet](https://github.com/onevcat/Kingfisher/wiki/Cheat-Sheet)
- [API Reference](http://onevcat.github.io/Kingfisher/)

## Migration Guide

- [3.0 Migration Guide](https://github.com/onevcat/Kingfisher/wiki/Kingfisher-3.0-Migration-Guide)
- [2.0 Migration Guide](https://github.com/onevcat/Kingfisher/wiki/Kingfisher-2.0-Migration-Guide)

## Communication

- [FAQ](https://github.com/onevcat/Kingfisher/wiki/FAQ)
- [Ask a question](http://stackoverflow.com/search?q=kingfisher)
- [Submit an issue](https://github.com/onevcat/Kingfisher/issues/new)
- [Open a pull request](https://github.com/onevcat/Kingfisher/compare)

## Information

- [Change Log](https://github.com/onevcat/Kingfisher/blob/master/CHANGELOG.md)
  复制代码
```

![Kingfisher 的侧边栏](../../图床/1604e8c3be044cedtplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

这里的话可以自己适当摸索一下，调整标题层级等样式，以获得一个自己比较满意的展示效果。同样的，点击侧边栏右上角的编辑按钮可以对快速侧边栏进行在线编辑。

![侧边栏编辑按钮](../../图床/1604e8c3e63d7ac2tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

## 六. 查看编辑历史

进入某个页面的编辑页面，点击右上角的 `Page History` 按钮，可以查看该页面的编辑历史，如下图所示：

![Page History 按钮](../../图床/1604e8c3e5863c89tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

![编辑历史页面](../../图床/1604e8c3ea7290e1tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

## 七. 权限控制

那么问题来了，既然是 Wiki 的话，为啥以上这些内容完全是项目所有者一个人手撸呢，完全没有体现出「多人协作」的特性啊喂。

嗯，GitHub Wiki 是可以开放给所有人编辑权限的，不过默认是只有项目所有者和合作者才有权限编辑的，只要到 Setting 中将 Restrict editing to collaborators only 选项去除勾选即可。

![Restrict editing to collaborators only](../../图床/1604e8c3e5c2704etplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

这样的话，只要有 GitHub 账号的用户，都可以对该项目的 Wiki 进行编辑。如果怕被胡乱篡改，不想开放编辑权限的话，还是保持勾选好了。

## 八. 本地编辑

唔，上文内容一直在介绍 Wiki 的在线编辑，实际上 Wiki 是一个单独的 Git 仓库，可以 Clone 到本地进行操作

### 1. Wiki 仓库下载

细心的同学应该已经注意到了，Wiki 的右下角处有当前 Wiki 的 Git 仓库地址（我们也可以通过该方法下载他人所属的 Wiki 页面的源代码）：

![Wiki 仓库地址](../../图床/1604e8c48b3fb8b8tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

Kingfisher 的 Wiki 仓库结构如下：

![Kingfisher Wiki 结构](../../图床/1604e8c46465a2dftplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

接下来就可以直接对 Wiki 页面源文件进行编辑了，实际上就是一堆 Markdown 文件的组合（或者其他比标记语言，看你选的是啥了）。

### 2. 本地预览

我们在本地手动编辑编辑完成后，只能通过 push 到 GitHub 的方式进行预览，非常不方便，这个时候，就需要借助一个叫 [gollum](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fgollum%2Fgollum) 的工具了。

Gollum 是 GitHub 上用到的 Wiki 引擎，使用它可以在本地上搭建一个类似的 GitHub Wiki 的网站，对本地的 Wiki 页面进行快速预览。执行以下命令即可安装：

```
sudo gem install gollum
复制代码
```

安装完成后，将路径切换到 Wiki 的 Git 仓库下然后执行 `gollum` 命令，然后访问 http://127.0.0.1:4567/ 即可进行预览。

![Gollum 预览](../../图床/1604e8c4739d37e0tplv-t2oaga2asx-zoom-in-crop-mark4536000.webp)

## 九. 其他

Wiki 不仅仅可以作为项目辅助工具来用，你也可以把它当作一个个人信息知识库来使用，不需要搭建，不需要部署，无需付费，方便快捷，更多功鞥大家可以自行开发。

如果你觉得上文的报道，哦不，描述可能有偏差，[GitHub Wiki 的帮助文档](https://link.juejin.cn?target=https%3A%2F%2Fhelp.github.com%2Fcategories%2Fwiki%2F) 也许能给你带来一些帮助

# GitHub 项目添加 license

- 当我们在 GitHub 浏览一些开源项目时，我们经常会看到这样的标志：

  ![img](../../图床/20170828202102117.png)

  如上图所示，`Apache-2.0`，我们可以将其称之为**开源许可证**，那么到底开源许可证是什么呢？

  - **开源许可证即授权条款**。开源软件并非完全没有限制。最基本的限制，就是开源软件强迫任何使用和修改该软件的人承认发起人的著作权和所有参与人的贡献。任何人拥有可以自由复制、修改、使用这些源代码的权利，不得设置针对任何人或团体领域的限制；不得限制开源软件的商业使用等。而许可证就是这样一个保证这些限制的法律文件。

  常见的开源许可证包括：

  - `Apache License 2.0`
  - `GNU General Public License v3.0`
  - `MIT License`
    开源许可证种类很多，以上三个许可证是比较常用的。至于 GitHub 都允许什么类型的许可证，以这个项目`cg-favorite-list`为例：

  ![cg](../../图床/20170828204109078.png)

  如上图所示，在项目首页，点击`Create new file`，创建名为`LICENSE`文件：

  ![License](../../图床/20170828204349709.png)

  实际上，当我们键入`LICENSE`文件名的时候，GitHub 就已经自动提示`Choose a license template`选项啦，点击进入：

  ![show](../../图床/20170828204851932.png)

  如上图所示，最左侧展示了 GitHub 可以选择的开源许可证名称，以`MIT License`为例，点击之后，中间部分显示具体开源许可证的内容。在此处，我们可以自由选择自己想要的许可证，然后点击`Review and submit`：

  ![1](../../图床/20170828205405503.png)

  ![2](../../图床/20170828205416866.png)

  标注 1：`Commit directly to the master branch.`
  标注 2：`Create a new branch for this commit and start a pull request`.
  如上图所示，在这里，我们有两个选择。如果我们选择 **标注 1** 所示的内容，则直接将此许可证提交到`master`分支；如果我们选择 **标注 2** 所示的内容，则是新建立一个分支，然后我们可以提 PR 到`master`，再进行合并。在此，我们选择 **标注 1** 所示的内容，直接将`MIT License`提交到`master`分支：

  ![img](../../图床/20170828210100351.png)

  如上图所示，我们已经为`cg-favorite-list`项目创建了一个开源许可证。

# GitHub 引用图片的另一种方式

先将一张图片加入到 git 仓库，在复制地址栏的 url

github 和 md 文件关联的图片地址是有一定的格式的，其格式如下：

https://github.com/用户名/repository仓库名/raw/分支名master/图片文件夹名称/`***`.png or`***`.jpg

按照此格式 github 会自动解析这个语法，并把图片在 md 文件中正常显示出来。

让我们来试一试，先拿一张 git 仓库的图片

![image-20230323104831457](../../图床/image-20230323104831457.png)

![测试图片](https://github.com/zhanghaooss/my-webdoc/raw/master/%E5%9B%BE%E5%BA%8A/1669619974892.png)

完美显示！！！

上传图床试验一下

![image-20230323104831457](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/image-20230323104831457.png)
