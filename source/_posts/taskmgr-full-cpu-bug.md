---
title: 【修机】Windows电源管理Bug引起的CPU占用虚高100%、风扇狂转
date: 2023-10-14 17:05:15
tags: 
- 转载
- 翻译
- Windows
- CPU
- PC
- Fixing

categories: 修机
---

> https://www.andrewmunsell.com/blog/oculus-rift-bug-power-plan-configuration

# TLDR
管理员权限下运行以下命令即可解决问题：
```bash
PowerCfg /SETACVALUEINDEX SCHEME_CURRENT SUB_PROCESSOR IDLEDISABLE 000
PowerCfg /SETACTIVE SCHEME_CURRENT
```