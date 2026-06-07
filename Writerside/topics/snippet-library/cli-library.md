---
is-library: true
title: cli Library
---


<snippet id="cli-lib-scoop">

Scoop 是一款专为 Windows 设计的命令行软件包管理工具，它能让你像 Linux 系统一样通过命令快速安装、更新和卸载软件。
其核心优势包括：

- **无需管理员权限**：默认安装在用户目录，避免权限问题。
- **绿色便携化**：软件独立存放，不污染系统注册表。
- **依赖自动处理**：自动配置环境变量和依赖项（如 Java、Python）。
- **海量软件仓库**：支持主流开发工具、实用小软件甚至 GUI 应用。

<tip>该段文字出现于
    <a href="https://blog.csdn.net/m0_74412436/article/details/145394440">用 Scoop 优雅管理 Windows 软件：安装、配置与使用全指南</a>
</tip>
</snippet>

<snippet id="cli-lib-install_scoop">

```powershell
<#
如果要安装来自微软商店的软件，而且不确定id的情况下，直接打开微软商店的网址
例如：https://apps.microsoft.com/detail/9pg2dk419drg?hl=zh-CN&gl=CN
其中detail后面的9pg2dk419drg便是该应用的id，注意此方式只支持msstore源的软件，如果是winget源则无效

HEIF 图像扩展：9pmmsr1cgpwg
Microsoft 照片：9WZDNCRFJBH4
WebP 映像扩展：9PG2DK419DRG

视频相关的可以转用potplayer
# Windows 媒体播放器：9wzdncrfj3pt
# Web 媒体扩展：9n5tdp8vcmhs
# VP9视频扩展:9n4d0msmp0pt
#>


# ===================== 修改执行策略 =====================

# 获取当前生效的执行策略
$currentPolicy = Get-ExecutionPolicy

# 判断策略是否为 Restricted
if ($currentPolicy -eq 'Restricted') {
    # 调整执行策略，并限制在当前用户
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
}

# ===================== 设置 Scoop 安装位置 =====================

# 设置Scoop的安装位置
$env:SCOOP = 'D:\SoftWare\Scoop' # 根据自己的实际需求修改

# 设置USERSCOOP环境变量到用户范围
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')

# ===================== 安装 Scoop =====================

# 为官方脚本增加代理
#Invoke-RestMethod https://gh-proxy.org/https://raw.githubusercontent.com/scoopinstaller/install/master/install.ps1 | Invoke-Expression
# gitee镜像优化库，自动设置代理(偶尔会抽风)
Invoke-WebRequest -useb scoop.201704.xyz | Invoke-Expression

# ==================== 设置Scoop Bucket (修改Bucket为国内源) ====================

# 检查是否安装git
if (Get-Command git -ErrorAction SilentlyContinue) {
    Write-Host "git已安装"
} else {
    # 下载7zip、git的json文件
   
    ## 1. 确保目录能正常访问
    New-Item -ItemType Directory -Force -Path "$env:SCOOP\buckets\main\bucket"
   
    ## 下载7zip.json
    Invoke-WebRequest -Uri "https://gh-proxy.org/https://raw.githubusercontent.com/ScoopInstaller/Main/master/bucket/7zip.json" -OutFile "$env:SCOOP\scoop\buckets\main\bucket\7zip.json"
   
    ## 下载git.json
    Invoke-WebRequest -Uri "https://gh-proxy.org/https://raw.githubusercontent.com/ScoopInstaller/Main/master/bucket/git.json" -OutFile "$env:SCOOP\scoop\buckets\main\bucket\git.json"
   
    ## 下载git
    scoop install git
}

Write-host "========删除原始bucket========" -ForegroundColor Cyan

Write-host "Delete main..."
# 删除原始Bucket
scoop bucket rm main

$github_is_true = Read-Host "请输入是否能正常访问Github(Y/N)"

if ($github_is_true.Trim() -eq "Y") {
    Write-host "========添加官方库========" -ForegroundColor Cyan

    Write-host "add ScoopInstaller/Main..."
    scoop bucket add main https://gh-proxy.org/https://github.com/ScoopInstaller/Main # 增加main bucket的代理
   
    Write-host "add ScoopInstaller/Extras..."
    scoop bucket add extras https://gh-proxy.org/https://github.com/ScoopInstaller/Extras # 增加Extras bucket的代理
   
    Write-host "add chawyehsu/dorado..."
    scoop bucket add dorado https://gh-proxy.org/https://github.com/chawyehsu/dorado # chawyehsu维护的国内软件bucket，添加代理
   
    Write-host "add nerd-fonts..."
    scoop bucket add nerd-fonts https://gh-proxy.org/https://github.com/matthewjberger/scoop-nerd-fonts # 增加nerd-fonts bucket的代理
   
    write-host "其他仓库请查看https://scoop.sh/#/buckets"
} else {
    Write-host "========增加国内源Bucket========" -ForegroundColor Cyan
   
    # 增加国内源Bucket
    Write-host "add main..."
    scoop bucket add main https://gitee.com/scoop-installer/Main
   
    Write-host "add extras..."
    scoop bucket add extras https://gitee.com/scoop-installer/Extras
   
    Write-host "add nerd-fonts..."
    scoop bucket add nerd-fonts https://gitee.com/scoop-installer/scoop-nerd-fonts
   
    Write-host "add dorado..."
    scoop bucket add dorado https://gitee.com/scoop-installer/dorado
   
    write-host "其他仓库请查看https://gitee.com/organizations/scoop-installer/projects"
   
  
}

# 刷新scoop，从指定的git仓库下载repo
scoop update

# 配置scoop config
Write-host "========配置scoop========"

if ($github_is_true.Trim() -eq "Y") {
    $scoop_repo_url = "https://gh-proxy.org/https://github.com/ScoopInstaller/Scoop"
} else {
    $scoop_repo_url = "https://gitee.com/scoop-installer/scoop"
   
    # 设置scoop的url代理，用于加速下载GitHub上的软件
    scoop config URL_PROXY "https://pd.zwc365.com"
    write-host "若要添加/修改URL代理，请使用scoop config URL_PROXY <url>添加/修改"
    write-host "若要删除URL代理，请使用scoop config rm URL_PROXY"
}

Write-host "设置scoop源代码的git仓库" -ForegroundColor Cyan
scoop config SCOOP_REPO $scoop_repo_url
scoop update # 刷新


# 安装winget
$winget_is_true = Read-Host "是否安装 winget(Y/N)"
if ($winget_is_true.Trim() -eq "Y") {
    scoop install winget
    # 安装UWP依赖
    $f = "$env:TEMP\Microsoft.Services.Store.Engagement.appx"
    Invoke-WebRequest -Uri "http://tlu.dl.delivery.mp.microsoft.com/filestreamingservice/files/3a37e79e-03be-4796-8d25-00a6cd45cc97?P1=1777804089&P2=404&P3=2&P4=nyrmFAK2cyQM47DQvd2viQ1cd9OpPGyENNAl3vMb6v9L8%2fDqc48E4OBY9zcMjvMo4HWtOVyUrhsunBfmOxyG1w%3d%3d" -OutFile $f
    Add-AppxPackage $f
    Remove-Item $f

}

# 检查winget是否正常执行
& winget ($LASTEXITCODE -eq 0) {
    write-host "winget缺少依赖"
}

$download_vc_redist_url_x86 = "https://download.visualstudio.microsoft.com/download/pr/7a47a870-bdd8-4301-9619-349e12b16c5d/E7267C1BDF9237C0B4A28CF027C382B97AA909934F84F1C92D3FB9F04173B33E/VC_redist.x86.exe"
$download_vc_redist_url_x64 = "https://download.visualstudio.microsoft.com/download/pr/7a47a870-bdd8-4301-9619-349e12b16c5d/E7267C1BDF9237C0B4A28CF027C382B97AA909934F84F1C92D3FB9F04173B33E/VC_redist.x64.exe"

try {
    Write-Host "⬇️ 正在下载依赖..." -ForegroundColor Cyan
    Invoke-WebRequest -Uri $download_vc_redist_url_x86 -OutFile "$env:TEMP\VC_redist.x86.exe" -UseBasicParsing
    Invoke-WebRequest -Uri $download_vc_redist_url_x64 -OutFile "$env:TEMP\VC_redist.x64.exe" -UseBasicParsing

    # 🛡️ 解除 Windows 安全锁定（避免“已阻止此应用”或智能屏幕警告）
    Unblock-File -Path "$env:TEMP" -ErrorAction SilentlyContinue

    # 安装vc_redist
    Write-Host "🚀 正在静默安装..." -ForegroundColor Cyan
    $Process_x86 = Start-Process -FilePath "$env:TEMP\VC_redist.x86.exe" -ArgumentList /quiet -Wait -PassThru
    $Process_x64 = Start-Process -FilePath "$env:TEMP\VC_redist.x64.exe" -ArgumentList /quiet -Wait -PassThru

    # 📊 判断安装结果
    if ($Process_x86.ExitCode -eq 0 -and $Process_x64.ExitCode -eq 0) {
        Write-Host "✅ 安装成功！" -ForegroundColor Green
    } else {
        Write-Host "❌ 安装失败，退出码: $($Process.ExitCode)" -ForegroundColor Red
        write-host "请手动安装vc_redist.x86.exe"
        write-host "请手动安装vc_redist.x64.exe"
        Start-Process "$env:TEMP"
    }
} catch {
    Write-Host "❌ 执行异常: $_" -ForegroundColor Red
    exit 1
} finally {
    # 🗑️ 清理临时文件
    if (Test-Path "$env:TEMP") {
        Remove-Item -Path "$env:TEMP" -Force -ErrorAction SilentlyContinue
        Write-Host "🧹 已清理临时安装文件" -ForegroundColor Gray
    }
}

<#
新系统安装scoop后安装winget可能会缺少dll导致无法使用经过检查缺少的是vc_redist，安装下面两个版本的vc_redist即可解决
https://download.visualstudio.microsoft.com/download/pr/7a47a870-bdd8-4301-9619-349e12b16c5d/E7267C1BDF9237C0B4A28CF027C382B97AA909934F84F1C92D3FB9F04173B33E/VC_redist.x86.exe
https://download.visualstudio.microsoft.com/download/pr/6f02464a-5e9b-486d-a506-c99a17db9a83/8995548DFFFCDE7C49987029C764355612BA6850EE09A7B6F0FDDC85BDC5C280/VC_redist.x64.exe
#>
```

</snippet>