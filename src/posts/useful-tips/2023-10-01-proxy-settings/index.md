# 网络代理设置解决方案合集

## 引言

本文用于记录一些`命令行`工具的代理设置方法，`UWP`应用的代理设置，`WSL2`的代理设置等等

## UWP 应用走代理

> 引用自：[https://www.duohuohub.cn/threads/49/#post-101](https://www.duohuohub.cn/threads/49/#post-101)

微软的 OneDrive 真好用… 但偶尔会出现怎么样都无法登陆的情况

本质上是因为 微软 uwp 应用默认直连，所以开了代理就用不了

### 【最终解决方案】

无脑批量解除所有 UWP 应用限制（管理员）

```powershell
Get-ChildItem -Path Registry::"HKCU\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Mappings\" -name | ForEach-Object {CheckNetIsolation.exe LoopbackExempt -a -p="$_"}
```

### 【其他尝试（未生效）】

- Clash 中开启 UWP 应用联网限制解除工具
- Clash 开启 Tun 模式
- 重置 OneDrive
- 出国（bushi

## PowerShell 下使用代理

> 引用自：[https://anuoua.github.io/2019/08/11/配置Powershell命令行代理/](https://anuoua.github.io/2019/08/11/配置Powershell命令行代理/)

在日常 windows 环境开发中，powershell 经常需要用到代理，但是每次去系统变量中配置环境变量还是显得过于麻烦了，所以我们可以定义两个命令：

- 应用代理环境变量
- 取消代理环境变量

直接在 powershell 中调用这两命令就方便很多了。

### 编写脚本

1. 使用管理员权限打开`PowerShell`，并执行`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

2. 输入`code $PROFILE`使用 vscode 打开配置文件。

3. 在配置文件中输入以下代码：

```powershell
function set_proxy_variable {
    Set-Item Env:http_proxy "http://127.0.0.1:33210"  # 代理地址
    Set-Item Env:https_proxy "http://127.0.0.1:33210" # 代理地址
}

function unset_proxy_variable {
    Remove-Item Env:http_proxy
    Remove-Item Env:https_proxy
}

New-Alias -Name spp -Value set_proxy_variable
New-Alias -Name upp -Value unset_proxy_variable
```

### 使用

- `spp`启用代理
- `upp`取消代理

## CMD 下使用代理

打开`cmd`，输入以下命令

```bash
set HTTP_PROXY=http://127.0.0.1:1080
set HTTPS_PROXY=http://127.0.0.1:1080
```

不过要注意的都是，上面设置的命令行代码作用的范围是一次性的，也就是关闭 cmd 窗口后，就失效了，如果需要永久生效的话，是需要配置系统的环境变量的

## Git 设置代理

```bash
#有些朋友好像为什么设置http和socket5其实设置哪种都是可以的，具体看你们自己代理软件都支持的协议有哪些，就可以了
#记得修改端口号，比如我的是1080，记得改成自己代理软件的所配置的端口号
#下面配置的都是全局，如果需要设置局部的把【--global】删除即可。
 
#http代理
git config --global http.proxy 'http://127.0.0.1:1080'
#https代理
git config --global https.proxy 'http://127.0.0.1:1080'
#http代理
git config --global http.proxy 'socks5://127.0.0.1:1080'
#https代理
git config --global https.proxy 'socks5://127.0.0.1:1080'
 
#取消http代理
git config --global --unset http.proxy
#取消https代理
git config --global --unset https.proxy
```