# 【软路由】基于 openwrt 的校园网多拨教程

## 0x00 前言

高中曾在网上冲浪时听说过`多拨`，但因为光猫后台不好进，改不了`桥接`，故作罢。

如今步入大学，从信技学长那得知校园网也能`多拨`，躁动的心便蠢蠢欲动起来。奈何本人脸皮薄，学长也守口如瓶，故并没有第一时间实现。

后来给宿舍买了`软路由`，就想着把`多拨`实现了。一通上网查询后，终于找到几篇细节较为详细的教程。

> [拿什么拯救你，我的校园网 —— 校园网优化之单线多拨](https://blog.csdn.net/m0_59496782/article/details/121862378)
>
> [讲讲多拨的额外骚操作（多拨附加教程）](https://blog.csdn.net/m0_59496782/article/details/128275861)
>
> [南信大校园网稳定 | 多拨 | 软路由 | 硬路由 | 保姆级教学 | 一步到位 | openwrt|pandavan](https://www.cfanz.cn/resource/detail/ygPBPyXqEYGLO)

理论上来说，参考这三篇就已经能解决 90% 大学的校园网多拨了。

## 1x00 准备工作

本人使用 ASUS-ACRH17 路由器作为软路由，接下来讲解刷入 openwrt 的过程。其他型号请自行搜索刷入方法。

## 0xFF 附录

校园网自动登录脚本

```shell
ip=$(curl -X GET http://10.255.255.46/api/v1/ip)
ip2=$(echo $ip | cut -d \" -f 6)
curl -s -d  "{\"username\":\"手机号\",\"password\":\"密码\",\"channel\":\"运营商\",\"ifautologin\":\"0\",\"pagesign\":\"secondauth\",\"usripadd\":\"$ip2\"}" http://10.255.255.46/api/v1/login
```