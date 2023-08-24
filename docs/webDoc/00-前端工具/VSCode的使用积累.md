### 在本地开启服务器

```bash
# 安装
npm install -g live-server

# 启动
live-server
```

参考链接：[Visual Studio Code + live-server 编辑和浏览 HTML 网页](http://www.cnblogs.com/1zhk/p/5699379.html)

### sftp：文件传输

输入快捷键「ctrl+shift+P」，弹出指令窗口，输入`sftp:config`，回车，当前工作工程的`.vscode`文件夹下就会自动生成一个`sftp.json`文件，我们需要在这个文件里配置的是：

- `host`：服务器的 IP 地址

- `username`：工作站自己的用户名

- `privateKeyPath`：存放在本地的已配置好的用于登录工作站的密钥文件（也可以是 ppk 文件）

- `remotePath`：工作站上与本地工程同步的文件夹路径，需要和本地工程文件根目录同名，且在使用 sftp 上传文件之前，要手动在工作站上 mkdir 生成这个根目录

- `ignore`：指定在使用 sftp: sync to remote 的时候忽略的文件及文件夹，注意每一行后面有逗号，最后一行没有逗号

举例如下：(注意，其中的注释不能保留)

```json
{
	"host": "", //服务器ip
	"port": 22, //端口，sftp模式是22
	"username": "", //用户名
	"password": "", //密码
	"protocol": "sftp", //模式
	"agent": null,
	"privateKeyPath": null,
	"passphrase": null,
	"passive": false,
	"interactiveAuth": false,
	"remotePath": "/root/node/build/", //服务器上的文件地址
	"context": "./server/build", //本地的文件地址

	"uploadOnSave": true, //监听保存并上传
	"syncMode": "update",
	"watcher": {
		//监听外部文件
		"files": false, //外部文件的绝对路径
		"autoUpload": false,
		"autoDelete": false
	},
	"ignore": [
		//忽略项
		"**/.vscode/**",
		"**/.git/**",
		"**/.DS_Store"
	]
}
```

### Express

在本地开启 Node 服务器：

![](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/20180611_2230.png)

然后在浏览器的地址栏输入`http://localhost/` + 文件的相对路径，就可以通过服务器的形式打开这个文件。

### Copy Relative Path

> 这个插件很有用，使用频率很高。

复制文件的相对路径：（相对于根路径而言）

![](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/20180611_2235.png)

### Auto Rename Tag

适用于 JSX、Vue、HTML。在修改标签名时，能在你修改开始（结束）标签的时候修改对应的结束（开始）标签，帮你减少 50% 的击键。

###Project Manager

项目管理，让我们方便的在命令面板中切换项目文件夹，当然，你也可以直接打开包含多个项目的父级文件夹，但这样可能会让 VSCode 变慢。

### highlight-icemode：选中相同的代码时，让高亮显示更加明显【荐】

VSCode 自带的高亮显示，实在是不够显眼。用插件支持一下吧。

所用了这个插件之后，VS Code 自带的高亮就可以关掉了：

在用户设置里添加`"editor.selectionHighlight": false`即可。

参考链接：[vscode 选中后相同内容高亮插件推荐](https://blog.csdn.net/palmer_kai/article/details/79548164)

### highlight-words：全局高亮（跨文件多色彩）

参考链接：[Visual Studio Code 全局高亮着色插件(跨文件多色彩)经验纪要](https://zhuanlan.zhihu.com/p/31163793)

### vetur：vue 文件的基本语法高亮

安装完 vetur 后还需要加上这样一段配置下：

```
"emmet.syntaxProfiles": {
  "vue-html": "html",
  "vue": "html"
}
```

参考链接：

- <https://www.clarencep.com/2017/03/18/edit-vue-file-via-vscode/>

- <https://github.com/varHarrie/varharrie.github.io/issues/10>

- <https://www.jianshu.com/p/0724921285d4>

- <https://www.cnblogs.com/AmosLee94/p/8338013.html>
