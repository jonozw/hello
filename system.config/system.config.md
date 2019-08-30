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