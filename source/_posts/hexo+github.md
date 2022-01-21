---
title: 基于hexo+github搭建个人博客
date: 2021-12-18 09:33:22
cover: https://s2.loli.net/2021/12/29/9VLdUJq56b1DmFv.png
top_img: /img/14.jpg
tags: 
- 教学
---
Date By: 2021-12-18 11:39:06

### 小故事：

> 早晨，猫医生要去河对岸的小山村给小动物们治病。但一到河边，猫医生就傻眼了——原来，前几天连下了几场大雨，把小桥给冲走了，这可怎么过河呢?猫医生着急地想着。听到这个消息，鸭子、小马、小狗、小兔······都闻声赶来，帮猫医生过河。小马自告奋勇地说：“我来，我来，我腿长，我驮你过河!”猫医生爬上了小马的背，因为猫医生太胖，小马被压死了，全剧终.....
>

### tips:
>When you finish reading my article teaching, you will certainly build their own independent personal blog site.
I will teaching you for mother's teach hand by hand.(我将进行手把手保姆级教学)



### 首先你得准备`node.js` 、 `git` 、 `github`个人账号

写博客内容不寒颤的话，再装一个`vscode`，手敲`vim`命令修改文件qio实很头疼。


以管理员身份运行`cmd`检查`node.js`是否配置完成，代码如下

``` bash
$ node -v
```
检查`git`是否配置完成，代码如下

``` bash
$ git --version 
```
主要是用到`hexo`工具，在`cmd`管理员命令窗口安装`hexo`工具，代码如下

``` bash
$ npm install hexo-cli -g
```
检查hexo工具是否安装成功

``` bash
$ hexo -v
```
检查node.js下的一款安装工具npm是否配置完成，代码如下(安装完node基本就能查出版本号)

``` bash
$ npm -v
```

我检查如下：
![21.png](https://s2.loli.net/2021/12/28/d5FRZVTW1Cst4Nz.png)

此时工具的准备工作完成啦！！！


### 第一步：搭建仓库

`github`网页注册账号，自行创建一个`respository`，注意：仓库名为`github账户名+.github.io`
在b站搜索`github`创建仓库教学，一抓一大把，这里不做缀叙，以我的为例，仓库名是：1760317896.github.io
`github`仓库作用就是充当远程服务器，存储我们的博客文件，让所有人可以去访问。



### 第二步：生成`SSH Keys`

随便进入一个文件夹当中，随便进，不要管，因为生成SSH秘钥跟你在哪个文件下操作没有关系。一定要在文件夹里面右击选择git bash打开git命令窗口，直接敲如下代码

注意：双引号下面的是你的`github`注册的邮件地址，自行修改，github的账户名跟邮箱号很重要！
``` bash
$ ssh-keygen -t rsa -C "1760317896@qq.com"
```
敲完按四次回车，这时命令就会在电脑本地新建一个文件夹，文件夹默认在C盘下面的users/administrator/.ssh文件
打开`ssh`文件，会生成两个文件，分别是 `id_rsa` 和 `id_rsa.pud`  两个文件
点开 `id_rsa.pud` 文件，复制出秘钥，点开`github`网页的个人头像的`settings`，点到`SSH and GPG keys`，新建一个秘钥，秘钥的title随便取名，秘钥粘贴上去，点击 `Add SSH Key` 就创建成功了！

然后要确定我们是否绑定成功，回到刚才那个git命令窗口，输入以下命令，不需修改，照着输入即可。

``` bash
$ ssh  -T git@github.com  //测试ssh是否绑定成功
```
输入命令敲回车，窗口会询问`yes / no[fingerprint]?`，不用担心，手敲yes，回车即可。
出现successfuly authenticated 就表示绑定`SSH`成功！


### 第三步，生成hexo文件

新建一个文件夹，文件夹名字随意取，在哪个盘新建都行，你记得地址就行，以后这就作为你博客项目的本地文件夹了，新建完成是一个空的文件夹，点进去，右击选择`git bash`，敲以下命令

``` bash
$ hexo init //初始化hexo，成功后会发现文件夹多了好多文件，就是hexo文件,可能会出现链接超时的情况
```
过程中可能会出现连接失败的情况，不要紧，因为`github`是国外的网站，链接不稳定，再次敲相同代码重试即可。一般第二次第三次就可以初始化啦！

初始化成功会提示：start blogging with Hexo！且发现文件夹多出好多文件，继续敲命令

``` bash
$ hexo s     #静态生成本地的hexo页面
```
`hexo s`命令敲完回车就会有一个本地的访问地址，比如我的出现的是http://localhost:4000/，复制地址，就可以访问啦
(复制的时候不要`Ctrl+C`,这是停掉本地生成页面服务的快捷键,你需要选中右击点击Copy,当然如果你手速较快，已经停止服务了，页面显示404错误，问题不大，重新输入`hexo s`即可)

此时本地的博客搭建完成！本地搭建完成剩下就是发布到`github`服务器。

> 如果hexo s命令没有运行成功的话，那就是没装服务的原因，在git面板输入以下命令:
```bash
npm install hexo server   #装server服务，再运行hexo server或者hexo s就可以了。
```
顺带把hexo deploy的依赖包也装上吧，代码如下：
```bash
npm install hexo-deployer-git --save  
```

### 第四步，将本地博客项目上传到github仓库。

在生成的`hexo`众多文件里面，找到 `_config.yml`文件，直接在文件夹中打开编辑也行，在`git bash`命令窗口敲`vim`命令也行,当然这需要你有Linux命令基础，我建议直接在文件夹点开编辑就好，打开方式选记事本，vscode打开都可。
修改 `_config.yml`文件夹的deploy，修改为以下内容:(直接拉到最下面)

``` bash
deploy:
  type: git
  repository： clone仓库地址     #仓库http地址，clone出来，加上.git
  branch: main
```
注意：冒号`：`后面必须输入一个空格。
      冒号`：`后面必须输入一个空格。
      冒号`：`后面必须输入一个空格。
`clone`仓库地址： 就是你在`github上面新建仓库的网址+.git`。仓库url复制克隆出来大家都会吧，不会的自行百度，很简单的。
`type` 和 `branch` 两个参数的值都是一样的，大家自行照抄就好啦。

修改完成就进行一下操作

在`hexo`项目的文件夹，右击选择 `git bash`, 输入以下命令

``` bash
$ hexo g   #生成页面的命令
```
生成页面命令敲完回车等待完成，继续输入以下命令

``` bash
$ hexo d  #上传到github 的命令 这是最关键的一步
```
输入完hexo d敲回车，会验证你的github账号，出来一个小输入框，要求输入你的username，输入github用户名即可。
用户名输完会马上要求输入密码

注意！！！！！！！！！！！！！！！！！！！！！！！！！！！

此时要求输入的密码并非github登录密码!!!
此时要求输入的密码并非github登录密码!!!
此时要求输入的密码并非github登录密码!!!

密码先不要急着输入，小输入框拉到一边，

我们回到`github`网页

点击到`github`网页，点击头像的`Settings`，点击`Developr Settings`，点击`Personal access tokens`

添加一个令牌，令牌名字随便取，令牌的权限建议全部勾选，影响不大，然后点击下面的generate token

就会出现令牌，很长，复制粘贴是不行的，必须纯手打进那个小输入框，自行手动输入。

注意：令牌很重要！！！
     令牌很重要！！！
     令牌很重要！！！
一定要把令牌复制出来放到一个记事本或者文档里面，离开页面以后就再也查看不到令牌了，这也是github出于安全性能方面，2021年的大更新，所以令牌要复制出来保存好。下次再发布到github上时还是需要纯手打输入令牌。（肠子都悔青了，令牌自己都忘记复制出来了）。

令牌手打输入到小输入框内，点击OK，此时就会将本地的`hexo`项目全部上传至`github`仓库，回到`github`仓库即可查看。

### 如果输入 `hexo d` 出现以下错误：

fatal: unable to auto-detect email address (got 'z@DESKTOP-DPE3A08.(none)')
error: src refspec HEAD does not match any
error: failed to push some refs to 'https://github.com/seekwhale13/seekwhale13.ithub.io.git'

### 可以先运行以下代码：
```bash
git config --global user.email "你的邮箱"
git config --global user.name "你的gihub名字"  #注意有两个`-`
```
例如我上传失败，输入以下命令回车，然后重新上传(hexo deploy)即可：
```bash
git config --global user.email "1760317896@qq.com"
git config --global user.name "ha1357"
```

### 参考文献

[哔哩哔哩](https://www.bilibili.com/) 众所周知，b站是学习网站

[百度](https://www.baidu.com/)  不懂就问度娘，自行搜索

### 结尾

芜湖！！ 结束了，昨天下午弄了两个小时把个人博客搭建完成，一步一步来，基本没有什么错误，`hexo+github`搭建个人博客还是比较简单的，如果你搭建的过程中遇到什么问题可以提出来，大家一起交流交流，仓库名就是我的QQ号，欢迎添加。
虽然这个教学有点抽象，没有图片支撑相应的步骤具体操作，但是大家将就着看吧，昨天搭建的时候属实是忘记截图了。有时间出一个视频教学。昨晚在b站看到一个也是比较流行的个人博客框架，基于GO语言的hugo博客框架，搭建起来比hexo操作更简单，hexo搭建失败的小伙伴，可以去试试hugo。[hugo搭建教学直达](https://www.bilibili.com/video/BV1q4411i7gL)。博客主题，博客内容大家自行修改，网上也有很多教学。推荐[CodeSheep](https://space.bilibili.com/384068749?spm_id_from=333.788.b_765f7570696e666f.1)这位娘心up主。也能会有小伙伴说，这搭建起来太简单了，有没有稍微难一点的，那是当然啦。你可以试试现Halo，Halo 是一款现代化的个人独立博客系统，给习惯写博客的同学多一个选择。大家搜Halo官网就会有官方文档，有具体部署教学，我大概看了一下官网的主题库，简直是太多、太好看、太个性化了！部署起来也会比较复杂，准备搭建的小伙伴需要准备一个云服务器，服务器管理软件堡塔面板，安装docker工具等等，详细教学可以参照参考文献自行搜索。

> #### 本文首次发布于2021年12月18号，转载请说明出处并附上本站网址！感谢。

### 最后，感谢阅读。









