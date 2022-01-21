---
title: Ngork内网穿透
date: 2021-12-22 13:35:25
tags: 
- Ngork 
- 教学
cover: https://s2.loli.net/2021/12/29/JAwUl7Q8BfHaSNI.png
top_img: /img/14.jpg
---
前言：
# 自己电脑上的项目如何让别人访问

一般自己在电脑上写了一个项目 ，自己把项目跑起来，生成一个本地地址，可以通过本地网址进行访问项目，但是怎么样让别人来访问你的项目，这就需要用到Ngork工具了。

> 原理：把外网地址通过服务器映射，映射到自己的本地地址，这样别人访问外网，就间接访问了你的本地网站

使用场景：

比如你是乙方，你承包了甲方的项目，当你写好了项目，甲方要验收，你要把自己的项目给甲方看呀，你总不能把自己的电脑带到甲方面前操作吧，这里就可以用到Ngork内网穿透，你给甲方一个网站，就可以访问你本地的网址啦！

还有一种就是你是学生，你在学校做了一个项目，你想让同学，老师，其他人访问你的网址，这时候用Ngork就很方便，分享资源啥的。

服务器映射过程也很简单哈，没有什么难度，很容易上手，看完就会。

# 实现内网穿透方法

## 第一步：注册账号并登陆

 去Ngork官网注册一个账号，点[这里](https://www.ngrok.cc/login.html)直接去注册吧。

## 第二步：购买实名认证机会

 因为之前有人使用网站服务器做非法的事情，导致Ngrok网站被公关部门调查了，加强了实名认证功能，想要搭建一个服务必须要实名认证，这其实不算什么，主要是人家实名认证接口是第三方了，要收取一次性费用两元，确实把热爱白嫖的我给无语到了。左侧的导航栏是有实名认证的，点击，然后好像还需要微信关注公众号来着，操作不难，你们按照网页提示操作吧。

## 第三步：开通隧道

(1)右侧导航栏实名认证上面有一个隧道管理，点击显示两栏，隧道管理和开通隧道，首先要去开通隧道,你可以购买宽带大，速度大的服务器，也可以选择白嫖的0元服务器，这里演示就用免费的。点击立即购买。

![1](https://img-blog.csdnimg.cn/42db95237daa46d4810365bfcd0b3189.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rC45LiN56eD5aS05qmZ5Y-Z54y_,size_20,color_FFFFFF,t_70,g_se,x_16)

(2)然后就是添加隧道了，隧道名称随便取，前置域名你也可以自定义，隧道协议建议使用http的，当然你也可以试试其他的协议。

注意！！！！！！！！！！！！！！！

注意！！！！！！！！！！！！！！！

图上忘记标了，本地端口号一定要填，比如你的端口号是`8080`，你就把图上的`80`改为`8080`，类推。


然后点击添加隧道。

![2](https://img-blog.csdnimg.cn/d186309a730341f8a06f7ccdaab32a87.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rC45LiN56eD5aS05qmZ5Y-Z54y_,size_20,color_FFFFFF,t_70,g_se,x_16)

(3)添加隧道成功进入管理隧道页面，此时准备工作做完就去下载Ngork工具吧。

![3](https://img-blog.csdnimg.cn/4fa0f00134ca47a3a85b1eeeeb7ff07e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rC45LiN56eD5aS05qmZ5Y-Z54y_,size_20,color_FFFFFF,t_70,g_se,x_16)

(4)下载完成解压，点开解压完成的文件夹，运行Ngork工具

![4](https://img-blog.csdnimg.cn/65b31be694f24cd5b2e134e301b1c2ec.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rC45LiN56eD5aS05qmZ5Y-Z54y_,size_18,color_FFFFFF,t_70,g_se,x_16)

（5）将隧道ID复制进去回车就完成了，此时就映射完毕了，就可以访问此时映射的外网地址了，也就是隧道管理页面的赠送的域名，访问试试效果吧。

![5](https://img-blog.csdnimg.cn/aebdda34c2894a3f8e0219d42ac4c376.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5rC45LiN56eD5aS05qmZ5Y-Z54y_,size_20,color_FFFFFF,t_70,g_se,x_16)

## 结尾
### 这上面免费的服务器是美国的，好处就是免费的，缺点是如果访问的人多了，就容易崩，不适合多人同时访问。

Ngork客户端那边刷资源就说明有人在访问，如果把Ngork客户端小窗口关闭，服务器就关闭了，别人访问就会显示404，所以还有一点就是本地服务要一直是启动的，因为你映射的那个本地地址，放到你的浏览器能访问，外网映射，通过外网才能访问，很好理解吧，你不能本地的服务不跑起来，直接去映射，先不说能不能映射成功，成功了你去访问外网是不是一定没任何东西啊。

## 最后，感谢阅读。


