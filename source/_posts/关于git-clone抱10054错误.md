---
title: 关于git_clone报10054错误
date: 2022-01-16 12:58:31
tags: 记录
cover: https://s2.loli.net/2022/01/16/IfxEHiWK4sX2zm7.png
top_img: /img/20.jpg
---

## git clone url.git 报10054错误，具体错误如下：
```bash
Administrator@PC202006091958 MINGW64 /d/GitHub_clone/themes
$ git clone https://github.com/blinkfox/hexo-theme-matery.git #这是git命令
Cloning into 'hexo-theme-matery'...
fatal: unable to access 'https://github.com/blinkfox/hexo-theme-matery.git/': OpenSSL SSL_read: Connection was reset, errno 10054
```
## 解决方法一
### 执行以下两行代码即可：
```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## 再次git 克隆即成功， 如下图

![7.png](https://s2.loli.net/2022/01/16/KD1WE2UotRcPXSY.png)

## 解决方法二
### 当解决方法一不能解决报错问题时，手动配置git的代理，git客户端输入以下命令
```bash
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy http://127.0.0.1:1080
```
>tip：绝大部分情况下方法一就能决解决大部分克隆报错问题。