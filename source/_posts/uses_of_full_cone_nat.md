---
title: 全锥型NAT的实用技巧
date: 2024-01-11 14:02:31
tags:
- NAT
- 网络
- 反代
- NAS
categories: 网络
---
# 前言

如今，随着ipv6的普及，越来越多的家庭宽带有了v6公网，可仍有部分使用场景不具备v6条件。本文介绍了一种基于Full Cone NAT的内网穿透解决方案，实现近乎原生的公网ipv6体验。

# 准备工作

- 运营商NAT级别为NAT1（Full Cone NAT，全锥型）
- 光猫打开桥接（路由器拨号）

首先得确认家庭网络是否为全锥型NAT。测试步骤如下：

1. 确保光猫开启桥接并使用路由器拨号（如果你不理解这段话的意思，自行上tb搜索“光猫桥接”）

2. 路由器中开启DMZ主机并指向你需要穿透的电脑的局域网ip

3. 下载[NatTypeTester](https://github.com/HMBSbige/NatTypeTester/releases)进行测试，若结果都显示Independent（即没有一个显示Dependent），那么恭喜，你将享受v4公网的体验。

# STUN

STUN，全称为Session Traversal Utilities for NAT，是一种网络协议，它允许位于NAT（网络地址转换）后的客户端找出自己的公网地址，查找当前NAT设备对其进行的NAT类型，并在需要时保持NAT设备上的端口开放。

STUN的主要作用是解决NAT穿透问题。NAT穿透是指在NAT环境中，从公网访问NAT后的私网设备的问题。由于NAT设备的存在，私网设备对外的通信地址并非其真实的IP地址，这就导致了公网设备无法直接访问私网设备。STUN通过发现设备的公网地址和NAT类型，帮助设备建立和公网的通信。

目前有3种用于STUN的工具，分别为

- [Lucky](https://lucky666.cn/)：拥有友好的Web管理页面，强烈推荐
- [Natter](https://github.com/MikeWang000000/Natter)：基于Python实现的命令行工具
- [NatMap](https://github.com/heiher/natmap)：基于C++实现的命令行工具

本文就Lucky的部署进行讲解，实际上，[官方文档](https://lucky666.cn/docs/shareteach/)也给出了很多网友提供的`经验分享`，可补充阅读！

## 安装Lucky

本文在`Windows`环境下部署Lucky，其余环境可自行参考[官方文档](https://lucky666.cn/docs/install)

1. 下载[最新安装包](https://www.daji.it:6/release/)，其中分为适中版和大吉版，后者在前者的基础上增加了`File Browser`的功能，本文仅选择普通版即可。

2. 将安装包解压到一个固定的位置作为程序的运行环境，双击运行。

3. 检查右下角托盘区出现lucky的logo，右键“打开Lucky后台”，也可以顺便设置开机自启。

4. 具体使用方法请参见[官方文档](https://lucky666.cn/docs/category/%E5%9F%BA%E7%A1%80%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)，如需部署`MC服务器`、`WEB服务`等，也可直接阅读本文剩余内容。

# 案例介绍

## MC服务器

本文以MCSM面板为例，其余mc服务器开设方式同理。

1. 启动MC服务器，若您尚未开服，可使用[MCSM面板](https://mcsmanager.com/)一键开服。![懒人开服](/images/uses_of_full_cone_nat/mcsm_select_server.png)

2. 确认服务器监听的端口，大多数情况下，`mc服务器`的默认端口号为`25565`，即在客户端中输入ip而不指定端口时，默认就是连接`25565`，除非特别指定端口。本文假定您的服务器开放在25565端口上，后续步骤请自行对照修改。

3. 进入Lucky添加STUN规则![stun规则](/images/uses_of_full_cone_nat/lucky_stun_edit.png)

4. 获得公网地址![stun结果](/images/uses_of_full_cone_nat/stun_result.png)

5. 若始终无法获得公网地址，说明你路由器的防火墙未开放，需要设置端口转发或DMZ主机，参见**准备工作**第2条。或若你的路由器支持并打开了UPnP功能，可将stun规则里的UPnP开关打开，则将自动设置路由器的端口转发。

6. 至此，你已经拥有一个外网可访问的地址了。但缺点是，这个地址随时可能更换（ip与端口号都是动态的），并非固定的。想要固定下来，可以继续阅读接下来的步骤。

7. (进阶)首先把ip地址固定下来，可以使用ddns，在lucky中直接设置。本文不再赘述。

8. (进阶)本文重点讲解端口号的固定。我们使用srv解析。