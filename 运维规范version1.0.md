#操作系统安装流程及初始化规范#

>    V1.0

##操作系统安装流程##
1. 服务器采购
2. 服务器验收并设置raid
3. 服务商提供验收单，运维验收负责人签字
4. 服务器上架
5. 资产录入
6. 自动化安装系统
7. 新服务器划入装机VLAN
8. 根据资产清单上的mac地址自定义安装
> 1)机房  2）机房区域  3）机柜  4）服务器位置  5）服务器网线接入端口  6）端口的mac地址 7）profile 操作系统、分区、预分配的ip地址、主机名、子网、网关、DNS、角色等

自动化装机平台、安装

* IP:192.168.56.12
* 主机名：linux-node2.example.com
* 掩码:255.255.255.0
* 网关:192.168.56.2
* DNS:192.168.56.2
<pre>
cobbler system add --name=linux-node2 --mac=00:50:56:31:6C:DF --profile=CentOS-7-x86_64 --ip-address=192.168.56.12 --subnet=255.255.255.0 --gateway=192.168.56.2 --interface=eth0 --static=1 --hostname=linux-node2.example.com --name-servers="192.168.56.2" --kickstart=/var/lib/cobbler/kickstarts/CentOS-7-x86_64.cfg
</pre>

##操作系统安装规范
1. 当前公司使用的操作系统为CentOS 6.x和CentOS7.x，均为x86_64位操作系统，安装系统时必须使用公司的cobbler服务进行自动化安装，禁止自定义设置
2. 版本选择：
	1. 数据库：统一使用公司cobbler服务器上的CentOS-7-DB专用的profile
	2. web应用：统一使用公司cobbler服务器上的CentOS-7-web的专用profile

##系统初始化规范
###初始化操作
* 设置DNS 192.168.56.111 192.168.56.112
* 安装Zbbix Agent: Zabbix Server:192.168.56.11
* 安装Saltstack Minion: Saltsatck Master：192.168.56.11
* 让histroy记录时间的操作步骤
<pre>
export HISTTIMEFORMAT="%F %T `whoami`" 
source /etc/profile
</pre>
* 日志记录操作
<pre>
export PROMPT_COMMAND='{ msg=$(history 1 | { read x y; echo $y; });logger "[euid=$(whoami)]":$(who am i):[`pwd`]"$msg"; }'
</pre>
* 内核参数优化
* yum仓库
* 主机名解析

###目录规范
* 脚本放置目录： /opt/shell
* 脚本日志目录： /opt/shell/log
* 脚本锁文件目录： /opt/shell/lock

###服务安装规范
* 源码安装路径: /usr/local/appname.version
* 创建软链接：ln -s /usr/local/appname.version /usr/local/appname

###主机命名规范
  **机房名称--项目--角色--服务--集群--节点.域名**

例子：
	idc01-itemname-api-nginx-bj-node1.shop.com

###服务启动用户规范
所有服务统一使用www用户，uid为1000，除负载均衡需要监听80端口使用root启动外，其他所有服务必须使用www用户启动，使用大于1024的端口