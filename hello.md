# GitHub 使用手册

## 安装Git
1. 访问Git官网 https://git-scm.com/
2. 下载Windows版本 https://git-scm.com/download/win
3. 安装

## 注册GitHub账号
1. 访问GitHub官网 https://github.com/
2. 注册账号

## 生成SSH Key
1. 打开Git Bash
2. 
3. 生成后的SSH Key存放在目录 `%USERPROFILE%\.ssh`下
   `注意`: 生成Key的时候, 会问你用于存放Key的文件名, 但是Git本身只认缺省的文件名
   缺省的文件名根据选择的加密方式而不同
   + 选择RSA的时候, 其文件名是
     - `%USERPROFILE%\.ssh\id_rsa` `(私钥)` 和
     - `%USERPROFILE%\.ssh\id_rsa.pub` `(公钥)`
   + 选择ED25519的时候, 其文件名是
     - `%USERPROFILE%\.ssh\id_ed25519` `(私钥)` 和
	 - `%USERPROFILE%\.ssh\id_ed25519.pub` `(公钥)`
4. 

## 关联和提交
1. 要关联一个远程库，使用Git命令  
   `git remote add origin git@server-name:path/repo-name.git`  
   对应到GitHub, 就是(大写的字母是要被替换的)  
   `git remote add origin git@bithub.com:ACCOUNT/REPO-NAME.git`  

2. 关联后，使用Git命令  
   `git push -u origin master`  
   来进行第一次推送行为, 推送master分支的所有内容  
3. 此后，每次本地提交后，如果有必要提交到远程库，就可以使用Git命令  
   `git push origin master`  
   来将本地已经提交的内容, 推送到远程库中  

