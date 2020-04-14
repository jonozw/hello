# System Config


## IPV6
https://github.com/XX-net/XX-Net/wiki/IPv6-Win10
```
  // 设置 Teredo 服务器，默认为：win10.ipv6.microsoft.com
  netsh interface teredo set state enterpriseclient server=default
  
  // 测试 IPv6 连接
  ping -6 ipv6.test-ipv6.com
  ping -6 [2001:470:1:18::125]

  // 重置 IPv6 配置
  netsh interface ipv6 reset

  通过命令ipconfig /all 查看当前网络信息，看到 Teredo Tunneling Pseudo-Interface 有以 2001 开头的IPv6地址即可。 启动IE浏览器，访问 http://test-ipv6.com 或 http://ipv6.test-ipv6.com，如果选项卡 “测试项目” 下面的 “不使用域名的 IPv6 测试” 显示成功，则隧道建立成功。Chrome浏览器的测试结果可能和IE不一样，请注意

  如果经过上面操作后仍无法启用 IPv6，可能是 Teredo 服务器无法正常连接，也可能是 Windows 自身配置问题，可尝试以下两种方法：

  // 第一种：修改 Teredo 服务器为 teredo.remlab.net
  netsh interface teredo set state server=teredo.remlab.net

  // 第二种：先卸载当前 Teredo 适配器再重新启用
  netsh interface Teredo set state disable
  netsh interface Teredo set state type=default
  ping -6 ipv6.test-ipv6.com
```

@20191128 更新
raiingwww
raiingthird1
raiingservice






### raiingthird1
Load Average: 0.00, 0.01, 0.05
Memory Used: 1,996,278K
外部动态IP: 13.75.123.172
内部静态IP: 10.0.0.5
内部子网: 10.0.0.0/16


### raiingwww
Load Average: 0.00, 0.01, 0.05
Used Memory: 3,880,788K
外部动态IP: 13.94.45.251
专用IP地址: 静态/10.0.0.17
子网IP地址: 10.0.0.0/10.0.0.16

### raiingcloudx
Load Average: 0.12, 0.11, 0.08
Memory Used: 2,640,000K


### raiingservice
Load Average: 0.00, 0.01, 0.05
Memory Used: 2,066,252K


- - - - - - -

### raiing3
外部动态IP: 139.219.111.183
专用IP地址: 静态/10.0.0.5
子网IP地址: 10.0.0.0/10.0.0.11

### raiing10
外部动态IP: 40.125.208.220
专用IP地址: 静态/10.0.0.6
子网IP地址: 10.0.0.0/10.0.0.11

### raiing2
#### Network
外部动态IP: 139.219.103.21
专用IP地址: 静态/10.0.0.7
子网IP地址: 10.0.0.0/10.0.0.11
#### Disk
raiing2@raiing2:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            823M     0  823M   0% /dev
tmpfs           167M   18M  150M  11% /run
/dev/sda1        30G  3.9G   26G  14% /
tmpfs           833M     0  833M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           833M     0  833M   0% /sys/fs/cgroup
/dev/sdc1      1007G  1.1G  955G   1% /www
/dev/sdb1        40G   48M   38G   1% /mnt
tmpfs           167M     0  167M   0% /run/user/1000



### raiingwww
#### Network
外部动态IP: 13.94.45.251
专用IP地址: 静态/10.0.0.17
子网IP地址: 10.0.0.0/10.0.0.16


### raiingcloudx
DS3_V2    4K  14G

DS11_V2
DS11_V2 Promo
D2V3  2K 8G
D4V3
A2M_V2 2K 16G



### raiingdbs1
D3标准: 4K 16G
D11标准: 2K 14G
A2M_V2: 2K 16G 


### 磁盘情况