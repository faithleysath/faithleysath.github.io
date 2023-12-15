---
title: NAS折腾记
date: 2023-12-14 23:28:08
tags:
- NAS
- 网络
- NAT
categories: 折腾
---

{% cq %} 本文记录了笔者折腾NAS的完整细节 {% endcq %}

机子配置如下：

- CPU ：i3 8100T
- 主板：MSI B360M PRO-VD
- 内存：ddr4 2400mhz 8G x 2
- 电源：海韵S12 II 550W

笔者决定NAS的理由之一是整合手头拥有的一些资源，使其发挥最大效用。

笔者整理了下掌握的资源，清单如下：

- 存储设备
  - 阿里云盘 22.88T
  - 百度网盘 8T
  - OneDrive 100T=5T*20
  - 2T机械*2
  - 256G固态*2
  - 1T固态*3（宿舍台式，Thinkbook，家里台式）
  - 250G固态U盘*2

- 宽带资源
  - 校园网：600M(对等)（理论无上限）（无ipv6）（ipv4为全锥型(Full Cone NAT)，即最好的一类NAT，几乎等于公网）
  - 家庭宽带：1000M(下行)/100M(上行)（未测试多拨，大概率不行）（有公网ipv6）（ipv4为端口受限锥型(Address and Port Restricted)，仅次于最差"对称型"的NAT）
  
- 硬件设备
  - ASUS ACRH-17 软路由
  - 自组NAS * 1
  - Redmi AX3000 路由器 * 2（此路由器无ipv6防火墙）
  - 华为某型号wifi6路由器一台
  - 宿舍、家中各一台台式
  - wifi插座 * 2
  - 联想个人云T1 Pro（包括一块3T机械）（家父使用中）
  - 阿里云服务器一台，100块一年

- 其他资源
  - 域名一枚
  - 百度网盘SVIP、阿里云盘20T超级会员
  - 网易云音乐黑胶SVIP
  - bilibili大会员
  - Microsoft Office E3 MSDN订阅（25个许可证）
  - Github学生认证包
  - 一些正版软件永久订阅

# 规划

首先要遵循以下原则：

- 文件"321备份"原则
- 高可用原则
- 即插即用原则

1. 文件"321备份"原则

即重要文件3份拷贝+2种介质+1个异地。

对此，笔者采用`Syncthing`+`Alist`+`Rclone`的方式。

## 第一步，挂载

利用`Alist`挂载`阿里云盘`、`百度网盘`、`坚果云`等云盘存储，并转换为Webdab协议，便于其他工具调用。

## 第二步，目录规划

由于笔者接触编程语言，习惯以项目(Project)为单位整理文件，且许多文件都是代码，故比较合适。但也要考虑部分归档文件，不适宜以项目的形式命名。故目录规划以两个角度展开，第一是目录的属性，第二是目录的大小。

- Projects目录

此目录起到一个工作区的作用，平时工作在此目录下进行。Projects下的一级目录表示不同的项目。此目录需要尽可能地保持轻量化，仅保存源代码，node_modules等环境需要设置排除规则。同时，本目录与github等版本控制服务并无冲突，因为git仅保存commits，对更小的粒度的文件同步并不起作用。

- VIP-File目录

此目录保存常规意义下的重要文件，包括个人档案，图片，密钥等等。

- RR-File目录（Rare Resource File，稀有资源文件）

此目录保存部分有必要321备份，无法容忍损失的稀有资源文件（指很难在互联网上直接下载，或独此一份）比如一些稀有许可证，crack。注意，由于321备份意味着占用所有存储相同的空间，故321备份的允许容量取决于最小的存储的容量。在这里，笔者使用的`坚果云`免费计划，每月上传流量为1G，下载流量为3G，所以得省着点用。这里考虑只存放VIP-File目录的文件。

## 第三步，设置同步

Nas与本地设备的同步采用`Syncthing`，这是一种基于P2P的同步软件。与其类似的还有`ResilioSync`，但后者是收费的，前者是开源的。`Syncthing`本身也支持版本管理。

Nas与云端存储的同步采用`Rclone`，前面已经将云端存储挂载为Webdav格式，设置定时脚本，将本地目录上传至`Rclone`。

2. 高可用原则

这里的高可用，指在需要的情况下高可用。由于校园网12点断网，次日6点恢复网络。笔者认为，在此期间一般无需使用服务，故不考虑此期间的可用性。

另外由于笔者经济有限，校园有线网/供电故障暂时无法解决。理论上，可以增加5G路由器提供网络可用性；增加UPS电源提供供电可用性

在更为宽松的要求下，笔者采用wifi插座+TailScale异地组网的方式来提供对供电/网络的控制。将主板bios设置为AC来电自启，即可远程开机。

3. 即插即用原则

即同时提供内网/公网两种访问方式。内网无需赘述，只需将一个固定的域名动态解析到内网ip上即可。公网支持略微复杂，如上所述，校园网采用Full Cone NAT。下面先介绍此NAT类型的特点。

> NAT即网络地址转换（Network Adress Transform）。内网设备可通过NAT访问公网。当内网设备A通过`ip1:port1`访问公网地址`ip2:port2`，ip报经过NAT设备时，NAT设备修改报文首部`ip1:port1`为`ip3:port3`，然后转发给`ip2:port2`。同时在其自身的数据库中记录映射关系`ip1:port1`<->`ip3:port3`。公网设备`ip2`接收到报文，查看报文头部后，认为此报文由`ip3:port3`发送。其回信则发往NAT设备`ip3:port3`。当数据包原路返回时，NAT设备根据映射表查询，将报文首部目的`ip3:port3`修改为`ip1:port1`，再转发给内网设备`ip1:port1`。

根据NAT行为的不同，可将NAT分类。具体分类方法有两种标准，`RFC 3489`与`RFC 5780`，其中前者的分类为熟知的`全锥型`、`地址受限锥型`、`端口受限锥型`、`对称型`；后者则在两个维度上进行分类，即从NAT设备的`映射行为`与`防火墙行为`上进行分类。具体来说，行为可分为`端点无关`与`端点有关`，其中`端点有关`又可以根据`源端点`、`目标端点`、`ip`、`端口`来判断是否有关。

对于`全锥型`，其行为就和最普通的NAT行为一致。即当内网设备需要访问公网设备时，由于其本身没有公网ip，报文无法传回（因为无法路由），故NAT设备将某个公网ip的某个特定端口映射到内网设备的特定端口上。对外网设备来说，访问这个映射后的公网ip:端口，就如同访问内网设备的特定端口一样，没有区别，可以双向传播。而映射是NAT设备自动建立的，具体来说，其第一次收到内网设备对外的通信请求时会建立映射，映射ip及端口是随机的。当然，映射也有时效性，若映射ip端口长期没有数据包通过，则此映射被“遗忘”，下次通信时将重新建立映射，且是随机的，不一定与上一次映射一致。当映射建立完成后，任意外网设备可主动向内网设备发起通信，而不受防火墙阻拦。

而对于其他类型的NAT，`映射行为`与`防火墙行为`中至少有一行为与端点有关。比如防火墙行为，有状态防火墙会要求，只有内网设备主动通信过的外网设备才可以回信/发起下一次通信。这意味着，即便存在一个外网与内网的映射，但除非内网设备先对某一主机通信，否则该主机不能先发起通信。再说映射行为，映射可以与目的地有关。比如内网设备的7890端口与外网的$n$个主机通信，则会建立$n$个映射，每个映射分别与特定主机通信，其他主机无法通过此映射联系内网设备。

很幸运的是，笔者学校的网络属于`全锥型NAT`。故笔者准备采用以下方法来达到公网ipv4的体验。

首先是web服务，使用`Natter`或`NATMap`基于`STUN`服务器，获取本机443/80端口的公网映射ip、端口号，并利用TCP长连接维持映射关系（如果不维持，遗忘时间大概在10s左右）

调用阿里云api，修改dns解析，使得A记录指向映射ip，并添加隐性URL转发，指向前述A记录的域名:映射端口。这时，公网访问这个隐性URL记录的域名，就如同访问内网设备的80端口。

接下来就是正常使用nginx反代了。

接着是qBittorrent的PT上传需要公网ipv6。qBittorrent里有设置监听端口，这个设置项有两个作用，第一个是它真的表示qb监听这个端口，第二个是上报tracker时上报的就是这个端口号，这个端口号进而会被tracker服务器通报给其他peer，其他peer就会拿着这个port来联系你了。不过ip倒不是自己上报的，而是tracker查看报文里的源ip，这个ip已经被NAT改成外网的映射ip了。那么如果我们在内网，我们的真实监听端口已经被映射成了另一个端口b了，其他peer拿着b来联系，自然联系不到。

解决方法就是，先选一个转发端口号`port1`，用这个转发端口去建立映射，然后用`Natter`或`NATMap`拿到映射`ip:port2`，再用脚本把qb的监听端口号改成映射端口号`port2`。再设置一个端口转发，把`port1`的流量都转发到`port2`这。这样上报的是映射端口，其他peer联系的也是映射端口，然后流量被NAT设备导到转发端口，再转发到qb的监听端口（已经设置成与映射端口一致）。
> 
> 具体可参考原作者博文：https://myth.cx/p/qbittorrent-nat-tcp-hole-punching/
> 
> 博文关键部分快照如下：
> 
> 我们以 NATMap 为例。建立 NAT 映射关系或映射关系改变时，NATMap 可以触发脚本执行自定义操作。传入参数为：
> 
> $1: Public address (IPv4/IPv6)
> $2: Public port
> $3: IP4P
> $4: Bind port (private port)
> $5: Protocol (TCP/UDP)
> 对于 BT 的应用场景，我们只需要关注参数 2 公网端口和参数 4 内网端口。我们的脚本需要将 qBittorrent 的监听端口设置为参数 2 公网端口，然后用 iptables 将本地的参数 4 内网端口转发至本地的参数 2 公网端口。具体实现如下：
>
> ```bash 
> #!/bin/sh
> 
> # Natter/NATMap
> private_port=$4 # Natter: $3; NATMap: $4
> public_port=$2 # Natter: $5; NATMap: $2
> 
> # qBittorrent.
> qb_web_port="8080"
> qb_username="admin"
> qb_password="adminadmin"
> 
> echo "Update qBittorrent listen port to $public_port..."
> 
> # Update qBittorrent listen port.
> qb_cookie=$(curl -s -i --header "Referer: http://localhost:$qb_web_port" --data "username=$qb_username&password=$qb_password" http://localhost:$qb_web_port/api/v2/auth/login | grep -i set-cookie | cut -c13-48)
> curl -X POST -b "$qb_cookie" -d 'json={"listen_port":"'$public_port'"}' "http://localhost:$qb_web_port/api/v2/app/setPreferences"
> 
> echo "Update iptables..."
> 
> # Use iptables to forward traffic.
> LINE_NUMBER=$(iptables -t nat -nvL --line-number | grep ${private_port} | head -n 1 | grep -o '^[0-9]+')
> iptables -t nat -D PREROUTING $LINE_NUMBER
> iptables -t nat -I PREROUTING -p tcp --dport $private_port -j REDIRECT --to-port $public_port
> 
> echo "Done."
> ```
> 详细使用说明和脚本下载请前往我的 Github 仓库 qBittorrent NAT TCP Hole Punching。欢迎 Star。
> 
> 如果运行 NATMap 的设备与运行 qBittorrent 的设备不同，则需要修改脚本中 iptables 转发的部分和 qBittorrent API 中的 localhost。