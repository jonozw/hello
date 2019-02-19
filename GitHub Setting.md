# GitHub 使用手册

## 前言
+ 本文中, 凡是`RP-`开头的全大写字符串, 都需要读者根据自己的实际情况, `替换`成自己的实际内容
+  

## 安装Git
1. 访问Git官网 https://git-scm.com/
2. 下载Windows版本 https://git-scm.com/download/win/
3. 安装

## 注册GitHub账号
1. 访问GitHub官网 https://github.com/
2. 注册账号

## SSH相关
### SSH必读部分
### 生成SSH Key
1. 打开Git Bash
2. 运行SSH命令`ssh-keygen`以生成密钥对  
   ssh-keygen的命令行选项很多, 具体参数请自行搜索学习, 缺省的命令如下  
   `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`  
   这个命令会产生几次交互
   一次是问存贮的文件名  
   `Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]`  
   建议直接回车用缺省文件名, 不然还得单独去配置SSH, 如果不是很熟悉SSH的配置, 建议就还是别改名了  
   一次是要求输入key的密码, 这个也建议为空, 不然每次提交都要输入密码, 太麻烦  
   + `Enter passphrase (empty for no passphrase): [Press Enter]`
3. 生成后的SSH Key存放在目录 `%USERPROFILE%\.ssh`下
   `注意`: 生成Key的时候, 会问你用于存放Key的文件名, 但是Git在未配置SSH的时候, 只认缺省的文件名  
   缺省的文件名根据选择的加密方式而不同  
   + 选择RSA的时候, 其文件名是  
     - `%USERPROFILE%\.ssh\id_rsa` `(私钥)` 和  
     - `%USERPROFILE%\.ssh\id_rsa.pub` `(公钥)`  
   + 选择ED25519的时候, 其文件名是  
     - `%USERPROFILE%\.ssh\id_ed25519` `(私钥)` 和  
	 - `%USERPROFILE%\.ssh\id_ed25519.pub` `(公钥)`  
### 重要: 请一定备份好自己的Key

## 设置GitHub
1. 登录GitHub
2. 打开 `https://github.com/settings/keys`
3. 选择右上角的`New SSH Key`
4. Title随便写, 就是个注释, 毕竟GitHub允许加入多个Key, 不加名字以后自己都不认识了
5. Key的部分, 打开公钥文件`xxx.pub`, 全部拷贝, 然后这里粘贴
6. 确认, 完毕

## 关联和提交
1. 要关联一个远程库
   + 首先打开Git Bash
   + 进入你要做版本管理的目录
   + Git init
2. 然后再使用Git命令  
   `git remote add origin git@server-name:path/repo-name.git`  
   对应到GitHub, 就是(大写的字母是要被替换的)  
   `git remote add origin git@github.com:ACCOUNT-NAME/REPO-NAME.git`  
3. 关联后，首次使用Git Push命令  
   `git push -u origin master`  
   来进行第一次推送行为, 推送master分支的所有内容  
4. 此后，每次本地提交后，如果有必要提交到远程库，就可以使用Git命令  
   `git push origin master`  
   来将本地已经提交的内容, 推送到远程库中  

## 设置Git忽略文件
   git config --global core.excludesfile ~/.gitignore
   下面是.gitignore文件的样例

```
# Jupyter check points
.ipynb_checkpoints/

# Visual studio code
.vscode/

# Android Studio Navigation editor temp files
.navigation/

# Apple local file
*/.DS_Store
.DS_Store

# Image local file
Thumbs.db

# System file
*.bak
*.tem
*.temp
.swp
*.*~
~*.*

# SVN
.svn

```

## 使用Visual Studio Code来进行Git操作
### Visual Studio Code基本操作
1. 看起来似乎Visual Studio Code只认识第一层带Git版本管理的子目录, 再深就不认识了, 不能自动在左侧版本管理栏出现
2. 在文件修改后, 会增加一项`更改`项
3. 将鼠标移动到更改那一栏, 会出现一个加号`+` `更改暂存` 图标
4. 点击加号`+`图标, 将`更改` `暂存`(这个图标对应的是Git的`add`命令)
5. 将鼠标移动到`PROJECT GIT`的那一行, 会出现对勾`√` `提交` 图标, 点击对勾图标`√`, 进行`commit`
6. `commit`以后, 再点击上方的`PROJECT Git`(带分支图标和循环图标的那一栏), 进行Git push即可

### 一些Visual Studio Code的Git杂项
1. VSC的Git和命令行的Git是`相通`的, 一个操作, 在VSC的GUI上进行操作和在命令行进行操作, 是完全等同的.
   甚至可以一半操作在命令行, 一半操作在GUI也毫无问题.
   比方说在命令行里`+`隐式执行Git的add命令, 然后在命令行里`Git status`就能看到add完毕了.
   在命令行里commit后, 在GUI里看, 表示有未提交代码的角标就消失了.

## 命令行自动化
需要写一个简单的批处理, 来把GUI的操作执行了.


## Git Merge

## GitHub各种技巧
### GitHub和OneDrive共存的方法


### 如何在一台机器上共存多个GitHub帐号


### 如何从GitHub上只提取代码库的一个子目录
  Git的`设计理念`, 强调的是`分布式开发`, 因此要求本地需要有仓库的`完整镜像`.  
   
  Git去搞一个项目, 第一个操作就是`Git clone`, 在本地生成一个远程仓库的`完整镜像`.  
  但是很多时候, 我们其实只想下载部分子目录的东西, 把一个库全抓下来太累了, 有什么好办法能抓仓库的部分目录么?  
   
  Git 1.7.0之前是不支持这种操作的, 在Git 1.7.0后, Git加入了稀疏检出`Sparse Checkout`模式, 允许checkout指定文件或者文件夹. 但是即便是这种稀疏检出的方法, 也并没有降低网络消耗, 其实质依然是全部检出到本地, 只是不需要的部分被自动删除掉了.  
   
  还好, GitHub并不完全等同于Git`本身`, 为了让SVN的客户能方便地迁移到Git, GitHub也支持SVN, 允许用户用SVN协议来访问仓库.  

  远程仓库路径: https://github.com/RP-ACCOUNT/RP-REPO.git  
   
  ```
  远程仓库的文件目录结构:  
  RP-REPO/  
  RP-REPO/README.md  
  RP-REPO/subdir/  
  RP-REPO/subdir/subdir.md  
  ```
  现在我们尝试只抓取远程仓库中`subdir`的部分,  首先创建本地目录结构
  ```
  本地目录结构:  
  RP-REPO.git/  
  RP-REPO.git/trunk/  
  RP-REPO.git/trunk/subdir/  
  ```
  本地目录结构中的目录名`RP-REPO.git`是随便取的, 这个根目录名可以随意, 但是还是建议取得和`远程仓库`的名字一样.  
  创建子目录`trunk`, `trunk`可就`不是`可以随便更改的了, `必须`如此命名.  
  最后把`trunk`当作远程仓库的`根目录`, 按照远程仓库的`目录结构`, 手工创建同样的目录树吧.(不需要全部创建, 只需要创建你需要获取的子目录即可)  
  例子中, 创建`subdir`子目录完毕后, chdir进入`subdir`子目录.  
   ```
  远程源:  
  https://github.com/RP-ACCOUNT/RP-REPO.git/trunk/subdir
  本地目录:  
  ~\RP-REPO.git\trunk\subdir\  
   ```
  最后, 执行SVN的checkout吧:)




