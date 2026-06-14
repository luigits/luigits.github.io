# 安装 Windows Terminal

安装 Windows Terminal 也有多种方案，推荐方案其实也和上一篇相同：使用 scoop 或安装包直接安装

## Scoop 安装

```powershell
PS D:\> scoop search windows-terminal
Results from local buckets...

Name                     Version      Source   Binaries
----                     -------      ------   --------
windows-terminal         1.24.11321.0 extras
windows-terminal-preview 1.25.1322.0  Versions

PS D:\> scoop install windows-terminal
```

## 通过 Winget 安装

```powershell
PS D:\> winget search "windows terminal"
Name                     Id                                Version      Source
--------------------------------------------------------------------------------
Windows Terminal         9N0DX20HK701                      Unknown      msstore
Windows Terminal Preview 9N8G5RFZ9XK3                      Unknown      msstore
Windows Terminal         Microsoft.WindowsTerminal         1.24.11321.0 winget
Windows Terminal Preview Microsoft.WindowsTerminal.Preview 1.25.1322.0  winget
PS D:\> winget install --id Microsoft.WindowsTerminal.Preview
```