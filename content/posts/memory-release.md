# 人脑内存释放

## 2026-02-23

最近上下文有点爆了，所以博客复活了。简单来说，我打算把我脑中的想法直接输出到这个页面，然后我就不再惦记着了，以后有需要再回来查。这个就有点像llm的渐进式披露prompt吧。问了Gemini，其实上下文爆了有个正统的心理学和认知科学概念，叫认知负荷理论，最早是由澳大利亚认知心理学家John Sweller在1988年提出的。

心理学研究表明，人类的工作记忆的容量极其有限。经典的米勒定律曾认为人类只能同时处理7+2个信息块，而近期的研究甚至认为，人脑在做复杂运算时，真正的多线程处理上限只有4个信息块左右。

所以当我同时面临“选哪种服务器架构”、“怎么配环境”、“要不要写文档”等一堆概念时，这几个可怜的并发线程就被瞬间占满了。

当外在负荷过大，导致总认知负荷超过了大脑的上限时，大脑的执行控制中枢（前额叶皮层）就会罢工。这时，不仅无法处理新信息，连决策能力也会直线下降。大脑在过载时还会释放压力激素（如皮质醇），这会产生烦躁、焦虑和类似强迫症的心理不适。

## 2026-02-25

今天来整家庭服务器，我有一台mac mini m4和pve台式机。

给你介绍下我的网络。
我有一台Mac mini m4 万兆口，还有一台pve。
其中Mac mini目前连了wifi、以太网、4g usb网卡。pve有两张网卡，一张主板自带的2.5G口，一张pcie拓展卡，万兆网卡。现在用超六类线把mac mini和pve连接了起来。mac mini上的网卡顺序是以太网>wifi>4g上网卡。以下是一些诊断信息。

```bash
laysath@laysaths-Mac-mini ~ % ifconfig
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
        options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
        inet 127.0.0.1 netmask 0xff000000
        inet6 ::1 prefixlen 128 
        inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1 
        nd6 options=201<PERFORMNUD,DAD>
gif0: flags=8010<POINTOPOINT,MULTICAST> mtu 1280
stf0: flags=0<> mtu 1280
anpi0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=400<CHANNEL_IO>
        ether 3a:0a:7f:ec:d5:7e
        media: none
        status: inactive
anpi3: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=400<CHANNEL_IO>
        ether 3a:0a:7f:ec:d5:81
        media: none
        status: inactive
anpi1: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=400<CHANNEL_IO>
        ether 3a:0a:7f:ec:d5:7f
        media: none
        status: inactive
en5: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=400<CHANNEL_IO>
        ether 3a:0a:7f:ec:d5:5e
        nd6 options=201<PERFORMNUD,DAD>
        media: none
        status: inactive
en6: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=400<CHANNEL_IO>
        ether 3a:0a:7f:ec:d5:5f
        nd6 options=201<PERFORMNUD,DAD>
        media: none
        status: inactive
en7: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=400<CHANNEL_IO>
        ether 3a:0a:7f:ec:d5:61
        nd6 options=201<PERFORMNUD,DAD>
        media: none
        status: inactive
en2: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
        options=460<TSO4,TSO6,CHANNEL_IO>
        ether 36:2f:77:00:b3:00
        media: autoselect <full-duplex>
        status: inactive
en3: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
        options=460<TSO4,TSO6,CHANNEL_IO>
        ether 36:2f:77:00:b3:04
        media: autoselect <full-duplex>
        status: inactive
en4: flags=8963<UP,BROADCAST,SMART,RUNNING,PROMISC,SIMPLEX,MULTICAST> mtu 1500
        options=460<TSO4,TSO6,CHANNEL_IO>
        ether 36:2f:77:00:b3:0c
        media: autoselect <full-duplex>
        status: inactive
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=567<RXCSUM,TXCSUM,VLAN_MTU,TSO4,TSO6,AV,CHANNEL_IO>
        ether d0:11:e5:96:31:35
        inet6 fe80::1885:6e74:1e1e:e717%en0 prefixlen 64 secured scopeid 0xf 
        inet 10.254.100.2 netmask 0xffff0000 broadcast 10.254.255.255
        nd6 options=201<PERFORMNUD,DAD>
        media: autoselect (10Gbase-T <full-duplex,flow-control>)
        status: active
bridge0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=63<RXCSUM,TXCSUM,TSO4,TSO6>
        ether 36:2f:77:00:b3:00
        Configuration:
                id 0:0:0:0:0:0 priority 0 hellotime 0 fwddelay 0
                maxage 0 holdcnt 0 proto stp maxaddr 100 timeout 1200
                root id 0:0:0:0:0:0 priority 0 ifcost 0 port 0
                ipfilter disabled flags 0x0
        member: en2 flags=3<LEARNING,DISCOVER>
                ifmaxaddr 0 port 10 priority 0 path cost 0
        member: en3 flags=3<LEARNING,DISCOVER>
                ifmaxaddr 0 port 11 priority 0 path cost 0
        member: en4 flags=3<LEARNING,DISCOVER>
                ifmaxaddr 0 port 12 priority 0 path cost 0
        nd6 options=201<PERFORMNUD,DAD>
        media: <unknown type>
        status: inactive
utun0: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1500
        inet6 fe80::cb5d:ad3:1a8c:217d%utun0 prefixlen 64 scopeid 0x11 
        nd6 options=201<PERFORMNUD,DAD>
ap1: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=6460<TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
        ether 66:c9:40:64:26:92
        nd6 options=201<PERFORMNUD,DAD>
        media: autoselect (none)
        status: inactive
en1: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=6460<TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
        ether ee:a6:92:91:82:76
        inet6 fe80::18ad:d66b:9236:657d%en1 prefixlen 64 secured scopeid 0xe 
        inet6 2409:8a21:428b:7f70:10f4:f584:a287:eca0 prefixlen 64 autoconf secured 
        inet6 2409:8a21:428b:7f70:cc2c:7268:3274:1006 prefixlen 64 autoconf temporary 
        inet6 2409:8a21:428b:7f70::ea3 prefixlen 64 dynamic 
        inet 192.168.100.2 netmask 0xffffff00 broadcast 192.168.100.255
        nd6 options=201<PERFORMNUD,DAD>
        media: autoselect
        status: active
awdl0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=6460<TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
        ether 32:26:fb:fd:14:a0
        inet6 fe80::3026:fbff:fefd:14a0%awdl0 prefixlen 64 scopeid 0x12 
        nd6 options=201<PERFORMNUD,DAD>
        media: autoselect
        status: active
llw0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=400<CHANNEL_IO>
        ether 32:26:fb:fd:14:a0
        inet6 fe80::3026:fbff:fefd:14a0%llw0 prefixlen 64 scopeid 0x13 
        nd6 options=201<PERFORMNUD,DAD>
        media: autoselect (none)
utun1: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1380
        inet6 fe80::b40f:f09e:edac:5f25%utun1 prefixlen 64 scopeid 0x14 
        nd6 options=201<PERFORMNUD,DAD>
utun2: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 2000
        inet6 fe80::fd69:b5dc:5c44:7c74%utun2 prefixlen 64 scopeid 0x15 
        nd6 options=201<PERFORMNUD,DAD>
utun3: flags=8051<UP,POINTOPOINT,RUNNING,MULTICAST> mtu 1000
        inet6 fe80::ce81:b1c:bd2c:69e%utun3 prefixlen 64 scopeid 0x16 
        nd6 options=201<PERFORMNUD,DAD>
en8: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        options=6464<VLAN_MTU,TSO4,TSO6,CHANNEL_IO,PARTIAL_CSUM,ZEROINVERT_CSUM>
        ether ac:00:33:aa:96:33
        inet6 fe80::1c64:cd1b:daaa:7d93%en8 prefixlen 64 secured scopeid 0x18 
        inet6 240e:878:b05:6a03:1c4d:a94f:75b9:8393 prefixlen 64 autoconf secured 
        inet6 240e:878:b05:6a03:ecd1:485:5179:d79c prefixlen 64 autoconf temporary 
        inet 192.168.2.100 netmask 0xffffff00 broadcast 192.168.2.255
        nd6 options=201<PERFORMNUD,DAD>
        media: autoselect (100baseTX <full-duplex>)
        status: active
laysath@laysaths-Mac-mini ~ % iperf3 -c 10.254.100.10
Connecting to host 10.254.100.10, port 5201
[  5] local 10.254.100.2 port 61807 connected to 10.254.100.10 port 5201
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.00   sec  1.10 GBytes  9.40 Gbits/sec                  
[  5]   1.00-2.00   sec  1.09 GBytes  9.41 Gbits/sec                  
[  5]   2.00-3.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   3.00-4.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   4.00-5.01   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   5.01-6.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   6.00-7.00   sec  1.09 GBytes  9.41 Gbits/sec                  
[  5]   7.00-8.01   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   8.01-9.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   9.00-10.00  sec  1.10 GBytes  9.41 Gbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-10.00  sec  11.0 GBytes  9.41 Gbits/sec                  sender
[  5]   0.00-10.01  sec  11.0 GBytes  9.41 Gbits/sec                  receiver

iperf Done.
laysath@laysaths-Mac-mini ~ % 
```

```bash
root@tower:~# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: nic0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel master vmbr0 state UP group default qlen 1000
    link/ether d8:43:ae:16:04:c1 brd ff:ff:ff:ff:ff:ff
    altname enxd843ae1604c1
3: nic1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master vmbr1 state UP group default qlen 1000
    link/ether 8c:a6:82:70:64:6a brd ff:ff:ff:ff:ff:ff
    altname enx8ca68270646a
4: wlo1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether e0:c2:64:1d:2d:f6 brd ff:ff:ff:ff:ff:ff
    altname wlp0s20f3
    altname wlxe0c2641d2df6
5: vmbr0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether d8:43:ae:16:04:c1 brd ff:ff:ff:ff:ff:ff
    inet 192.168.100.10/24 scope global vmbr0
       valid_lft forever preferred_lft forever
    inet6 fe80::da43:aeff:fe16:4c1/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
6: vmbr1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 8c:a6:82:70:64:6a brd ff:ff:ff:ff:ff:ff
    inet 10.254.100.10/16 scope global vmbr1
       valid_lft forever preferred_lft forever
    inet6 fe80::8ea6:82ff:fe70:646a/64 scope link proto kernel_ll 
       valid_lft forever preferred_lft forever
root@tower:~# cat /etc/network/interfaces
# network interface settings; autogenerated
# Please do NOT modify this file directly, unless you know what
# you're doing.
#
# If you want to manage parts of the network configuration manually,
# please utilize the 'source' or 'source-directory' directives to do
# so.
# PVE will preserve these directives, but will NOT read its network
# configuration from sourced files, so do not attempt to move any of
# the PVE managed interfaces into external files!

auto lo
iface lo inet loopback

iface nic0 inet manual

iface nic1 inet manual

iface wlo1 inet manual

auto vmbr0
iface vmbr0 inet static
        address 192.168.100.10/24
        gateway 192.168.100.1
        bridge-ports nic0
        bridge-stp off
        bridge-fd 0

auto vmbr1
iface vmbr1 inet static
        address 10.254.100.10/16
        bridge-ports nic1
        bridge-stp off
        bridge-fd 0

source /etc/network/interfaces.d/*
root@tower:~# iperf3 -s
-----------------------------------------------------------
Server listening on 5201 (test #1)
-----------------------------------------------------------
Accepted connection from 10.254.100.2, port 61806
[  5] local 10.254.100.10 port 5201 connected to 10.254.100.2 port 61807
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.00   sec  1.09 GBytes  9.39 Gbits/sec                  
[  5]   1.00-2.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   2.00-3.00   sec  1.10 GBytes  9.42 Gbits/sec                  
[  5]   3.00-4.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   4.00-5.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   5.00-6.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   6.00-7.00   sec  1.10 GBytes  9.42 Gbits/sec                  
[  5]   7.00-8.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   8.00-9.00   sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]   9.00-10.00  sec  1.10 GBytes  9.41 Gbits/sec                  
[  5]  10.00-10.01  sec  5.50 MBytes  9.39 Gbits/sec                  
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-10.01  sec  11.0 GBytes  9.41 Gbits/sec                  receiver
-----------------------------------------------------------
Server listening on 5201 (test #2)
-----------------------------------------------------------
^Ciperf3: interrupt - the server has terminated
root@tower:~# 
```

你好！这是一个非常棒且配置清晰的家庭实验室（HomeLab）网络架构。从你提供的诊断信息来看，你的**万兆直连已经完美跑通了**，iperf3 的测速结果非常漂亮！

我已经完全了解了你的网络拓扑。为了确认我们信息同步，我把你目前的网络状况梳理如下：

### 🖥️ Mac mini (M4) 网络状态

你的 Mac mini 配置了多重网络保障，并且按照 **以太网 > Wi-Fi > 4G** 的优先级运行：

- **万兆以太网 (`en0`)**：IP `10.254.100.2/16`。这是通过超六类线与 PVE 直连的万兆通道，目前状态完美（10Gbase-T, full-duplex）。
- **Wi-Fi (`en1`)**：IP `192.168.100.2/24`。连接着你的主路由网络。
- **4G USB网卡 (`en8`)**：IP `192.168.2.100/24`。作为备用外网链路。

### 🐧 PVE 服务器网络状态

你的 PVE 物理机通过两个桥接接口（Bridge）区分了管理/日常网络与高速直连网络：

- **日常网络 (`vmbr0` 绑 `nic0`)**：IP `192.168.100.10/24`，网关 `192.168.100.1`。这是主板自带的 2.5G 网卡，与你的 Mac mini 的 Wi-Fi 处于同一个局域网网段，负责 PVE 的默认上网和基础管理。
- **万兆直连 (`vmbr1` 绑 `nic1`)**：IP `10.254.100.10/16`。这是 PCIe 万兆扩展卡，专门用于和 Mac mini 进行点对点高速通信。

### 🚀 性能表现

你的 `iperf3` 测试结果跑出了 **9.41 Gbits/sec**。考虑到 TCP/IP 协议本身的开销（Overhead），这已经是万兆以太网在实际应用中能跑出的**极限满速**了。超六类线（Cat6a）在这里发挥了应有的实力。

---

这种“双网段分离”的设计非常经典：用 `192.168.100.x` 走日常互联网流量，用独立的 `10.254.x.x` 走万兆局域网流量，既保证了万兆带宽不被打扰，又不需要昂贵的万兆交换机。

既然我已经熟悉了你的网络底座，**请问你想让我帮你做些什么？** 比如：你想在 PVE 上配置基于此万兆链路的 NAS（如 TrueNAS/Samba）高性能文件共享，还是需要调整 Mac mini 上的静态路由表以优化流量走向？


你的网络架构非常清晰，万兆直连的跑满状态（iperf3 测得 9.41 Gbps）看着很舒服。Mac mini 和 PVE 的网络分层也做得很合理（192.168.100.x 作为主路由网段，10.254.x.x 作为万兆内网直连网段）。

既然你准备用 Debian 13 开一个 LXC 容器做轻量级的纯 IPv4 转发（软路由），并且不需要 DHCP/DNS，这个配置过程会非常清爽。以下是具体的实施步骤：

### 1. PVE 创建与配置 LXC 容器

在 PVE 中创建一个 Debian 13 的 LXC 容器（非特权容器通常即可支持 nftables，如果遇到权限问题可随时在“选项”中开启特权模式）。

为容器配置两张虚拟网卡：

- **eth0 (接入 vmbr0)**：作为这个软路由的“WAN”口（连接上级网络）。
- IPv4: 例如 `192.168.100.254/24` (确保不与现有设备冲突)
- 网关: `192.168.100.1` (指向上级路由)
- **eth1 (接入 vmbr1)**：作为这个软路由的“LAN”口（万兆网络）。
- IPv4: 例如 `10.254.100.1/16` (作为万兆网段的默认网关)
- 网关: 留空

### 2. 开启 Debian LXC 的 IPv4 转发

进入你刚刚创建的 Debian 13 LXC 容器控制台，首先开启内核的 IPv4 转发功能。

```
# 编辑 sysctl 配置文件
nano /etc/sysctl.conf
```

找到或添加以下这行，去掉前面的 `#` 注释：

```
net.ipv4.ip_forward=1
```

保存退出后，使配置生效：

```
sysctl -p
```

### 3. 安装并配置 Nftables

因为只需要做纯 IPv4 转发（通常意味着你需要把 `10.254.x.x` 网段的流量 NAT 伪装到 `192.168.100.x` 网段出去），我们可以写一个极简的 nftables 规则。

```
# 更新源并安装 nftables
apt update && apt install nftables -y
```

编辑 nftables 的默认配置文件：

```
nano /etc/nftables.conf
```

清空里面的默认内容，替换为以下精简版 NAT 规则（假设你的 `vmbr0` 对应容器内的网卡名为 `eth0`）：

```
#!/usr/sbin/nft -f

flush ruleset

table ip filter {
        chain input {
                type filter hook input priority 0; policy accept;
        }
        chain forward {
                type filter hook forward priority 0; policy accept;
        }
        chain output {
                type filter hook output priority 0; policy accept;
        }
}

table ip nat {
        chain prerouting {
                type nat hook prerouting priority -100; policy accept;
        }
        chain postrouting {
                type nat hook postrouting priority 100; policy accept;
                # 将从 eth0 (vmbr0) 出去的流量进行源地址伪装 (SNAT/Masquerade)
                oifname "eth0" masquerade
        }
}
```

保存退出后，启用并重启 nftables 服务：[^1]

```
systemctl enable nftables
systemctl restart nftables
```

### 4. 客户端路由配置 (Mac mini)[^2]

你的软路由已经就绪。现在，如果你希望 Mac mini 通过这条万兆链路访问 192.168.100.x 甚至外部网络，你需要调整 Mac mini 的路由表，或者将 Mac mini 上 `en0` (万兆网卡) 的默认网关设置为你刚给 LXC `eth1` 分配的 IP（例如 `10.254.100.1`）。[^3]

**注意**：你提[^4]到 Mac mini 上的网卡优先级是 `以太网 > wifi > 4g`，且目前 `en1` (192.168.100.2) 也是通的。如果你在 `en0` 上加了网关，macOS 会根据服务顺序（Service Order）来决定默认路由。如果你只想让特定网段走万兆，可以在 Mac mini 上手动写静态路由：

```
# 例如：强制让某个网段 (如 192.168.50.0/24) 走万兆软路由
sudo route -n add -net 192.168.50.0/24 10.254.100.1
```

---

你需要我为你提供在 Mac mini (macOS) 上优雅管理这些静态路由的具体命令，还是说你打算直接通过修改 macOS 的网卡优先级来让全局流量默认走这台 PVE 上的 LXC 软路由呢？
