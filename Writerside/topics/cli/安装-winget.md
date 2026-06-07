# 安装 winget

能用 winget 安装软件的大多听过 Scoop，至于其他的我也不会，就用 Scoop 示例吧

```powershell
PS D:\> scoop search winget
Results from local buckets...

Name           Version  Source   Binaries
----           -------  ------   --------
winget-ps      1.12.440 main
winget         1.28.240 main
wingetcreate   1.12.8.0 main
winget-preview 1.29.170 Versions
PS D:\> scoop install main/winget
```

注意 `winget-ps` 是一个 PowerShell 的扩展，并非是 winget 程序本身

新系统安装 winget 会出现没有运行库的情况，在 powershell 中运行表现形式为不报错，也没有任何输出

因此需要安装 `extras/vcredist2022`，在 scoop 中检索一下 vcredist，安装最新版本即可