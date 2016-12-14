# shadowsocks android 客户端流程简介（基于v3.3.0）

## ss-android进程概述
ss-android启动时共创建了2个java进程shadowsocks、shadowsocks:vpn以及4个native进程ss-local、pdnsd、ss-tunnel、tun2socks。

### ss-android进程启动流程以及进程通信关系图：
>shadowsocks => shadowsocks:vpn => ss-local => pdnsd => ss-tunnel => tun2socks

![AndroidProcess](../images/ProcessOfAndroidShadowsocks.jpg)

#### java进程
shadowsocks进程主要负责UI展示，以及参数设置；ShadowsocksVpnService工作在shadowsocks:vpn进程，shadowsocks:vpn是后台进程，除了启动native进程，流量统计等功能也是shadowsocks:vpn和shadowsocks协作完成的。shadowsocks通过bindService和shadowsocks:vpn建立Binder IPC。

#### native进程
###### ss-local
ss-local是本地socks代理服务进程，可以看作一个通道，通道一端连着墙外的shawdowsocks服务器，另一端接着手机的本地客户端进程。
> 本地客户端进程 <=>（本地服务端:1080）ss-local (本地socks客户端) <==| GFW |==> 远程socks服务端

###### tun2socks
vpn进程shadowsocks:vpn拿到tunfd后就拥有了读写虚拟网卡ip包的能力，但是采用socks5协议的ss-local工作在L4，没法直接处理L3的ip包，所以tun2socks成了让ss-android成为全局socks代理的关键。tun2socks在用户态实现了一个轻量级的TCP/IP栈（lwip），来实现L3的包转成L4的数据流。

> 本地进程 => tun设备（虚拟网卡）=> tun2socks => ss-local => 物理网卡

###### pdnsd与ss-tunnel
pdnsd是一个带永久缓存的dns代理服务器，而ss-tunnel主要用来实现本地端口转发；ss-tunnel会在本地起服务进程，然后同样转发dns查询的udp包，通过远程shadowsocks服务器转发给靠谱的dns服务器（例如8.8.8.8:53），而pdnsd主要用来提供一个无污染的dns，它不仅可以转发dns查询给上游dns服务器，修改TTL，提供缓存功能，尤为重要的是，pdnsd可以修改dns查询方式为tcp，从而避免dns污染。

## 结语
整个 shadowsocks-android 客户端的代码模块逻辑清晰，非常值得学习，但由于是从PC端移植过来的代理协议，android端的代码稍微有点臃肿，引用了比较多的第三方库，可以挑感兴趣的点去研读。 
