# Rocky 安装 Rust

我采用了使用虚拟机的方案。<u>VirtualBox + Rocky8 + VS Code</u> 进行编码。

## 准备

1. 下载 [virtualBox](https://www.virtualbox.org/wiki/Downloads) 虚拟机
2. 下载 [Rocky8](https://rockylinux.org/zh-CN/download) 镜像
3. 下载 [Vs Code](https://code.visualstudio.com/Download) 编辑器

## 安装

<include from="project-library.md" element-id="placeholder"/>

### 安装虚拟机软件

<include from="project-library.md" element-id="placeholder"/>

### 安装虚拟机

<include from="project-library.md" element-id="placeholder"/>

#### 系统设置

```bash
# ==== 设置网络 ====
vi /etc/sysconfig/network-scripts/ifcfg-ens33 #也有可能是其他名字

# 只修改下面的内容，其他地方不要改动
BOOTPROTO=static
ONBOOT=yes
IPADDR=实际IP
PREFIX=24 #子网掩码简写，等同于MASK=255.255.255.0
GATEWAY=实际网关 #网关
# 域名解析
DNS1=223.5.5.5
DNS2=8.8.8.8

# RockyLinux和CentOS8不再使用systemd管理网络服务，转为使用nmcli命令
nmcli c reload #加载网卡文件，c和connection都可以
nmcli c down ens33# 先关闭指定网卡
nmcli c up ens33# 再打开指定网卡

# ==== 修改源为阿里云源 ====
sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
    -e 's|^#baseurl=http://dl.rockylinux.org/$contentdir|baseurl=https://mirrors.aliyun.com/rockylinux|g' \
    -i.bak \
    /etc/yum.repos.d/Rocky-*.repo

sudo dnf makecache

# ==== 安装基础软件 ====
sudo dnf update -y && sudo dnf install -y vim epel-release tar wget curl

# ==== 关闭防火墙 ====
sudo systemctl stop firewalld
sudo systemctl disable firewalld
# ==== 关闭selinux ====
sudo vim /etc/selinux/config
# 修改以下内容
SELINUX=disabled
```

完成设置后关机打快照

### 安装 Vs Code

<include from="project-library.md" element-id="placeholder"/>

### 安装 Rust

```Bash
# 安装Rust依赖项
sudo dnf install cmake gcc make curl clang -y

# 安装rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# 启用环境变量
source ~/.profile
source ~/.cargo/env

# 检查rust版本确认正确安装
rustc -V
```