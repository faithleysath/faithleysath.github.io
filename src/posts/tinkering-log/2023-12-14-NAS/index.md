# NAS 折腾记       

> 本文记录了笔者折腾 NAS 的完整细节

机子配置如下：

- CPU ：i3 8100T
-  主板：MSI B360M PRO-VD
-  内存：ddr4 2400mhz 8G x 2
-  电源：海韵 S12 II 550W

笔者决定 NAS 的理由之一是整合手头拥有的一些资源，使其发挥最大效用。

笔者整理了下掌握的资源，清单如下：

### 存储设备
  - 阿里云盘 22.88T
  - 百度网盘 8T
  - OneDrive 100T=5T*20
  - 2T 机械 * 2
  - 256G 固态 * 2
  - 1T 固态 * 3（宿舍台式，Thinkbook，家里台式）
  - 250G 固态 U 盘 * 2
### 宽带资源
  - 校园网：600M (对等)（理论无上限）（无 ipv6）（ipv4 为全锥型 (Full Cone NAT)，即最好的一类 NAT，几乎等于公网）
  - 家庭宽带：1000M (下行)/100M (上行)（未测试多拨，大概率不行）（有公网 ipv6）（ipv4 为端口受限锥型 (Address and Port Restricted)，仅次于最差 "对称型" 的 NAT）
### 硬件设备
  - ASUS ACRH-17 软路由
  - 自组 NAS * 1
  - Redmi AX3000 路由器 * 2（此路由器无 ipv6 防火墙）
  - 华为某型号 wifi6 路由器一台
  - 宿舍、家中各一台台式
  -  wifi 插座 * 2
  -  联想个人云 T1 Pro（包括一块 3T 机械）（家父使用中）
  - 阿里云服务器一台，100 块一年
### 其他资源
  - 域名一枚
  - 百度网盘 SVIP、阿里云盘 20T 超级会员
  - 网易云音乐黑胶 SVIP
  - bilibili 大会员
  -  Microsoft Office E3 MSDN 订阅（25 个许可证）
  - Github 学生认证包
  - 一些正版软件永久订阅

##  规划

首先要遵循以下原则：

- 文件 "321 备份" 原则
- 高可用原则
- 即插即用原则

###  1. 文件 "321 备份" 原则

即重要文件 3 份拷贝 + 2 种介质 + 1 个异地。

对此，笔者采用 `Syncthing`+`Alist`+`Rclone` 的方式。

####  第一步，挂载

利用 `Alist` 挂载`阿里云盘`、`百度网盘`、`坚果云`等云盘存储，并转换为 Webdab 协议，便于其他工具调用。

####  第二步，目录规划

由于笔者接触编程语言，习惯以项目 (Project) 为单位整理文件，且许多文件都是代码，故比较合适。但也要考虑部分归档文件，不适宜以项目的形式命名。故目录规划以两个角度展开，第一是目录的属性，第二是目录的大小。

- Projects 目录

此目录起到一个工作区的作用，平时工作在此目录下进行。Projects 下的一级目录表示不同的项目。此目录需要尽可能地保持轻量化，仅保存源代码，node_modules 等环境需要设置排除规则。同时，本目录与 github 等版本控制服务并无冲突，因为 git 仅保存 commits，对更小的粒度的文件同步并不起作用。

- VIP-File 目录

此目录保存常规意义下的重要文件，包括个人档案，图片，密钥等等。

- RR-File 目录（Rare Resource File，稀有资源文件）

此目录保存部分有必要 321 备份，无法容忍损失的稀有资源文件（指很难在互联网上直接下载，或独此一份）比如一些稀有许可证，crack。注意，由于 321 备份意味着占用所有存储相同的空间，故 321 备份的允许容量取决于最小的存储的容量。在这里，笔者使用的`坚果云`免费计划，每月上传流量为 1G，下载流量为 3G，所以得省着点用。这里考虑只存放 VIP-File 目录的文件。

####  第三步，设置同步

Nas 与本地设备的同步采用 `Syncthing`，这是一种基于 P2P 的同步软件。与其类似的还有 `ResilioSync`，但后者是收费的，前者是开源的。`Syncthing` 本身也支持版本管理。

Nas 与云端存储的同步采用 `Rclone`，前面已经将云端存储挂载为 Webdav 格式，设置定时脚本，将本地目录上传至 `Rclone`。

###  2. 高可用原则

这里的高可用，指在需要的情况下高可用。由于校园网 12 点断网，次日 6 点恢复网络。笔者认为，在此期间一般无需使用服务，故不考虑此期间的可用性。

另外由于笔者经济有限，校园有线网 / 供电故障暂时无法解决。理论上，可以增加 5G 路由器提供网络可用性；增加 UPS 电源提供供电可用性

在更为宽松的要求下，笔者采用 wifi 插座 + TailScale 异地组网的方式来提供对供电 / 网络的控制。将主板 bios 设置为 AC 来电自启，即可远程开机。

###  3. 即插即用原则

即同时提供内网 / 公网两种访问方式。内网无需赘述，只需将一个固定的域名动态解析到内网 ip 上即可。公网支持略微复杂，如上所述，校园网采用 Full Cone NAT。下面先介绍此 NAT 类型的特点。

> NAT 即网络地址转换（Network Adress Transform）。内网设备可通过 NAT 访问公网。当内网设备 A 通过 `ip1:port1` 访问公网地址 `ip2:port2`，ip 报经过 NAT 设备时，NAT 设备修改报文首部 `ip1:port1` 为 `ip3:port3`，然后转发给 `ip2:port2`。同时在其自身的数据库中记录映射关系 `ip1:port1`<->`ip3:port3`。公网设备 `ip2` 接收到报文，查看报文头部后，认为此报文由 `ip3:port3` 发送。其回信则发往 NAT 设备 `ip3:port3`。当数据包原路返回时，NAT 设备根据映射表查询，将报文首部目的 `ip3:port3` 修改为 `ip1:port1`，再转发给内网设备 `ip1:port1`。

根据 NAT 行为的不同，可将 NAT 分类。具体分类方法有两种标准，`RFC 3489` 与 `RFC 5780`，其中前者的分类为熟知的`全锥型`、`地址受限锥型`、`端口受限锥型`、`对称型`；后者则在两个维度上进行分类，即从 NAT 设备的`映射行为`与`防火墙行为`上进行分类。具体来说，行为可分为`端点无关`与`端点有关`，其中`端点有关`又可以根据`源端点`、`目标端点`、`ip`、`端口`来判断是否有关。

对于`全锥型`，其行为就和最普通的 NAT 行为一致。即当内网设备需要访问公网设备时，由于其本身没有公网 ip，报文无法传回（因为无法路由），故 NAT 设备将某个公网 ip 的某个特定端口映射到内网设备的特定端口上。对外网设备来说，访问这个映射后的公网 ip: 端口，就如同访问内网设备的特定端口一样，没有区别，可以双向传播。而映射是 NAT 设备自动建立的，具体来说，其第一次收到内网设备对外的通信请求时会建立映射，映射 ip 及端口是随机的。当然，映射也有时效性，若映射 ip 端口长期没有数据包通过，则此映射被 “遗忘”，下次通信时将重新建立映射，且是随机的，不一定与上一次映射一致。当映射建立完成后，任意外网设备可主动向内网设备发起通信，而不受防火墙阻拦。

而对于其他类型的 NAT，`映射行为`与`防火墙行为`中至少有一行为与端点有关。比如防火墙行为，有状态防火墙会要求，只有内网设备主动通信过的外网设备才可以回信 / 发起下一次通信。这意味着，即便存在一个外网与内网的映射，但除非内网设备先对某一主机通信，否则该主机不能先发起通信。再说映射行为，映射可以与目的地有关。比如内网设备的 7890 端口与外网的 nnn 个主机通信，则会建立 nnn 个映射，每个映射分别与特定主机通信，其他主机无法通过此映射联系内网设备。

很幸运的是，笔者学校的网络属于`全锥型NAT`。故笔者准备采用以下方法来达到公网 ipv4 的体验。

首先是 web 服务，使用 `Natter` 或 `NATMap` 基于 `STUN` 服务器，获取本机 443/80 端口的公网映射 ip、端口号，并利用 TCP 长连接维持映射关系（如果不维持，遗忘时间大概在 10s 左右）

调用阿里云 api，修改 dns 解析，使得 A 记录指向映射 ip，并添加隐性 URL 转发，指向前述 A 记录的域名：映射端口。这时，公网访问这个隐性 URL 记录的域名，就如同访问内网设备的 80 端口。

接下来就是正常使用 nginx 反代了。

接着是 qBittorrent 的 PT 上传需要公网 ipv6。qBittorrent 里有设置监听端口，这个设置项有两个作用，第一个是它真的表示 qb 监听这个端口，第二个是上报 tracker 时上报的就是这个端口号，这个端口号进而会被 tracker 服务器通报给其他 peer，其他 peer 就会拿着这个 port 来联系你了。不过 ip 倒不是自己上报的，而是 tracker 查看报文里的源 ip，这个 ip 已经被 NAT 改成外网的映射 ip 了。那么如果我们在内网，我们的真实监听端口已经被映射成了另一个端口 b 了，其他 peer 拿着 b 来联系，自然联系不到。

解决方法就是，先选一个转发端口号 `port1`，用这个转发端口去建立映射，然后用 `Natter` 或 `NATMap` 拿到映射 `ip:port2`，再用脚本把 qb 的监听端口号改成映射端口号 `port2`。再设置一个端口转发，把 `port1` 的流量都转发到 `port2` 这。这样上报的是映射端口，其他 peer 联系的也是映射端口，然后流量被 NAT 设备导到转发端口，再转发到 qb 的监听端口（已经设置成与映射端口一致）。

> 具体可参考原作者博文：[https://myth.cx/p/qbittorrent-nat-tcp-hole-punching/](https://myth.cx/p/qbittorrent-nat-tcp-hole-punching/)
