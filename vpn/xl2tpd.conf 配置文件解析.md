参考 [ubuntu 下 xl2tpd 配置文件的解释](https://manpages.ubuntu.com/manpages/focal/en/man5/xl2tpd.conf.5.html)，大抵都是一样的

xl2tpd.conf 包含了实现 l2tp 协议的 xl2tpd 的配置信息

配置文件由 section 和 parameter 组成， 每个 section 都有一个特定的名称，在使用配置 FIFO 时(正常是 /var/run/xl2tpd/l2tp-control) 将使用该名称， 详细可查看 xl2tpd.8

特定的给定名称 default 将指定参数应用于接下来的所有的 section。

## GLOBAL SECTION

* auth file

  指定在哪里寻找用于验证 l2tp 隧道的验证文件，默认是 /etc/xl2tpd/l2tp-secrets

* ipsec saref

  使用 IPsec 安全关联(Security Association )追踪, 当这个选项使能时，xl2tpd 接收到的包应该有额外的字段(refme 和 refhim)，这允许使用相同的内部 NATed IP 地址跟踪他多个 client， 并且也允许跟踪同一 NET 路由器后面的多个 client， 这需要内核支持，目前，仅适用在 Openswan KLIPS 的 mast 模式

  可以被设置为 yes 和 no, 默认是 no

* saref refinfo

  使用IPsec安全关联跟踪时，将使用新的 setsockopt

  默认 30 , 对于老的 SAref 修补内核，使用22

* listen-addr

  daemon 监听接口的 IP 地址，默认它监听 INADDR_ANY (0.0.0.0)， 意味着它监听所有的接口

* port

  指定 xl2tpd 应当使用的 UDP 端口，默认是 1701

* access control

  如果设置为 yes, xl2tpd 进程将只接受以下 section 中指定的对端地址的连接，默认是 no

* debug avp

  将此项设置为 yes 以启用 L2TP  AVP 调试信息的 syslog 输出

* debug network

  将此项设置为 yes 以启用 L2TP  network调试信息的 syslog 输出

* debug packet

  将其设置为 yes 以启用 L2TP 数据包调试信息的打印。注意：输出将转到STDOUT，因此只能与 -D 命令行选项一起使用

* debug state

  将此项设置为 yes 以启用 FSM 调试信息的 syslog 输出

* debug tunnel

  将此项设置为 yes 以启用 tunnel 调试信息的 syslog 输出

*  max retries

  指定 tunnel  关闭前的重试次数，如果没有了 tunnel , 则停止重传，默认是 5

## LNS SECTION

* exclusive

  如果设置为 yes，则在 2 个对端设备之间只允许建立一个控制隧道。

* (no) ip range

  指定 LNS 将分配给连接的 LAC PPP 隧道的 IP 地址范围。 可以定义多个范围。 使用“no” 声明不允许使用该特定范围。 使用 IP - IP 格式定义范围（示例：1.1.1.1 - 1.1.1.10）。 请注意，要么必须至少提供一个 ip 范围选项，要么必须将 assign ip 设置为 no。

* assign ip

  如果 xl2tpd 不应从使用 ip range 选项定义的池中分配 IP 地址，则将其设置为 no。 如果您有一些其他方法来分配 IP 地址，这会很有用，例如支持 RADIUS AAA 的 pppd。

* (no) lac

  指定允许连接到 xl2tpd  LNS 的 LAC 的 IP 地址。 格式与 ip range 选项相同。

* hidden bit

  如果设置为 yes，xl2tpd 将使用 L2TP 的 AVP 隐藏功能。 要获取有关隐藏 AVP 和一般 AVP 的更多信息，请参阅 rfc2661（添加 URL？）

* local ip

  使用下面的IP作为xl2tpd自己的ip地址。

* local ip range

  指定 LNS 将分配为连接 LAC PPP 隧道的本地地址的地址范围。 此选项与 local ip 选项互斥，在希望每个隧道具有唯一 IP 地址的情况下很有用。 指定与 ip range 选项完全相同的范围值。 请注意，分配 ip 选项对此选项没有影响（没太懂，可以看英语原文）

* length bit

  如果设置为 yes，将使用 l2tp 数据包有效载荷中存在的长度位。

* (refuse | require) chap

  将要求或拒绝远程对端设备通过 CHAP 进行身份验证以进行 ppp 身份验证。

* (refuse | require) pap

  将要求或拒绝远程对等点通过 PAP 进行身份验证以进行 ppp 身份验证。

* (refuse | require) authentication

  将要求或拒绝远程对端设备对自己进行身份验证。

* unix authentication

  如果设置为 yes,  /etc/passwd 将会被用于对端设备的 ppp authentication

* host name

  将在协商中将其报告为 xl2tpd 主机名。

* ppp debug

  这将为 pppd 启用调试。

* pass peer

  将对端的 IP 地址作为 ipparam 传递给 pppd。 默认启用。

* pppoptfile

  指定包含要使用的 pppd 配置参数的文件的路径。

* call rws

  此选项已弃用，不再起作用。 它曾经用于定义单个 L2TP 调用或会话的流量控制窗口大小。 L2TP 标准 (RFC2661) 不再定义呼叫或会话的流量控制或窗口大小。

* tunnel rws

  这定义了控制通道的窗口大小。 窗口大小定义为未确认的数据包的数量，而不是字节数。

* flow bits

  如果设置为 yes，序列号将包含在通信中。 在会话中使用序列号的功能目前已损坏且无法运行。

* challenge

  如果设置为 yes, 使用 challenge authentication 来认证对端设备

* rx bps

  如果设置，接收的最大带宽将是这个设置的值

* tx bps

  如果设置，发送的最大带宽将是这个设置的值

  

## LAC SECTION

以下是 LAC 特定的配置标识， LNS section 中大部分描述可以在 LAC 的上下文中使用，这在其中是常识(本质上是 l2tp 协议调整标志和身份验证/ppp 相关的)

* lns

  设定 要连接的 lns 的域名和 ip 地址

* autodial

  如果设置为 yes, xl2tpd 将在启动时自动拨号 LAC

* redial

  如果设置为 yes, 如果拨号断开，xl2tpd  将尝试重拨，要注意的是如果启用，xl2tpd  会将密码保存在内存中，这是一个潜在的安全风险

* redial timeout

  在重拨之前等待 X 秒，redial 选项必须为 yes 才可以使用这个选项，默认 30 秒

* max redials

  将在尝试 X 次后放弃



部分翻译出错，请看原英文解释： https://manpages.ubuntu.com/manpages/focal/en/man5/xl2tpd.conf.5.html

## linux 下作为 LNS 的一些文件配置

* /etc/xl2tpd/xl2tpd.conf 

```

[global]
ipsec saref = no
debug tunnel = no
debug avp = no
debug network = no
debug state = no
access control = no
rand source = dev
port = 1701
auth file = /etc/ppp/chap-secrets
 
[lns default]
ip range = 192.168.18.1 - 192.168.18.254
local ip = 192.168.18.2
name = 192.168.4.220
pass peer = yes
refuse pap = yes
refuse chap = yes
require authentication = yes
ppp debug = no
pppoptfile = /etc/ppp/options.xl2tpd
length bit = yes


```

* /etc/ppp/chap-secrets

```
# Secrets for authentication using CHAP
# client	server	secret			IP addresses
  l2tp            *          l2tp@123                   *
```

* /etc/ppp/options.xl2tpd

```
require-mschap-v2
refuse-mschap
ms-dns 127.0.0.53
asyncmap 0
auth
crtscts
idle 1800
mtu 1410
mru 1410
hide-password
local
modem
lock
name l2tpd
connect-delay 5000
lcp-echo-interval 30
lcp-echo-failure 4
```

## linux 下作为 LAC 的一些文件配置

* /etc/xl2tpd/xl2tpd.conf 

```
[global]
    port = 1701
    ipsec saref = yes
[lac l2tp]
    lns = 192.168.4.220
    name = 192.168.4.220
    ppp debug = yes
    pppoptfile = /tmp/options.xl2tpd
    redial = yes
    redial timeout = 30
    max redials = 3
```

* /etc/ppp/options.xl2tpd

```
#xl2tp pppd config
nobsdcomp
nodeflate
noaccomp
nopcomp
novj
nomppe
unit 2
nodefaultroute
remotename l2tp
user l2tp
password l2tp@123
lcp-echo-interval 60
lcp-echo-failure 5
logfile /var/log/ppp.log
```

使用 `/etc/init.d/xl2tpd start` 启动后，还需要使用以下命令去连接 LNS

```
echo "c l2tp" > /var/run/xl2tpd/l2tp-control
```

"c l2tp" 对应  /etc/xl2tpd/xl2tpd.conf  中的 [lac l2tp] 

