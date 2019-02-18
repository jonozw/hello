# GitHub 使用手册

## 安装Git
1. 访问Git官网 https://git-scm.com/
2. 下载Windows版本 https://git-scm.com/download/win/
3. 安装

## 注册GitHub账号
1. 访问GitHub官网 https://github.com/
2. 注册账号

## 生成SSH Key
1. 打开Git Bash
2. 运行SSH命令`ssh-keygen`以生成密钥对  
   ssh-keygen的命令行选项很多, 具体参数请自行搜索学习, 缺省的命令如下  
   + `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`  
   这个命令会产生几次交互
   一次是问存贮的文件名  
   + `Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]`  
   建议直接回车用缺省文件名, 不然GitHub不认, 还得改名, 麻烦
   一次是要求输入key的密码, 这个也建议为空, 不然每次提交都要输入密码, 太麻烦  
   + `Enter passphrase (empty for no passphrase): [Press Enter]`
3. 生成后的SSH Key存放在目录 `%USERPROFILE%\.ssh`下
   `注意`: 生成Key的时候, 会问你用于存放Key的文件名, 但是Git本身只认缺省的文件名
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
5. Key的部分, 打开`id_rsa.pub`, 全部拷贝, 然后这里粘贴
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
2. 关联后，使用Git命令  
   `git push -u origin master`  
   来进行第一次推送行为, 推送master分支的所有内容  
3. 此后，每次本地提交后，如果有必要提交到远程库，就可以使用Git命令  
   `git push origin master`  
   来将本地已经提交的内容, 推送到远程库中  

1624
1625