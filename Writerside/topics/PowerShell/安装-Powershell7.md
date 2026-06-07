# 安装 PowerShell7

安装 PowerShell7 有多种方案，通过安装包、winget、scoop......

## 通过安装包安装

没什么好说的，通过浏览器下载 PowerShell7 的安装包，直接双击安装即可

## 通过 Winget 安装

> 要求系统中安装了 winget，而安装 winget 也有多种方式，大致为通过 `Add-AppxPackage` 命令和通过第三方工具安装

```powershell
PS D:\> winget search --id Microsoft.PowerShell
Name               Id                           Version Source
---------------------------------------------------------------
PowerShell         Microsoft.PowerShell         7.6.1.0 winget
PowerShell Preview Microsoft.PowerShell.Preview 7.7.1.0 winget

PS D:\> winget install --id Microsoft.PowerShell
```

如果使用稳定版就按照我上面的命令执行即可，如果要用预览版记得修改 id 为 *Microsoft.PowerShell.Preview*

Microsoft store 的安装软件也是通过 winget 命令

## 通过 Scoop 安装

```powershell
PS D:\> scoop search powershell
Results from local buckets...

Name                     Version         Source Binaries
----                     -------         ------ --------
powershell-beautifier    1.2.5           main
powershell-yaml          0.4.12          main
powershelleditorservices 4.6.0           main
powershell-lts           7.4.13          dorado
powershell-preview       7.7.0-preview.1 dorado
powershell               7.6.1           dorado

PS D:\> scoop install powershell
```

## 通过套娃安装

套娃是什么方式？

其实我用的就是套娃的方案，scoop 安装 winget，winget 安装 powershell

我套娃的原因是我需要使用微软商店中的特定软件，但我又不想安装微软商店 ~~毕竟让我电脑让微软商店卡死好几次了，电脑已经用了八年了，没办法~~

推荐的方案：软件包安装或 scoop 安装

> scoop 如何安装？
> 
> 如果要问这个问题，那就需要在左上角切换实例为 CLI 了。
> 
> 因为 scoop 是个命令行工具，因此便将 scoop 放置在 CLI 中。