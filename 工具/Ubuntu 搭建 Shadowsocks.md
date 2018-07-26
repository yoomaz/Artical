感觉买的 shadowssocks 服务都很慢，而且安全性得不到保证，就准备自己搭建一台服务器，以前一直想买 digital ocea 上的一款，但是paypal一直不让我付款。。所以选择了搬瓦工，可以支付宝，真的很方便。

下面记录下步骤，很简单：
1. 执行下面命令安装 python 和 shadowssocks
```
apt-get update
apt-get install python-pip
pip install shadowsocks
```
pip 是 python 下的方便安装的工具

2. 配置
写一个配置文件保存为etc/shadowsocks.json，文件内容如下：
```
{ 
  "server":"0.0.0.0",  // 填自己服务器的 ip 地址
  "server_port":8388, 
  "local_port":1080, 
  "password":"mypassword", 
  "timeout":600, 
  "method":"aes-256-cfb", 
}
```

因为这个文件是没有的，需要我们自己创建：
```
cd /etc
touch shadowsocks.json
vim shadowsocks.json
// 这里就是vim的一些用法了，
按 i 进入插入模式
复制过去上面代码并修改成自己的
按 ESC 再 :wq 退出保存
```

3. 运行和关闭
```
# ssserver -c /etc/shadowsocks.json -d start
# ssserver -c /etc/shadowsocks.json -d stop
```