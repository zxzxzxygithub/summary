简单理解的话，Shadowsocks是将以前通过SSH创建的Socks5协议拆
开成Server端和client端，下面这个原理图能简单介绍其翻墙原理，
基本上和利用SSH tunnel大致类似：

![](../images/pcshadowsocks.png)

PC客户端（即你的电脑）发出请求基于Socks5协议跟SS-Local端进
行通讯，由于这个SS-Local一般是本机或路由器等局域网的其他机
器，不经过GFW，所以解决GFW通过特征分析进行干扰的问题。

SS-Local和SS-Server两端通过多种可选的加密方法进行通讯，经
过GFW的时候因为是常规的TCP包，没有明显特征码GFW也无法对通
讯数据进行解密，因此通讯放行。SS-Server将收到的加密数据进
行解密，还原初始请求，再发送到用户需要访问的服务网站，获取
响应原路再返回SS-04，返回途中依然使用了加密，使得流量是普
通TCP包，并成功穿过GFW防火墙。

因此，Shadowsocks的优点在于它解决了GFW通过分析流量特征从而
干扰的问题，这是它优于SSH和VPN翻墙的地方。缺点也依然明显，
需要一点点技术和资源（墙外VPS服务器）来搭建Shadowsocks服务，
好在已经有人搭建相应的服务出售翻墙服务了。

