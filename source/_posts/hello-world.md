---
title: 前言：搭建成功
date: 2021-12-17 17:50:20
tags: 
- 记录
top_img: https://s2.loli.net/2021/12/29/HUiyLnJDzF2hKuo.jpg

---
芜湖！前言不知道写啥，离了个大谱。此处省略三千个字。

### 关于博客：
* 主要写博客的原因还知识简单的将自己的学习到的知识及解决思路，整理下来，方便自己日后能用到的时候能够翻翻，快速的找到。就像高端的程序猿CV手法一样朴实无华！！！
* 开源还能给别人看到，供别人阅读跟学习也挺不错的。之后在网页上加点标签栏，音乐栏啥的，不然是有点单调看起来，慢慢搞吧，做样式挺复杂的。
* 没事干的时候写写博客，将学到的零七零八的知识点记录下来，本来记性就不好。写博客加强个人记忆，提高思维能力，好多东西看的时候可以理解，但是过了一段时间因为业务上没有用到就容易忘，如果记录下来，那相当于把自己当时理解的思路给整理下来，这样下次重新再看的时候可以很快的想起来。
* 第二点就是写的时候可以促进我去把一些不明白的东西弄明白，虽然没几个人看，但是如果写出来了就一定能保证自己可以把要写的整个流程捋清楚，自己掌握的东西还是自己的。芜湖！
* 研究官方文档，锻炼自学能力，锻炼笔记能力，锻炼项目能力
* 第三点当然是产生学习动力啦，督促自己多多学习，不然网站就一两篇文章，看着磕碜。
### 致自己

我常常觉得计算机和互联网的发明给人类带来了如此大的方便，让人们不用阅读说明书就知道如何上手，但是偏偏编程的道路又是如此艰辛。
BUT,担心未来，不如现在好好努力。这条路上，只有奋斗才能给你安全感。不要轻易把梦想寄托在某个人身上，也不要太在乎身旁的耳语，因为未来是你自己的，只有你自己才能给你自己最大的安全感。你说不喜欢现在的生活，不喜欢朝九晚五，不喜欢从周一忙碌到周五；你的梦想是环游世界，是睡觉睡到自然醒、数钱数到手抽筋。其实，你纯粹只是懒，是在逃避责任，生活不是励志故事，也不要再拿“梦想”这种伟大的词当借口了。不如先在有限的境地里，努力做好自己。

* (C站有很多大牛)

![图片](https://img-blog.csdnimg.cn/img_convert/95f1aa94fa79757a1c6ffeba6cbb5661.png)

---

###  Start My blog journey！

## Hexo 命令笔记


### vim File esc(SAVE and exit)

``` bash
$ esc : wq
```
### Create a new post

``` bash
$ hexo new "My New Post"
```

### clean

``` bash
$ hexo clean
```

### Run server

``` bash
$ hexo server
```


### Generate static files

``` bash
$ hexo generate
```
>小结：修改了文件，执行以下命令
```bash
hexo clean   #清理一下缓存
hexo g   #重新加载一下页面
hexo server   #重新运行 
```
### Deploy to remote sites

``` bash
$ hexo deploy
```

### 安装hexo工具代码(管理员身份运行cmd)

``` bash
$ npm install hexo-cli -g
```

### 在git bash命令窗口生成ssh秘钥

``` bash
$ ssm-keygen -t rsa -C "2760317896@qq.com"
```


### 测试秘钥是否绑定成功
``` bash
$ ssh -T git@github.com
```


### 初始化hexo(生成hexo项目)

``` bash
$ hexo -init
```

### 拉取github主题

``` bash
$ git clone https://github.com/hexojs/hexo-theme-phase.git themes/phase
```

## Butterfly命令

* 安装Butterfly命令
```bash
npm i hexo-theme-butterfly
```
* 安装或者切换主题后hexo s 打开http://localhost:4000/后出以下错误:
```bash
extends includes/layout.pug block content #recent-posts.recent-posts include includes/recent-posts.pug include includes/pagination.pug
```
因为缺少安装pug依赖和渲染插件依赖，需要安装：
```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```
