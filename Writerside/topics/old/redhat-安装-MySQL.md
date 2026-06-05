<show-structure depth="3"/>

# RedHat 系安装 MySQL

<secondary-label ref="old-secondary-updated-218"/>

> 提示：
> 1. 本次安装的是 MySQL 最新的版本
> 2. 使用的系统为 CentOS7
> 3. 每个页面的链接在文档中都有，可以直接点击跳转

---

> 碎碎念
> 
> RPM 安装太费劲了！
>
> RPM 不会帮你解决依赖问题，所以安装是有安装顺序的 QAQ。
>
> 如果有想折腾的可以参考这篇文章：[LinuxCentOs7 下安装 MySQL8.0.26 详细教程，本人亲测可行，仅供大家避雷](https://blog.csdn.net/qq_45263520/article/details/124084190?spm=1001.2014.3001.5502)
>
{style="note"}

## 一、导入官方仓库

<tldr>

<format color="Tomato">先决条件</format>

1. wget 命令行下载工具
2. vim 编辑器，可选下载，Linux 自带的 vi 可以替代

```bash
yum install -y wget vim
```
</tldr>

### 打开官网，进入下载界面

下载页面链接:<https://dev.mysql.com/downloads/>

![](download-page.png)

- 蓝色框内的是官方提供的 Linux 软件仓库
- 红色框内的是 RedHat 系 Linux 使用的包管理器 yum 的仓库

### 选择官方仓库

yum 库链接：[MySQL Yum Repository](https://dev.mysql.com/downloads/repo/yum/)

![](OS-Version.png)

根据安装的系统版本点击相应的 download 即可。

### 复制链接

![](copy-link.png)

打开终端，执行以下命令

```bash
# 下载链接对应的文件，官方库的rpm文件
wget https://dev.mysql.com/get/mysql80-community-release-el7-5.noarch.rpm
# 安装下载的rpm文件
rpm -ivh mysql80-community-release-el7-5.noarch.rpm # 直接复制链接的后半部分即可
```
## 二、安装 MySQL {id="redhat-H2-install_MySQL"}

```bash
yum install -y mysql-community-server 
```

## 三、配置 MySQL {id="redhat-H2-config_MySQL"}

### 启动 MySQL {id="redhat-H3-start_MySQL"}
```bash
systemctl start mysqld  # 如果是出现的下面一行，表示MySQL安装没有问题

Redirecting to /bin/systemctl start  mysqld.service # 只有第一次启动会出现
```

**相关命令，此部分内容可以跳过，只是对 mysqld 服务简单操作的罗列**

```bash
# 查看MySQL服务运行状态
systemctl status mysqld
# 启动MySQL
systemctl start mysqld
# 关闭MySQL
systemctl stop mysqld
# 重启MySQL 
systemctl restart mysqld
```


### 初次登录 {id="redhat-H3-login_one"}

#### 获取 MySQL 随机生成的初始密码 {id="redhat-H4-get_init_passwd"}

```bash
grep 'temporary password' /var/log/mysqld.log
```

举例说明：

```bash
# 密码在最后,每个人都是不同的
grep 'temporary password' /var/log/mysqld.log

2016-11-14T04:34:41.742516Z 1 [Note] A temporary password is generated for root@localhost: sNKz9yEdzw%/
```

<deflist>
<def title="从执行结果来看">

1. 账户是`root`
2. 主机名是`localhost`
3. 初始密码是`sNKz9yEdzw%/`
</def>
</deflist>

#### 修改密码

> 本处我采用的顺序
>
> 1. 修改初始密码
> 2. 修改配置文件
> 3. 修改为简单密码
>
> 注意：顺序可以变换，比如先修改配置文件，再登录修改初始密码等

MySQL 默认开启了 `validate_password` 插件，该插件要求密码至少：
- 一个大写字母
- 一个小写字母
- 一个数字
- 一个特殊字符
- 长度至少 8 个字符


```bash
mysql -uroot -p
# 输入上一步获取到的初始密码，进入MySQL

# 先将密码修改为Pw12345.可以在mysql终端中进行操作
ALTER user 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'Pw12345.';
```

> 如果想使用标准 MySQL 安全水平进行学习，到此为止，后面不需要看。
> 
> 如果只是为了玩，可以继续。
>
{style="warning"}

```bash
# 查看安全策略
show variables like 'varlidate%';
```

当前生效的安全策略如下

![](Security-policy.png)


修改安全策略有两种方式

> 1. 直接在 mysql 终端中使用 set 进行修改
> 2. 在 my.cnf 文件中修改。

<tabs>
<tab title="mysql终端使用set">

```bash
# 修改密码策略等级为 low
set global validate_password.policy=low;
# 密码的最小长度
set global validate_password.length=6;
# 设置密码中至少要包含 0 个大小写字母
set global validate_password.mixed_case_count=0;
# 设置密码中至少包含 0 个数字
set global validate_password.number_count=0;
# 设置密码中至少包含 0 个特殊字符
set global validate_password.special_char_count=0;
```
</tab>
<tab title="修改my.conf">

```bash

vim /etc/my.cnf

# 是否能将密码设置成当前用户名
validate_password.check_user_name=OFF

# 密码的最小长度，也就是说密码长度必须大于或等于 4
validate_password.length=4

# 密码必须包含的大写、小写字符数
validate_password.mixed_case_count=0

# 密码必须包含的数字个数
validate_password.number_count=0

# 密码强度 0 只检查长度
validate_password.policy=0

# 密码必须包含的特殊字符个数
validate_password.special_char_count=0
```
</tab>
</tabs>

无论选择哪种方案都要重启 MySQL

```bash
# 首先重启 MySQL
systemctl restart mysqld

# 进入 MySQL
mysql -uroot -pPw12345.

# 查看当前的安全策略
show variables like 'varlidate%';
```

当前生效的安全策略如下：

![](new-Security-policy.png)

此时就可以设置简单密码 1234 了

```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY '1234';
# 注意，此处的root是获取初始密码时得到的。同样localhost也是
```

此时使用 SQL 客户端连接虚拟机可以发现无法连接，是防火墙的问题。

#### 禁用 firewall
```bash
# 临时关闭，重启后恢复
systemctl stop firewalld

# 关闭开机自启动
systemctl disable firewalld
```

#### 进入 MySQL 操作，更新权限

```bash
# 进入mysql数据库
use mysql;

# 查看当前mysql的用户及权限
select user,host from user;

# 如果账户所对应的host不是'%',使用下面的命令更新
update user set host='%' where user='root';

# 刷新内容
FLUSH PRIVILEGES;
```

完成上述内容后，使用 sql 客户端链接即可

## 总结

> MySQL在win/mac和Linux上的安装差距实在是大，差点疯掉

1. 下载 MySQL 的官方库
2. 导入官方库
3. 下载 MySQL
4. 修改密码
5. 连接物理机 sqlyog

以上就是本文档的五个部分

---

> 警告：不知为何 MySQL 的安全插件 validate\_password 消失，卸载后重新安装也没有加载成功。
> 另，修改密码的另一个方式就是直接卸载密码安全插件
> 方式如下：
>
> 卸载插件
>
> ```bash
> UNINSTALL COMPONENT 'file://component_validate_password';
>```
>
> 安装插件
> ```bash
> INSTALL COMPONENT 'file://component_validate_password';
> ```