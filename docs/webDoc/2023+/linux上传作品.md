## Linux云服务配置

### 一、用Linux云服务器

 购买<u>[华为云服务器](https://account.huaweicloud.com/usercenter/?locale=zh-cn&region=cn-east-3#/accountindex/realNameAuth?service=https:%2F%2Factivity.huaweicloud.com%2Fdiscount_area_v5%2Findex.html%3Futm_source%3Dgoogle%26utm_medium%3Dse-cpc-op%26utm_campaign%3D10056%26utm_content%3D%26utm_term%3D%25E5%258D%258E%25E4%25B8%25BA%25E4%25BA%2591%26utm_adplace%3DAdPlace067158%26gclid%3DCj0KCQjwoeemBhCfARIsADR2QCutoP0T_ZAyQ4oittjQd3bfE6_8OfbbhJ4dD-KRgytM_HzDVFgMhhUaAsXVEALw_wcB)</u>的步骤：

1. 百度里搜“华为云”，在华为云推广页购买￥39或￥99/年的云服务器https://activity.huaweicloud.com/ecs.html

2. 注册成为华为云用户，支付云服务器费用，选择云服务器的操作系统为UbuntuLinux

3. 登录华为云服务器(www.huaweicloud.com)，点击右上角的“控制台”，进入云服务器控制台

4. 在左上角的“云耀云服务器”服务器列表下找到自己的云服务器，在“更多”>“配置安全组”下面修改云服务器的“安全组策略”——即允许当前服务器的哪些端口被外界访问

5. 在云服务器控制台中点击“远程控制”，开始登录云服务器，进行远程控制

 

### 二、Linux系统日常操作命令

| **序号** | **命令名**                                                   | **全称**                                                     | **含义**                                                     |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1        | pwd                                                          | Print Working Directory                                      | 输出当前的工作目录                                           |
| 2        | ls                                                           | List                                                         | 列出当前目录下有哪些子目录/文件                              |
| 3        | ls  -a                                                       | List All                                                     | 列出当前目录下全部的内容（包括隐藏内容）                     |
| 4        | ls  -l                                                       | List Long                                                    | 完整显示出文件/目录的信息                                    |
| 5        | cd 目录名                                                    | Change Directory                                             | 进入指定的目录                                               |
| 6        | mkdir 目录名                                                 | Make Directory                                               | 创建一个新的文件夹                                           |
| 7        | touch  文件名                                                | Touch，触摸                                                  | 创建一个新的文件                                             |
| 8        | rm -rf 文件或目录名                                          | Remove，删除  Force，强制  Recursive，递归的                 | 删除指定的文件或目录                                         |
| 9        | cp 名1 名2                                                   | Copy                                                         | 复制指定的文件                                               |
| 10       | mv 名1 名2                                                   | Move                                                         | 移动文件到另一个地址或文件名，可用于重命名或移动到另一个目录下 |
| 11       | unzip 压缩包名                                               |                                                              | 解压缩指定的压缩文件                                         |
| 12       | vi/vim  文件名                                               | ![img](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/clip_image002.jpg) | Linux系统中自带的“记事本”程序                                |
| 13       | init  6                                                      |                                                              | 把系统状态转换到 6 模式 （即重启模式）                       |
| 14       | init  0                                                      |                                                              | 把系统状态转换到 0 模式 （即关机模式）                       |
| 15       | wget  URL地址                                                | www get                                                      | 从互联网上的指定地址处下载资源                               |
| 16       | ./可执行文件名                                               |                                                              | 执行当前目录下的一个可执行文件                               |
| 17       | chmod   +r 文件名  chmod   +w 文件名chmod   +x 文件名chmod -r 文件名chmod -w 文件名chmod  -x   文件名 | Change Mode   r(Read)   w(Write) x(Execute)                  | 修改文件的权限                                               |
| 18       | netstat   -anp                                               | Network Statistics                                           | 网络端口占用情况统计，以及占用此端口的是哪个程序/进程编号    |
| 19       | tar   -xf 压缩包文件名                                       | x: Extact，解压缩  f: File，指定文件名                       | 解压缩.tar.xz文件                                            |
| 20       | ln -s  原文件名  链接文件名                                  | Link：链接，快捷方式  Soft：软链接                           | 为指定的文件或目录创建另一个快捷方式                         |
| 21       | clear                                                        |                                                              | 清屏                                                         |
| 22       | kill  程序编号                                               |                                                              | 杀死指定编号对应的程序                                       |


```
①创建两个目录： /root/ibm、 /root/tesla
②进入/root/ibm，创建一个index.html，复制一份名为login.html
③把/root/ibm/login.html移动到/root/tesla目录下，并重名为signin.html          
④删除/root/ibm目录
```

> Linux小知识：文件的类型
>
> ① d： Directory，是一个目录
>
> ② -： 是一个普通文件，例如：图片、音视频、可执行文件....
>
> ③ l： Link，是一个链接文件，类似于Windows下面的“快捷方式”

 

### 三、在Linux系统中如何下载并安装软件：XAMPP套装

 **①设法找到Linux版本的XAMPP软件下载地址**

https://jaist.dl.sourceforge.net/project/xampp/XAMPP%20Linux/8.2.4/xampp-linux-x64-8.2.4-0-installer.run

 **②在Linux系统中下载指定地址处的XAMPP软件**

```
wget XAMPP的下载地址
```

 **③为XAMPP安装文件添加“执行”权限**

```
chmod  +x  XAMPP安装包文件名
```

 **④安装XAMPP可执行文件，稍等片刻即可安装完成**

```
./XAMPP安装包文件名
```

 **⑤找到XAMPP中服务器的启动脚本，运行其中的服务器**

```
/opt/lampp/xampp   restart
```

 **⑥删除XAMPP自带的欢迎网页，替换为自己的个人空间内容**

```
rm -rf  /opt/lampp/htdocs/* 
```




   ![img](https://raw.githubusercontent.com/zhanghaooss/clouding/master/img/clip_image007.png)

 

### 四、在Linux上搭建“前后端分离项目 —— 后端数据API子系统”

①编写一个SQL脚本文件，上传到Linux服务器

保存路径：/root/zhsq.sql

②Linux服务器上启动MySQL服务，执行上述脚本，在服务器上创建必需的数据库结构

重启服务器：   `/opt/lampp/xampp  restart`

导入SQL脚本：`/opt/lampp/bin/mysql  -uroot  <  /root/zhsq.sql`

③编写Node.js+Express服务器接口程序，上传到Linux服务器

创建项目目录： zhsq_api

下载必需的第三方模块：  npm  i  mysql express    

编写接口： GET  /user/list

④在Linux服务器上下载并安装Node.js解释器                      

下载Node.js安装程序：      

`wget Node.js下载地址`              

解压缩下载得到的.tar.xz文件： 

`tar  -xf  Node.js压缩包 `     

把Node.js可执行程序移动到/opt目录下：  

`mv  /root/node-v18.16.0-linux-x64  /opt`

为Node.js目录创建一个快捷方式：         

`ln -s  /opt/node-v18.16.0-linux-x64  /opt/node `

执行Node.js解释器，运行.js文件：         

```
/opt/node/bin/node  /root/zhsq_api/index.js           #程序运行在前台，会阻塞后续的命令行输入

/opt/node/bin/node  /root/zhsq_api/index.js  &        #程序运行在后台，命令行可以继续接收输入
```

提示：如果想杀死后台运行的程序，使用命令： `kill 进程编号`

提示：如果想查看之前置入后台的程序及其进程编号，使用命令： `netstat  -anp`

> Node.js官方下载地址：https://nodejs.org/dist/v18.16.0/node-v18.16.0-linux-x64.tar.xz 
>
>  Windows系统下的压缩包格式： .zip、 .rar、 .7z  Linux系统下的压缩包格式： .zip、 .tar、 .tar.gz、 .tar.xz 

### 五、使用Vue3创建一个新项目（前端子系统），部署到云服务器

①创建Vue3项目

`npm  init  vue@latest`

②进入Vue3项目，安装必需的依赖，启动开发服务器        

`cd  项目名`

`npm  install`

`npm  run  dev`

③修改Webpack或Vite打包工具的配置文件，指定打包完的资源请求地址使用相对路径而不是绝对路径

//修改webpack.config.js 或 vite.config.js文件

`base：‘./’,`          //Vite

`publicPath: ‘./’, `      //Webpack

④开发完毕，把项目打包，构建为纯静态HTML/CSS/JS文件

`npm run  build`

⑤把静态文件打包为.zip，上传到Linux服务器，保存到XAMPP的htdocs下

 

### 六、把上述服务器的启动操作编写为一个自动执行的脚本(Shell脚本），在Linux系统重启后自动执行

 ①创建一个脚本文件，其中包含需要启动的命令：

​     `vim /etc/init.d/my-servers.sh  `  

在其中添加如下内容：

 ```
 #!/bin/bash
 #启动MySQL和Apache服务
 /opt/lampp/xampp   restart
 #启动Node.js服务
 /opt/node/bin/node   /root/zhsq_api/index.js   &
 #更多的系统启动时要执行的命令...
 exit  0
 ```



​    ②为上述脚本文件添加执行权限：

​        ` chmod +x  /etc/init.d/my-servers.sh`

​     ③将上述文件添加到系统服务

​        ` update-rc.d  my-servers.sh  defaults   90`

​     ④把上述系统服务添加到当前运行级别下（即只要系统再次进入当前运行级别，则自动运行该脚本）

​      所有的系统默认启动的脚本都要保存在/etc/init.d目录下，但是需要在/etc/rcX.d目录下创建软连接（即快捷方式）

​        ` ln  -s  /etc/init.d/my-servers.sh  /etc/rc5.d/S90my-servers.sh`

​         (Link) (Soft)    原始文件名        快捷方式文件名

### 七、补充：Linux系统的七种运行级别/运行状态

​     **级别0：**停机状态

​     **级别1：**单用户模式，只允许root用户访问

​     **级别2：**类似于模式3，但是没有NFS功能

​     **级别3：**多用户模式，允许任何用户使用系统，只有命令行界面

​     **级别4：**留给用户自定义

​     **级别5：**多用户模式，允许任何用户使用系统，可有命令行界面和图形化界面

​     **级别6：**重启状态

```
查看当前系统的运行级别：   runlevel              
切换当前系统的运行级别：   init   另一个运行级别 
```



