# Windows系统安装步骤

## 下载Windows Image映像文件
+ 访问[itellyou](#http://itellyou.cn/)
+ 在左边栏找到要下载的正确的产品和版本.
+ 点击`详细信息`, 展开.
+ 记录好文件名以及SHA1值.
+ 拷贝`ed2k://|file|xxxxxxxx/`的连接地址, 下载, 如果不会下载`ed2k`的连接, 往下看. 
+ 打开百度盘, 选择`离线下载`, `新建链接任务`, 将`ed2k`的链接地址输入, 确定. 
+ 从百度盘再下载系统映像
+ 下载完毕, 找个算HASH值的软件, 算一下SHA1值, 看看和前面记录好的值是否一样, 不一样的话, 重新下载.

## 烧录Image到U盘
+ 以管理员身份, 打开UltraISO. 
+ 打开下载好的ISO文件, 然后依次选择菜单`Bootable`, `Write Disk Image`. 
+ 选择好U盘, 烧录之. 

## 安装系统
+ 设置BIOS从U盘启动, 或者按`F12`, 选择从U盘启动.
+ 安装程序运行, 运行到出错, 说找不到系统文件的时候, 别关机, 将U盘拔下来.
+ 将U盘插到刚才烧录ISO文件的机器上
+ 以管理员身份, 打开`cmd`或者`PowerShell`
+ 运行`convert @volume /FS:NTFS`, 其中`@volume`是U盘的盘符, 将U盘的文件系统, 转换成NTFS
+ 从ISO映像文件里面, 找到`source\install.wim`, 将其拷贝到U盘对应目录`source\install.win`, 覆盖原文件.
+ 将U盘插到要安装的机器上, 选择继续安装, 搞定.
之所以中间这么复杂, 是因为现在install.wim这个文件太大了, 超过4G了, 启动盘都是FAT32的, 放不下这个文件, 只能这样脱裤子放屁搞一下了.

## 激活Windows
+ 以管理员身份打开`cmd`或者`PowerShell`
+ `slmgr /skms kms.03k.org` (如果这个kms服务器失效了, 再找个其他的就行了)
+ `slmgr /ato`

## Office激活
首先你的office必须是vol版本，否则无法激活。
找到你的office安装目录，比如C:\Program Files (x86)\Microsoft Office\Office16
64位的就是C:\Program Files\Microsoft Office\Office16
office16是office2016，office15就是2013，office14就是2010.然后目录对的话，该目录下面应该有个OSPP.VBS。
接下来我们就cd到这个目录下面，例如（请更改为自己的实际安装目录）：
cd "C:\Program Files (x86)\Microsoft Office\Office16"
如果你不知道你的office装在哪个目录，可以打开一个程序比如word，然后用打开任务管理员右键选择“打开文件所在的位置”。
然后执行注册kms服务器地址：
cscript ospp.vbs /sethst:kms.03k.org
/sethst参数就是指定kms服务器地址。

一般ospp.vbs可以拖进去cmd窗口，所以也可以这么弄：
cscript "C:\Program Files (x86)\Microsoft Office\Office16\OSPP.VBS" /sethst:kms.03k.org
一般来说，“一句命令已经完成了”，但一般office不会马上连接kms服务器进行激活，所以我们额外补充一条手动激活命令：
cscript ospp.vbs /act
如果提示看到successful的字样，那么就是激活成功了，重新打开office就好。

## 设置`etc\hosts`的权限
在贵国, 没事搞搞hosts是免不了的, 懂的和完全不懂的都请跳过, 半懂不懂的, 可以看下
+ 
+ 

