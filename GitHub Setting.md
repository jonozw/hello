# GitHub 使用手册

## 前言
+ 本文主要讲解了如何在Windows下使用
+ 本文中, 凡是`RP-`开头的全大写字符串, 都需要读者根据自己的实际情况, `替换`成自己的实际内容
+  

## 目录
+ [软件安装](#软件安装)
  - [安装Git](#安装Git)
  - [安装SSH](#安装SSH)

## 太长不看版
### Git符号链接操作
```
  git init
  git lfs install
  git lfs track "*.exe"
  git config core.symlinks true
  git config user.name "username"
  git config user.email "user@email.com"  
  git config lfs.https://xxx.github.com/xxx/yyy.git/info/lfs.locksverify true
  git add *
  git commit -m "first commit"
  git branch -M main
  git remote add origin git@xxx.github.com:xxx/yyy.git
  git push -u origin main
  mklink /J xxx path-to-xxx
```

## 必要软件

## 安装Git
**(必选操作)**
1. 访问Git官网 https://git-scm.com/
2. 下载Windows版本 https://git-scm.com/download/win/
3. 安装

## 安装SSH
**(非必要操作)**
从Windows10 1803开始, Windows10已经内置OpenSSH Client了, 无需安装. 
但是如果我们需要在同一台机器上使用多个帐号的话, 就需要多个SSH密钥, 从而需要ssh-agent, 而这部分功能, 是在OpenSSH Server里面的. 网上可以轻易搜到用`设置`安装OpenSSH Server的流程, 但是有些Windows版本, 设置里面没有这个选项, 那就只能采用命令行了.
[Windows10安装OpenSSH](https://docs.microsoft.com/zh-cn/windows-server/administration/openssh/openssh_install_firstuse)
1. 以管理员身份打开`PowerShell`
2. 运行  
   `Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'`
   `Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0`
   `Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0`
   `Add-WindowsCapability -Online -Name OpenSSH.Server`
   `Add-WindowsCapability -Online -Name OpenSSH.Client`
3. 运行如下四个命令, 安装并启动服务
```
   Set-Service sshd -StartupType Automatic
   Set-Service ssh-agent -StartupType Automatic
   Start-Service sshd
   Start-Service ssh-agent
```
4. 一些额外服务相关命令
```
   Get-Service ssh-agent
   Get-NetFirewallRule -Name *ssh*
```
5. 最后键入命令将服务器数据库端口映射到本地端口   
   `ssh  -fNg -L <本地端口>:<服务器数据库地址>  <用户名>@<服务器地址>`

## 测试SSH全局
尝试在windows下的etc中加入ssh_config, 测试了下, 不行啊, 失败了

## 注册GitHub账号
1. 访问GitHub官网 https://github.com/
2. 注册账号

## SSH相关
### SSH必读部分
### 重要: 请一定备份好自己的密钥
### 生成SSH密钥
** 至少在当前的情况下, 机器里面有两份SSH, 缺省运行的是Windows的这一套 **
1. 打开Git Bash
2. 运行SSH命令`ssh-keygen`以生成密钥对  
   ssh-keygen的命令行选项很多, 具体参数请自行搜索学习, 缺省的命令如下  
   `ssh-keygen -t ed25519 -b 4096 -C "your_email@example.com"`  
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
   + 这里建议大家选择ED25519, 在密码学里, ED25519的评价远高于RSA采用的不透明算法NIST.
### 设置GitHub的SSH公钥
1. 登录GitHub
2. 打开 `https://github.com/settings/keys`
3. 选择右上角的`New SSH Key`
4. Title随便写, 就是个注释, 毕竟GitHub允许加入多个Key, 不加名字以后自己都不认识了
5. Key的部分, 打开公钥文件`xxx.pub`, 全部拷贝, 然后这里粘贴
6. 确认, 完毕

### Git设置代理
#### 使用命令行 
```
// HTTP Proxy
git config --global http.proxy http://127.0.0.1:10800
git config --global https.proxy https://127.0.0.1:10800
// Socks Proxy
git config --global http.proxy 'socks5://127.0.0.1:10800'
git config --global https.proxy 'socks5://127.0.0.1:10800'
// SSL Verify Setting
git config --global http.sslVerify false
// 取消代理设置
git config --global --unset http.proxy
git config --global --unset https.proxy
// 查看代理设置
git config --global --get http.proxy
git config --global --get https.proxy
```
#### 使用配置文件
```
.gitconfig
[http]
proxy = socks5://127.0.0.1:10800
[https]
proxy = socks5://127.0.0.1:10800
sslVerify = false
```


### SSH选读部分
### 如何在一台机器上共存多个GitHub帐号(多SSH密钥的处理)
1. 生成多个SSH密钥对
   + 注意, 生成多个SSH密钥, 就必须明确指明存贮密钥的文件名了.
     `ssh-keygen -t ed25519 -b 4096 -C "your-email-address" -f `%USERPROFILE%/.ssh/RP-KEYNAME`  
   + 假设我们用两个帐号, 一个工作用(RP-WORK), 一个私人用(RP-HOME), 则生成了两个密钥对, 四个文件
     - RP-WORK, RP-WORK.pub
     - RP-HOME, RP-HOME.pub
   + 将不同的公钥设置到不同的GitHub帐号中
   + 将密钥加入SSH Agent中
     - ssh sh-add 
2. 创建修改SSH配置文件
   + 创建`~/.ssh/config`
   + 修改`~/.ssh/config`文件, 为每个帐号, 生成相应的Host配置如下
    ```
    Host RP-ACCOUNT-NAME.github.com
    HostName  github.com
    User  git
    IdentityFile  ~/.ssh/RP-ACCOUNT-NAME.ssh-private-key
    ```
3. 

### SSH配置文件
  在linux下, SSH配置文件的位置在 `~/.ssh/ssh_config`  
  在windows下, SSH配置文件在 `%USERPROFILE%\.ssh\config`  

```
Host nick.github.com
  HostName  github.com
  User  git
  IdentityFile  ~/.ssh/private_ssh_key_file
# 设置使用HTTP代理
	ProxyCommand connect -H 127.0.0.1:8888 github.com %p
# 设置使用SOCKS代理
	ProxyCommand connect -S 127.0.0.1:8888 github.com %p
# 事实上, connect.exe在Git Windows版本中自带, 无需外购
# mklink connect.exe "C:\Program Files\Git\mingw64\bin\connect.exe"
```

```
Host github.com *.github.com
    HostName  %h
    User      git
    Port      22

    ProxyCommand connect -H 127.0.0.1:1087 %h %p #设置代理
    IdentityFile  ~/.ssh/your_private_key_file
    IdentitiesOnly yes
```

1. ForwardAgent yes
2. git config credential.helper store 
3. 







###. 本地仓库的配置
  + 如果以前配置了Git的全局用户和全局邮箱, 那么要取消一下  
    - 输入`git config -l`, 先看看全局配置信息, 熟悉下
    - 取消全局用户: `git config --global --unset user.name`  
    - 取消全局邮箱: `git config --global --unset user.email`
  + 取消了全局的邮箱和用户的代价, 就是每个仓库需要单独配置一下  
    进入相应的Git仓库后, 执行
    - 配置局部用户: `git config user.name "RP-USERNAME"`  
    - 配置局部邮箱: `git config user.email "RP-USER@DOMAIN.com"`  
  + 如果以前配置了远程仓库, 现在需要重新配置一下
    - 先看看远程分支: `git remote -v`  
    - 删除远程分支:   `git remote rm origin`  
    - 重建远程分支:   `git remote add origin git@RP-DOMAIN:RP-ACCOUNT-NAME/RP-REPO-NAME.git`  
      * RP-DOMAIN是在`~/.ssh/config`中列出的和SSH Key对应的域名, 如果使用单帐号, 那么这部分就不用修改, 就是缺省的github.com
4. 

error: src refspec master does not match any
error: failed to push some refs to 


## 关联GitHub远程仓库和初次提交
1. 首先要关联一个远程库
   + 首先打开Git Bash
   + 进入你要做版本管理的目录
   + Git init
2. 其次要保证此目录不是一个空目录, 不然后续命令执行会出错
   + 至少放一个Readme.md在这个目录下
   + 执行`git add *`
   + 执行`git commit -m "initial"`
3. 然后关联远程仓库  
   + 执行`git remote add origin git@server-name:path/repo-name.git`  
     对应到GitHub, 就是(大写的字母是要被替换的)  
   + 执行`git remote add origin git@github.com:RP-ACCOUNT-NAME/RP-REPO-NAME.git`  
   + 执行`git remote add origin https://RP-ACCOUNT-NAME@github.com/RP-ACCOUNT-NAME/RP-REPO-NAME.git`  
   + 执行`git push -u origin master`  
     来进行第一次推送行为, 推送master分支的所有内容  
     注意, 如果没有执行第二步, 目录为空, 则执行此命令会出错.  
4. 例行提交  
   此后，每次本地提交后，如果有必要提交到远程库，就可以使用Git命令  
   + `git push origin master`  
   来将本地已经提交的内容, 推送到远程库中.  

## 设置Git忽略文件
   git config --global core.excludesfile ~/.gitignore
   下面是.gitignore文件的样例
git@github.com:notwoms/backup.gitback



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
1. VSC如何打开, 关闭仓库
  + Visual Studio Code能自动扫描目录, 并打开一些Git版本管理的子目录, 如果需要Git的目录层级比较深的话, 能不能自动打开就不好说了, 不过这并不重要. 
  + 如果VSC不能主动找到仓库并打开, 那么可以`Ctrl+Shift+P`打开命令面板(`Command Palette`)后手工键入命令, `Git open repository...`, 然后选择Git仓库的目录, 主动打开即可.
  + 如果看着版本管理里面的Git仓库太多了, 烦了, 可以输入`Git close repository`, 然后按照提示选择要关闭的仓库即可.



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


### 内部SVN外部Git
目的
+ 内部采用SVN, 进行基本权限管理
+ 外部采用Git做自动备份
### 目录结构
项目名称为PNAME
ROOT-GIT-DIR
└─PNAME.git
  ├─.git(dir)
  ├─.gitignore
  └─PANME(dir)
      ├─.svn(dir)
      └─trunk(dir)
        ├─content.txt
        ├─contentA(dir)
        └─contentB(dir)
```BAT
  pushd %ROOT-GIT-DIR%
  mkdir %PNAME.git%
  pushd %PNAME.git%
  git init
  modify .gitignore to ignore .svn at least
  mkdir %PNAME%
  pushd %PANME%
  svn update ...
```



## 暂存箱

环境变量GIT_SSH_COMMAND：

从Git版本2.3.0可以使用环境变量GIT_SSH_COMMAND，如下所示：

GIT_SSH_COMMAND="ssh -i ~/.ssh/id_rsa_example" git clone example

请注意，-i有时可以被您的配置文件覆盖，在这种情况下，您应该给SSH一个空配置文件，如下所示：

GIT_SSH_COMMAND="ssh -i ~/.ssh/id_rsa_example -F /dev/null" git clone example

配置core.sshCommand：

从Git版本2.10.0，您可以配置每个repo或全局，所以您不必再设置环境变量！

     
    git config core.sshCommand "ssh -i ~/.ssh/id_rsa_example -F /dev/null"
     
    git pull
     
    git push
https://www.cnblogs.com/chenkeyu/p/10440798.html
git 指定要提交的ssh key


    3：把该key加到ssh agent上。由于不是使用默认的.ssh/id_rsa，所以你需要显示告诉ssh agent你的新key的位置

    $ ssh-add ~/.ssh/id_rsa_work
ssh-add ~/.ssh/ssh-jonozw@outlook
ssh-add ~/.ssh/ssh-tjww@hotmail


        1
        可以通过ssh-add -l来确认结果

    4：配置.ssh/config

    $ vi .ssh/config
     
    # 加上以下内容
    #default github
    Host github.com
      HostName github.com
      IdentityFile ~/.ssh/id_rsa
     
    Host github_work
      HostName github.com
      IdentityFile ~/.ssh/id_rsa_work

    5：这样的话，你就可以通过使用github.com别名github_work来明确说你要是使用id_rsa_work的SSH key来连接github，即使用工作账号进行操作。

    #本地建库
    $ git init
    $ git commit -am "first commit'
    #push到github上去
    $ git remote add origin git@github_work:xxxx/test.git
    $ git push origin master
--------------------- 
作者：一往无前-千夜 asd

??

来源：CSDN 
原文：https://blog.csdn.net/wolfking0608/article/details/78512171 
版权声明：本文为博主原创文章，转载请附上博文
TEST Windows SSH server  
TEST Windows SSH server again

sadf
双开VSC

UI界面commit
连不上github @ 201902251415?

## GitHub hosts

151.101.44.249 github.global.ssl.fastly.net
192.30.253.113 github.com
103.245.222.133 assets-cdn.github.com
23.235.47.133 assets-cdn.github.com
203.208.39.104 assets-cdn.github.com
204.232.175.78 documentcloud.github.com
204.232.175.94 gist.github.com
107.21.116.220 help.github.com
207.97.227.252 nodeload.github.com
199.27.76.130 raw.github.com
107.22.3.110 status.github.com
204.232.175.78 training.github.com
207.97.227.243 www.github.com
185.31.16.184 github.global.ssl.fastly.net
185.31.18.133 avatars0.githubusercontent.com
185.31.19.133 avatars1.githubusercontent.com

作者：__阳阳
链接：https://www.jianshu.com/p/4caa9c291bfc
来源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。


219.76.4.4 github-cloud.s3.amazonaws.com

------------------------------------
# Amazon AWS Start
54.239.31.69 aws.amazon.com
54.239.30.25 console.aws.amazon.com
54.239.96.90 ap-northeast-1.console.aws.amazon.com
54.240.226.81 ap-southeast-1.console.aws.amazon.com
54.240.193.125 ap-southeast-2.console.aws.amazon.com
54.239.54.102 eu-central-1.console.aws.amazon.com
177.72.244.194 sa-east-1.console.aws.amazon.com
176.32.114.59 eu-west-1.console.aws.amazon.com
54.239.31.128 us-west-1.console.aws.amazon.com
54.240.254.230 us-west-2.console.aws.amazon.com
54.239.38.102 s3-console-us-standard.console.aws.amazon.com
54.231.49.3 s3.amazonaws.com
52.219.0.4 s3-ap-northeast-1.amazonaws.com
54.231.242.170 s3-ap-southeast-1.amazonaws.com
54.231.251.21 s3-ap-southeast-2.amazonaws.com
54.231.193.37 s3-eu-central-1.amazonaws.com
52.218.16.140 s3-eu-west-1.amazonaws.com
52.92.72.2 s3-sa-east-1.amazonaws.com
54.231.236.6 s3-us-west-1.amazonaws.com
54.231.168.160 s3-us-west-2.amazonaws.com
52.216.80.48 github-cloud.s3.amazonaws.com
54.231.40.3 github-com.s3.amazonaws.com
52.216.20.171 github-production-release-asset-2e65be.s3.amazonaws.com
52.216.228.168 github-production-user-asset-6210df.s3.amazonaws.com 
# Amazon AWS End


52.216.164.171  github-production-release-asset-2e65be.s3.amazonaws.com  
52.216.99.59    github-production-release-asset-2e65be.s3.amazonaws.com  
54.231.112.144  github-production-release-asset-2e65be.s3.amazonaws.com  
54.231.88.43    github-production-release-asset-2e65be.s3.amazonaws.com  
52.216.8.107    github-production-release-asset-2e65be.s3.amazonaws.com  
--------------------- 
作者：欲穷三千界 
来源：CSDN 
原文：https://blog.csdn.net/duanluan/article/details/86503853 
版权声明：本文为博主原创文章，转载请附上博文链接！


# Amazon AWS Start
54.239.31.69 aws.amazon.com
54.239.30.25 console.aws.amazon.com
54.239.96.90 ap-northeast-1.console.aws.amazon.com
54.240.226.81 ap-southeast-1.console.aws.amazon.com
54.240.193.125 ap-southeast-2.console.aws.amazon.com
54.239.54.102 eu-central-1.console.aws.amazon.com
177.72.244.194 sa-east-1.console.aws.amazon.com
176.32.114.59 eu-west-1.console.aws.amazon.com
54.239.31.128 us-west-1.console.aws.amazon.com
54.240.254.230 us-west-2.console.aws.amazon.com
54.239.38.102 s3-console-us-standard.console.aws.amazon.com
54.231.49.3 s3.amazonaws.com
52.219.0.4 s3-ap-northeast-1.amazonaws.com
54.231.242.170 s3-ap-southeast-1.amazonaws.com
54.231.251.21 s3-ap-southeast-2.amazonaws.com
54.231.193.37 s3-eu-central-1.amazonaws.com
52.218.16.140 s3-eu-west-1.amazonaws.com
52.92.72.2 s3-sa-east-1.amazonaws.com
54.231.236.6 s3-us-west-1.amazonaws.com
54.231.168.160 s3-us-west-2.amazonaws.com
52.216.80.48 github-cloud.s3.amazonaws.com
54.231.40.3 github-com.s3.amazonaws.com

52.216.228.168 github-production-user-asset-6210df.s3.amazonaws.com 

#52.216.99.59    github-production-release-asset-2e65be.s3.amazonaws.com  
#54.231.112.144  github-production-release-asset-2e65be.s3.amazonaws.com  
#54.231.88.43    github-production-release-asset-2e65be.s3.amazonaws.com  
#52.216.8.107    github-production-release-asset-2e65be.s3.amazonaws.com  
#52.216.20.171   github-production-release-asset-2e65be.s3.amazonaws.com 
#52.216.16.16    github-production-release-asset-2e65be.s3.amazonaws.com 

52.216.131.187   github-production-release-asset-2e65be.s3.amazonaws.com 

# Amazon AWS End

# GitHub Start
151.101.44.249 github.global.ssl.fastly.net
192.30.253.113 github.com
103.245.222.133 assets-cdn.github.com
23.235.47.133 assets-cdn.github.com
203.208.39.104 assets-cdn.github.com
204.232.175.78 documentcloud.github.com
204.232.175.94 gist.github.com
107.21.116.220 help.github.com
207.97.227.252 nodeload.github.com
199.27.76.130 raw.github.com
107.22.3.110 status.github.com
204.232.175.78 training.github.com
207.97.227.243 www.github.com
185.31.16.184 github.global.ssl.fastly.net
185.31.18.133 avatars0.githubusercontent.com
185.31.19.133 avatars1.githubusercontent.com
# GitHub End


…or create a new repository on the command line
echo "# demo" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:notwoms/demo.git
git push -u origin master
…or push an existing repository from the command line
git remote add origin git@github.com:notwoms/demo.git
git push -u origin master
…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

…or create a new repository on the command line
echo "# demo" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/notwoms/demo.git
git push -u origin master
…or push an existing repository from the command line
git remote add origin https://github.com/notwoms/demo.git
git push -u origin master
…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

git 

### KnowHow
+ git clone 默认会下载项目的完整历史版本，如果你只关心最新版的代码，而不关心之前的历史信息，可以使用 git 的浅复制功能，如：
  `git clone --depth=1 https://github.com/xxx.git`


### Git SSH Proxy
[Git如何设置SSH代理](https://segmentfault.com/q/1010000000118837)
Git 目前支持的三种协议 git://、ssh:// 和 http://，其代理配置各不相同：
+ core.gitproxy 用于 git:// 协议;
+ http.proxy 用于 http:// 协议;
+ ssh:// 协议的代理需要配置 ssh 的 ProxyCommand 参数;
[内网配置Git SSH代理](https://blog.csdn.net/twilightdream/article/details/78260394)
[Git走Proxy代理总结](https://blog.csdn.net/LeoFitz/article/details/81326315)
[给Github设置代理](https://gist.github.com/chuyik/02d0d37a49edc162546441092efae6a1)
[GitHub 加速最佳实践](https://www.hi-linux.com/posts/11850.html)

### Git Credential helper
[Git配置credential helper](https://blog.csdn.net/u012163684/article/details/52433645/)
[git-credential-store](https://git-scm.com/docs/git-credential-store)
```
git config credential.helper 'store [<options>]'
git config credential.helper store --file=git-credential
git config --get credential.helper

```

### Git Alias
[Git Alias Howto](https://www.atlassian.com/blog/git/advanced-git-aliases)  


### Git LFS
[详解Git大文件存储Git LFS](https://www.cnblogs.com/cangqinglang/p/13097777.html)  
[Git LFS上传大文件](https://blog.csdn.net/u012390519/article/details/79441706)  
[使用GitLFS上传大文件到GitHub](https://blog.csdn.net/m0_46419510/article/details/112719633)  


1. 在每个需要LFS支持的仓库下, 运行 ` git lfs install` 来初始化Git对LFS的支持
2. 如果想要禁用LFS, 运行`git lfs uninstall`即可
3. 当你的仓库初始化了Git LFS后, 就可以通过使用 `git lfs track` 来指定要跟踪的文件了
4. 安装Git LFS后, 可以像往常一样使用`git clone`命令来克隆Git LFS仓库. 在克隆过程的结尾, Git 将检出master分支, 并且将自动下载完成检出过程所需的所有Git LFS文件
   `git clone git@bitbucket.org:ACCOUNTS/PROJECTS.git`
5. 如果需要克隆包含大量LFS文件的仓库, 则显式使用`git lfs clone`命令可提供更好的性能  
   `git lfs clone git@bitbucket.org:ACCOUNTS/PROJECTS.git`
6. 像克隆一样, 可以使用常规的`git pull`命令拉取Git LFS仓库. 拉取完成后, 所有需要的Git LFS文件都会作为自动检出过程的一部分而被下载. 
7. 不需要显式的命令即可获取Git LFS内容. 然而, 如果检出因为意外原因而失败, 你可以通过使用`git lfs pull`命令来下载当前提交的所有丢失的Git LFS内容. 
8. 如`git lfs clone`命令一样, `git lfs pull`命令会批量下载Git LFS文件. 如果用户知道自上次拉取以来已经更改了大量文件, 则可以`显式`调用`git lfs pull`命令来批量下载Git LFS内容, 而禁用在检出期间自动下载Git LFS. 这可以通过在调用`git pull`命令时使用`-c`选项覆盖Git配置来完成
   `git -c filter.lfs.smudge= -c filter.lfs.required=false pull && git lfs pull`
9. 由于输入的内容很多, 可以创建一个简单的`Git别名(Git Alias)`来执行批处理的Git和Git LFS命令
   `git config --global alias.plfs "\!git -c filter.lfs.smudge= -c filter.lfs.required=false pull && git lfs pull"`
   `git plfs`
10. 使用Git LFS跟踪文件, 当向仓库中添加新的大文件类型时, 需要使用`git lfs track`命令指定一个模式来告知Git LFS对其进行跟踪
   `git lfs track "*.largefiles"`
11. Git LFS支持的模式与.gitignore支持的模式相同
    ```
    # track all .ogg files in any directory
      git lfs track "*.ogg"
    # track files named music.ogg in any directory
      git lfs track "music.ogg"
    # track all files in the Assets directory and all subdirectories
      git lfs track "Assets/"
    # track all files in the Assets directory but *not* subdirectories
      git lfs track "Assets/*"
    # track all ogg files in Assets/Audio
      git lfs track "Assets/Audio/*.ogg"
    # track all ogg files in any directory named Music
      git lfs track "**/Music/*.ogg"
    # track png files containing "xxhdpi" in their name, in any directory
      git lfs track "*xxhdpi*.png"
    ```
12. 运行`git lfs track`后, 会在当前仓库中生成`.gitattributes`文件. `.gitattributes`是一种Git机制, 用于将`特殊行为` `绑定` 到`某些文件模式`. Git LFS会自动创建或更新.gitattributes文件, 以将跟踪的文件模式绑定到Git LFS过滤器. 但是, 用户`需要`将对.gitattributes文件的任何更改`主动`提交到仓库. 


### Git删除commit记录的方法
[Git删除commit记录的方法-删除push失败的记录](https://blog.csdn.net/quiet_girl/article/details/79487966)  



### GitBook
[ProGit](http://git.oschina.net/progit/)  
[git server搭建指南](https://www.cnblogs.com/zhangjianbin/p/6351560.html)  
[Windows Server2012搭建Git服务器](https://blog.csdn.net/u011781521/article/details/78337632)  
