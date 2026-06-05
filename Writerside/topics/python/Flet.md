# Flet

<tldr>
<format color="Tomato">可以直接跳过本文档</format>
</tldr>

## 安装 Flet {id="flet_1"}

<include from="project-library.md" element-id="placeholder"/>

## 运行 Flet {id="flet_2"}

<include from="project-library.md" element-id="placeholder"/>

## 编译 Flet{id="flet_3"}

<include from="project-library.md" element-id="placeholder"/>

> **可以跳过本文档**
>
> 事实上，flet 提供的编译功能和常见的编译功能相差许多，或者说与我想象中的制作成安装包并不符合，编译后的目录一就能看到明显的
> python 库 (并非影子),因此在实际需求中可以转向使用 NSIS 等安装包制作工具来完成此步骤。
> 当然，若是憋着一口气，想要完成这一项功能也是可以的，继续看就是了

这一步比较复杂，需要其他的软件配合

> flet 除了原生的 build 之外，还有一个 pack 命令，他是通过 Pyinstaller 进行编译打包

## 需要的软件

| 编辑器  |      vscode      |
|:----:|:----------------:|
| 网络加速 |     steam++      |
| 额外的库 | pip-system-certs |

## 报错

### SSL

报错信息如下

```powershell
HTTPSConnectionPool(host='github.com', port=443): Max retries exceeded with url: /flet-dev/flet/releases/download/v0.84.0/flet-build-template.zip (Caused by ││ SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1081)')))
```

若不安装 *pip-system-certs* 则会出现 SSL 报错，主要是虚拟环境的证书无法连接 GitHub。

### Flet 代码模板 {id="flet_4"}

flet 有自己的命令初始化代码模板，此命令是通过从 GitHub 下载 zip 文件解压覆盖的方式完成，因此需要能正确从 GitHub 下载文件。

steam++ 可以直接让 flet 下载需要的组件，若下载报错超时则可以在本地 *flet-cli* 中修改 ==build_base.py==，通过代理链接的方式完成下载

```python
# 原始 url
DEFAULT_TEMPLATE_URL = (
    "https://github.com/flet-dev/flet/releases/download/"
    "v{version}/flet-build-template.zip"
)
# 修改后的 url
DEFAULT_TEMPLATE_URL = (
    "https://gh-proxy.org/https://github.com/flet-dev/flet/releases/download/"
    "v{version}/flet-build-template.zip"
)
```

此链接不仅仅用于 `flet create` 命令，还用于 `flet build` 命令

### 缺少文件

这个报错比较长，自己看过报错信息基本上就是说缺少 DLLs 和 Lib，此时只需要将需要的文件手动复制过去即可，不需要额外的设置

> **完整报错信息**，这部分很长，可以直接忽略
>


想要知道的详细写可以仔细看看上面的完整报错，如果不想浪费时间就继续往后看

```powershell
# 涉及到的目录
## 初始位置
<项目目录>/build/flutter/build/windows/x64/python
<Project>/build/site-packages
## 目标位置
<项目目录>/build/flutter/build/windows/x64/runner/Release
# 涉及到的操作
## 复制目录Lib
<项目目录>/build/flutter/build/windows/x64/python/Lib -> <项目目录>/build/flutter/build/windows/x64/runner/Release/Lib
## 复制目录DLLs
<项目目录>/build/flutter/build/windows/x64/python/DLLs -> <项目目录>/build/flutter/build/windows/x64/runner/Release/DLLs
## 复制site-packages
<项目目录>/build/site-packages -> <项目目录>/build/flutter/build/windows/x64/runner/Release/site-packages
```

除此之外还有几个删除操作
，不过无伤大雅，只需要保证上面的目录中有所需要的文件让 build 命令操作即可

再次整理上面的目录，都是复制到 <ui-path>&lt;Project&gt;/build/flutter/build/windows/x64/runner/Release</ui-path> 因此进行第一次实验

1. 将 build 命令下载的 Cpython 完整复制到 <ui-path>&lt;Project&gt;/build/flutter/build/windows/x64/python</ui-path>
2. 不复制 <ui-path>site-packages</ui-path>

经过尝试，python 目录复制后其他工作 build 命令自行完成。

```powershell
# 获取项目根目录
$parent = Get-Location

# 定义源和目标子目录名称
## 源目录
### Cpython路径
$cpython = Join-Path $parent (Join-Path "build/flutter/build" (Get-ChildItem -Path "build/flutter/build" -Name "build*"))
### site-packages
$packages  = Join-Path $parent "build/site-packages"
## 目标目录
$destPython = Join-Path $parent "build/flutter/build/windows/x64/"

# 复制 cpython 的全部内容到 destPython
Write-Host "正在从 '$cpython' 复制到 '$destPython' ..."
try {
    Copy-Item -Path "$cpython\*" -Destination $destPython -Recurse -Force -ErrorAction Stop
    Write-Host "复制完成。"
} catch {
    Write-Error "从 $cpython 复制到 $destPython 时出错：$_"
}
```