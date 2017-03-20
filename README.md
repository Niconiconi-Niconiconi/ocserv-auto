Auto Install Ocserv Server for CentOS&RedHat 7
=======================================
这是 ocserv 在 CentOS 7 和 RHEL 7 的一键安装脚本，使用 epel 的二进制源，可以在最小化安装环境的 CentOS 7 和 RHEL 7 下一键部署 ocserv。

支持自动判断 firewalld 和 iptables，安装前请确保其中之一在运行。

* 支持自动判断防火墙，请确保 Firewalld 或者 iptables 其中一个是 active 状态；
* 默认采用用户名密码验证；
* 默认配置文件在 /etc/ocserv/ 目录；
* 安装时会提示你输入端口、用户名、密码等信息，也可直接回车采用默认值，密码是随机生成的；
* 安装脚本会关闭 SELINUX；
* 自带路由表，只有路由表里的 IP 才会走 VPN，如果你有需要添加的路由表可自行添加，最多支持 200 条；
* 如果你有证书机构颁发的证书，可以把证书放到脚本的同目录下，确保文件名和脚本里的匹配，安装脚本会使用你的证书，客户端连接时不会提示证书错误。


AnyConnect VPN用户配置

ocserv的主用配置文件就是/etc/ocserv.conf，我们可以把通用的配置放在这个里面。拿路由表举例，可以把这个文件里面的路由表全部#掉。比如usera需要智能上网，而userb不用，首先创建用户并设置密码

1 ocpasswd -c /etc/ocserv/ocpasswd usera

2 ocpasswd -c /etc/ocserv/ocpasswd userb

创建usera的配置文件(无后缀）

nano etc/ocserv/config-per-user/usera

把路由表放进这个文件。

编辑/etc/ocserv.conf使用用户名登入，

auth = "plain[passwd=/etc/ocserv/ocpasswd]"

config-per-user = /etc/ocserv/config-per-user/

config-per-group = /etc/ocserv/config-per-group/

之后重启ocserv服务。现在使用usera登入VPN就是智能上网，而userb就是全局VPN。

另外，用户配置支持 route，dns，限速，独立子网网段等参数，可以自行摸索。

Unset to enable bandwidth restrictions (in bytes/sec). The setting here is global, but can also be set per user or per group.

#rx-data-per-sec = 40000 

#tx-data-per-sec = 40000
