---
title: 如何将Hexo博客部署到云服务器
date: 2022-01-03 21:55:28
tags: 
- 教学
top_img: /img/20.jpg
#cover: https://s2.loli.net/2022/01/03/FEqAMmPWH18LeGY.png
cover: https://s2.loli.net/2022/01/03/dOe1bnlyX2jRfgp.png
---
### 前言：
> 此前我的博客都是部署在github上，但是github老是抽风，访问速度慢，当我拥有域名和服务器后，迫不及待的就要将个人博客项目部署到我的云服务器上，然后通过域名去访问，就大功告成，皆大欢喜啦。
但是在网上搜索了很多部署到服务器的教程，杂七杂八，有些很麻烦，有些通过ssh远程连接服务器敲linux命令，有些步骤还不详细，有些甚至跳步部署，确实是刚开始跟着那些教学踩了很多坑啊，部署好久，不是权限不够就是git完出现各种错误。
第一次部署问题太多，我解决不了，新建的文件夹还多，有点晕，就把服务器格式化了一次，着实头疼，浪费了太多的时间。
第二次是因为服务器被攻击，又格式化了一次，部署次数较多，嘿嘿，有经验。
所以当我部署成功后，第一时间就把我部署过程以及部署遇到的一些问题整理出来，记录下来，发一篇博客，帮助后面的同学部署少踩一点坑。此文章不断完善中。

把我的部署成功过程分享出来给大家查看。

### 注意：以下所有命令，一框多条命令，建议一行一行执行！！一行一行的复制到服务器运行。(大佬忽略)

## 一、准备工作
1. 已经备案的域名，已经解析绑定好服务器ip地址了，
2. 一台云服务器，我这边使用的阿里云服务器，(TentOS 8系统)
3. 在本地有基于hexo框架的博客项目
> 基于其他框架的博客如果要部署到服务器此文章仅供参考，我没试过，步骤应该差不多哈。
4. 云主机环境搭建（Git，宝塔面板一键部署Nginx） 使用git自动化部署博客 。

## 二、修改本地Hexo博客的配置文件
1. 找到你本地项目的站点配置文件(不是主题配置文件)
打开D盘，blog文件夹，打开blog文件夹，打开_config.yml, 找到deploy，修改为以下内容
```bash
deploy:
type:git
repo: git@server:/var/repo/hexoBlog.git
branch: master
```
> #server改为你的服务器ip地址或者解析后的域名
> #例如我改为  repo: git@skkya.cn:/var/repo/hexoBlog.git

### 本地只需改这一个配置即可。原理就不赘叙了，大家部署过项目到GitHub上面都知道。

## 三、开始部署服务器

### 1. 终端连接服务器，我使用的`finalShell`工具，比较方便

### 2. 安装[宝塔面板](https://www.bt.cn/)来一键部署Nginx，命令如下：
```bash
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```
#### 上面命令宝塔面板7.8.0的安装，安装过程会询问`y`or`n`,输入`y`即可（大约2分钟完成面板安装）。
> 提示：宝塔Linux面板7.8.0版本是基于Centos/Debian/Ubuntu开发的，为了最好的兼容性，请使用以上系统
系统兼容性顺序：
Centos7.x > Debian10 > Ubuntu 20.04 > Cenots8.x > Ubuntu 18.04 > 其它系统
注意：Centos官方已宣布在2020年停止对Centos 6的维护更新，各大软件开发商也逐渐停止对Centos 6的兼容，新服务器不建议使用Centos 6。
我的8系统兼容性还是靠后的，所以很多教学都是推荐CentOS 7系统，我觉得问题不大，照样可以部署成功。

### 3.配置文件
安装完成后我们顺便把配置文件也弄好，后期就不需要弄了。
安装完成后会显示面板后台地址·账号·密码。打开浏览器，访问面板外网登陆地址，选择Nginx的部署方案，静静等待部署。
#### 具体操作图如下：
![2.png](https://s2.loli.net/2022/01/04/7rmPx8BtXU5EwGg.png)

![3.png](https://s2.loli.net/2022/01/04/FHtZJdqe6oT8Wkv.png)

等5分钟Nginx部署完成，点击网站-添加站点-输入域名(没有域名的输入自己服务器的IP地址)-底部的PHP版本选择”纯静态”(其他不改 或者根据自己的习惯来改)-提交。

网站创建完成后点击`设置` -配置文件：
```bash
server
{


listen 80;

\# server_name 填写自己的域名(没有域名填写服务器ip地址也行)


server_name skkya.cn;   #没有域名填写服务器ip地址也行
index index.php index.html index.htm default.php default.htm default.html;


\# 这里root填写自己的网站根目录，修改为/var/www/hexo

root /var/www/hexo;

```

#### 站点的`设置`中的`配置文件`只要修改这两个就可以保存

然后点击设置-网站目录， 网站目录默认的就行了，我看有的教程里改成/var文件夹，然后发现无法保存，这对我来说又是一个大坑。
运行目录为`/`应该也可以，我上一次部署是这个，现在我设置的运行目录为`/var/www/hexo`，保存即可。

### 4.云服务器部署仓库

#### 以下步骤在finalShell终端敲命令
1. 安装git
```bash
yum install git   #回车会提示y/n是否安装，输入y即可
```
2. 创建Git账户
```bash
adduser git
```
3. 添加账户权限
```bash
chmod 740 /etc/sudoers
vim /etc/sudoers
```
4. 找到
Allow root to run any commands anywhere
root ALL=(ALL) ALL
添加以下内容:
```bash
git ALL=(ALL) ALL
```
![4.png](https://s2.loli.net/2022/01/04/tMlg9exoPV1wzOp.png)

保存退出(按 Esc 键退出编辑模式，输入”:wq” 保存退出)并改回权限:
```bash
chmod 400 /etc/sudoers
```
5. 设置git账户密码
```bash
sudo passwd git  #输入此命令敲回车就可以输入密码，自己设置，别忘记，输入敲回车会再次输入确认密码
```
6. 切换至git用户，创建 ~/.ssh 文件夹和 ~/.ssh/authorized_keys 文件，并赋予相应的权限
```bash
su git
mkdir ~/.ssh
vim ~/.ssh/authorized_keys
```
然后将前面部署本地Hexo静态博客中生成的`SSH`秘钥，就是`id_rsa.pub`文件中的公钥复制到`authorized_keys`中,如果没有SSH秘钥请空降本文目录第五章`生成ssh公钥`。

7. 在`本地`(不是服务器)Git终端中测试是否能免密登录git，其中下面SERVER为填写自己的云主机IP一定替换过来，执行输入yes后不用密码就说明好了（我这里没有免密成功但是不影响使用，你如果也没有可以放弃直接下一步步骤，反正我这边暂时找不到原因放弃了只是后期需要输入密码，不过还行也就1秒钟的事情，嫌麻烦的可以自己想办法解决一下）。
```bash
ssh -v git@SERVER   #执行此命令需要我输入密码，不影响，不能免密也就手动输个密码的事
```
8. 创建目录

### 这边执行下面的命令没有权限，需要切回root用户，这是个大坑，我搞半天，离谱，在此加上：
```bash
su root #敲回车会让你输密码，就输你自己的设置的git密码，如果不对就输入服务器的密码，我密码设置都一样，没有排查用的是哪个。然后在执行下面的命令
```
### 1. repo作为为Git仓库目录
```bash
mkdir /var/repo
chown -R git:git /var/repo
chmod -R 755 /var/repo
```
### 2. hexo作为网站根目录
```bash
mkdir /var/www
mkdir /var/www/hexo
chown -R git:git /var/www/hexo
chmod -R 755 /var/www/hexo
```
然后创建一个裸的 Git 仓库,返回root目录创建。
```bash
su root
cd /var/repo
git init --bare hexoBlog.git
```
创建一个新的 Git 钩子，用于自动部署 在 /var/repo/hexoBlog.git 下，有一个自动生成的 `hooks `文件夹。我们需要在里边新建一个新的钩子文件 `post-receive`。

```bash
vim /var/repo/hexoBlog.git/hooks/post-receive
```
按 i 键进入文件的编辑模式，在该文件中添加两行代码（将下边的代码粘贴进去)，指定 Git 的工作树（源代码）和 Git 目录（配置文件等）
```bash
# !/bin/bash

git --work-tree=/var/www/hexo --git-dir=/var/repo/hexoBlog.git checkout -f
```
然后，按 Esc 键退出编辑模式，输入”:wq” 保存退出。

修改文件权限，使得其可执行。
```bahs
chown -R git:git /var/repo/hexoBlog.git/hooks/post-receive
chmod +x /var/repo/hexoBlog.git/hooks/post-receive
```
### 到这里，我们的 Git 仓库算是完全搭建好了

重启宝塔面板服务
```bash
service bt restart
```
不出错显绿说明完成，如图：
![5.png](https://s2.loli.net/2022/01/04/D2ZrfNWbBqlhFJ7.png)


然后到你的本地博客项目根目录下`git bash`打开git面板，输入以下命令试试能够部署成功

```bash
hexo clean
hexo g -d
```
期间可能要输入密码，可能是免密登录，我hexo d是需要密码才能上传的，这个影响不大
打开浏览器输入你的域名或ip地址就可以看到你部署的Hexo博客了。至此，我们已经成功完成，并且访问自己的服务器端比访问github快多了.

## 四、常见问题
### 1. git权限不够

我在部署过程中，执行`hexo d`发现部署老是出错，什么权限不允许之类的，这里我们需要检查我们在上述的git操作部署是否使用了git用户操作，若是没有，需要给相应的目录更改用户组使用.
比如：
![1](https://gitee.com/mingkaikai/tuku/raw/master/640.webp.jpg#vwid=646&vhei=367)

我们需要给权限，在服务器输入以下命令：
```bash
chown -R git:git /var/repo/
```
这条命令递归的将repo目录及其子目录用户组设置为git。同时使用：
```bash
chown -R git:git /var/www/hexo
```
然后在去上传到服务器`hexo d`试试吧。我给玩权限就成功啦！！！

### 2. /var/www/hexo不允许被设置成网站目录

我也无法将站点网站修改为/var目录，但是本地通过`hexo d`是上传到/var目录，经过我的测试，，都改成了root /var/www/hexo，这个是对的

### 3. 宝塔面板外网地址无法访问

宝塔面板默认使用服务器8888端口，服务器这个端口没开，去你的服务器控制台，添加安全组，开启8888放行端口即可。点击服务器-安全组-服务器-入方向-手动添加安全组-添加8888端口(端口范围8888，授权对象为0.0.0.0/0),还是不会的建议自行百度哈，不难。控制台也有文档教学的。

### 4.SSH服务端口(默认端口为22)改了，之后再deploy报错
为了安全，我修改了`SSH`默认连接端口，在宝塔面板安全里面修改的，项目更新了，想再次上传，发现一直报错，刚开始以为需要重新给权限，最后发现是端口的问题，输入以下命令发现是端口的问题：
```bash
ssh -v git@SERVER   #server改成自己的服务器ip地址
```
报错如下：
![6.png](https://s2.loli.net/2022/01/08/72J6bX5DNOfuhAv.png)

>解决方法：
1. 将SSH连接端口重新改回默认端口。(暂时没找到其他解决方法，实在不行咱就不改端口吧)
2. 将默认端口改完以后再部署git权限(下次部署试试)。

## 五、生成ssh 公钥

在桌面右击，点击Git Bash Here打开命令行终端，执行如下命令（直接按三次回车生成密钥）
```bash
ssh-keygen -t rsa
```
这时命令就会在电脑本地新建一个文件夹，文件夹默认在C盘下面的users/administrator/.ssh文件
打开`ssh`文件，会看到两个文件，分别是 `id_rsa` 和 `id_rsa.pud`  两个文件
点开 `id_rsa.pud` 文件，复制出秘钥即可。

## 六、致谢

至此，搭建的过程就结束了...
感谢[铭kk个人博客](https://quefeixi.com/archives/18.html)的部署教学！本文是基于这篇文章完善的，第一次部署也是基于这篇博客成功的，期间也踩了不少坑，浪费的时间挺多，头发也掉不少哈。我给它完善了一下，还有两行命令没啥用来着，我没执行。以上内容就是根据我成功部署来写得步骤，一步一步来没有什么大毛病。如果遇到什么问题，欢迎在下方评论，一起解决。

### 本文首次发布于2022年1月4号，转载请说明出处并附上本站地址！感谢。

### 最后，感谢阅读
