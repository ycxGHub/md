###安装github
1. 下载[官网](https://git-for-windows.github.io/)
  [git window 64](https://github.com/git-for-windows/git/releases/download/v2.14.1.windows.1/Git-2.14.1-64-bit.exe)
  [git window 32](https://github.com/git-for-windows/git/releases/download/v2.14.1.windows.1/Git-2.14.1-32-bit.exe)
  [git mac 64](https://github.com/git-for-windows/git/releases/download/v2.14.1.windows.1/Git-2.14.1-64-bit.tar.bz2)
  [git mac 32](https://github.com/git-for-windows/git/releases/download/v2.14.1.windows.1/Git-2.14.1-32-bit.tar.bz2)
2. 安装后右键点击 Git Bash Here

![git.png](http://upload-images.jianshu.io/upload_images/8030893-164cce1cf00bf878.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入以下命令进行配置
```
$git config --global user.name "yourname"
$git config --global user.email "youremail@example.com"
$git config --global user.editor yourdeitor
```
###关于github ssh创建问题
1. 查看当前计算机是否已经有ssh
```
$ cd ~/.ssh
```
2. 查看ssh文件夹下文件
```
$ ls -la
```
3. 如果有 id_rsa , id_rsa.pub,known_hosts 则删除
```
$ rm  id_rsa
$ rm id_rsa.pub
$ rm known_hosts
```
4. 生成自己的ssh-key
```
$ ssh-keygen -t rsa -C "****@163.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/ycx/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/ycx/.ssh/id_rsa.
Your public key has been saved in /c/Users/ycx/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:vOdSmPCEWy5scBMIFRQHUEQXFDQ//B/iXxIW3Cr5fqM ycxadr@163.com
The key's randomart image is:
+---[RSA 2048]----+
|  oOXBOo         |
|    .o.+   . .   |
|       o+   o .  |
![ssh.png](http://upload-images.jianshu.io/upload_images/8030893-5d01460e0d299bb6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

|    . =.oo . o   |
|     + OSo= =    |
|      = =o.* o   |
|     . ...o + .  |
|        .o o oo  |
|         .. Eo . |
+----[SHA256]-----+
```
5. 添加到[github](https://github.com/),点击setting进入设置界面
  ![setting.png](http://upload-images.jianshu.io/upload_images/8030893-f1730d875210737d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  然后进入ssh设置
  ![ssh.png](http://upload-images.jianshu.io/upload_images/8030893-4202b6877939c0e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  新建点击new ssh key

![sshkey.png](http://upload-images.jianshu.io/upload_images/8030893-d54b22b2de7da9a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
将id_rsa.pub中内容复制进去
![sshcontext.png](http://upload-images.jianshu.io/upload_images/8030893-f8ffa9733386b8b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
###创建自己的repository
+  在Github创建自己的仓库，点击 New repository

![图片1.png](http://upload-images.jianshu.io/upload_images/8030893-d5915d3511f83c0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 创建成功后，在自己本地目录打开 git bash下输入以下命令
>创建git 仓库
>`git init `
>创建自己的忽略文件
>`touch .gitignore/`
>创建README.md
>`echo "ssd" >>README.md`
>添加所有文件到索引
>`git add -A`
>提交changes
>`git commit -m "first"`
>添加服务器 git@.....是你的ssh路径
>`git remote add origin git@github.com********`
>提交到服务器，“master”是你的branchname
>`git push -u origin master`

android 工程忽略文件如下：
```
/build
/app/.externalNativeBuild
/.gradle
/gradle
/.idea
/*.iml
```