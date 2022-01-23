---
title: Optimize
date: 2021-12-18 17:47:54
type: "about"
top_img: https://s2.loli.net/2021/12/29/HUiyLnJDzF2hKuo.jpg
---

> 博客的优化任重道远，一口吃不成胖子，有时间就优化一下，完善页面，将优化时间以及优化的相关内容记录下来，做个笔记吧，算是一个循序渐进的过程！
>

Date By：2021.12.20

### 1. 博客评论功能优化

这几天捣鼓着博客的评论功能，看到很多形形色色的评论插件，比如来必力，畅言等等之类的。功能是各不一样，网上教程是关于某一类的评论插件的介绍，不是很全面。

* [Hypercomments](https://www.hypercomments.com/):官网登录，目前只支持谷歌账号登录，国内有万里长城阻隔，翻墙注意安全！

* [畅言](http://changyan.kuaizhan.com/): 评论的时候会进行手机验证，不能直接评论，畅严实现评论系统，需要备案号，那太麻烦了。

* [友言](http://www.uyan.cc/)：对于友言，看了一下网上的介绍，我觉得确实是我比较喜欢的类型了，支持匿名评论功能（默认是关闭的，需要手动开启）。功能确实可以，但是完犊子，官方网页进不去，没法。应该是国外的网站，需要翻墙。

* [来必力](http://livere.com/)：百度了一下，韩国的，可以访问官网，看不懂韩文，注册页面点出来不晓得怎搞了，再见！


#### 2. 百度统计优化

除了想要增加一个博客的评论功能，当然还要添加博客的访问数据分析啦，博客被多少人访问，打开次数，访问时间全部交给了百度统计，在百度统计官网注册了一个账号，绑定了我的博客网址，获取到一段代码，代码含有id，编辑`主题配置文件`，修改字段 baidu_analytics 字段值设置成自己的百度统计脚本的id，已经获取了id，配置到了站点配置文件里面，等上传看效果。


### 3. 自定义本站底部版权申明

我放弃了。

### 4. theme大更新，由NexT改为Butterflt

今天又发现了一个好看的：`butterfly`,hexo-theme-butterfly是基于Molunerfinn的hexo-theme-melody的基础上进行开发的。今天晚上搞这玩意又一晚上了，next主题的配置文件，三方插件太多了，有些功能还挺麻烦的，花了大心思，主要是排版不是很喜欢，现在好了，next也不配了，配置文件待会上传到github上吧，改的挺多了，之后要是再想用NexT配一下hexo的`_config.yml`文件就行了。把butterfly配置好了以后一下子就舒服多了，但是还有很多样式没实现，scc/js文件没引入，大概的排版算是出来了。挺喜欢butterfly风格的。

* 很多设置优化都单独封装在yml配置了（配置很方便，加载很nice，主题更新也方便）。

* 主题中的个人文件不要放在Hexo根目录的source文件夹里的源文件夹中，升级主题会覆盖掉，可以在source文件夹里新键文件夹。

* 友链单独封装yml文件，不用写index.md了


### 5. 站内文章图片获取方式

将我的CSDN当一个图床，图片从[CSDN](https://blog.csdn.net/m0_62145574?spm=1001.2101.3001.5343)网址里面取，这样会有[CSDN](https://blog.csdn.net/m0_62145574?spm=1001.2101.3001.5343)的logo，头疼,然后就是用了网上比较火的免费白嫖图床服务器，哈哈哈哈。

### 6. 语言英文转中文

站内文字从英文改为中文，各文章字数、发表时间、更新时间都为中文。

### 7. 音乐导航栏并尝试添加音乐系统

如果想使用音乐页面，很多人都会推荐安装hexo-tag-aplayer这款插件。 这款插件通过Hexo独有的标签外挂，我们可以很方便的写入一些参数，插件就会帮我们生成对应的html。
如果使用插件，在markdown中要这样写：

``` javascript
{% meting "000PeZCQ1i4XVs" "tencent" "artist" "theme:#3F51B5" "mutex:true" "preload:auto" %}
```
其会被渲染为:

``` javascript
<div id="aplayer-uxAIfEUs" class="aplayer aplayer-tag-marker meting-tag-marker" data-id="000PeZCQ1i4XVs" data-server="tencent" data-type="artist" data-mode="circulation" data-autoplay="false" data-mutex="true" data-listmaxheight="340px" data-preload="auto" data-theme="#3F51B5"></div>
```

1. 安装音乐插件
``` bash
$ npm install --save hexo-tag-aplayer
```
2. 在主题配置文件_config.yml中custom下引入插件依赖的js和css。

``` javascript
custom_js:
  - //cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.js  #/APlayer#/APlayer依赖
  - //cdn.jsdelivr.net/gh/metowolf/MetingJS@1.2/dist/Meting.min.js  #/APlayer依赖
custom_css:
  - //cdn.jsdelivr.net/npm/aplayer/dist/APlayer.min.css   #/APlayer依赖
```

3. 然后在网站的根目录下的配置文件_config.yml中填上以下代码：

``` markdown
# aplayer
aplayer:  
  meting: true  
  asset_inject: false
```

4. 建立音乐页面

使用命令创建音乐界面，比如命名为playlist。

``` cmd
$ hexo new page playlist
```

打开网站根目录source\playlist\index.md根据hexo-tag-aplayer文档书写即可。
自建或者用别人的歌单https://y.qq.com/n/yqq/playlist/7724497259.html#stat=y_new.profile.create_playlist.click&dirid=3 按照一下格式书写即可。

``` javascript
{% meting "7724497259" "tencent" "playlist" "theme:#3F51B5" "mutex:true" "preload:auto" %}
```

有关`{% meting %} `的选项列表如下:

> 选项 默认值 描述
id 必须值 歌曲 id / 播放列表 id / 相册 id / 搜索关键字
server 必须值 音乐平台: netease, tencent, kugou, xiami, baidu
type 必须值 song, playlist, album, search, artist
fixed false 开启固定模式
mini false 开启迷你模式
loop all 列表循环模式：all, one,none
order list 列表播放模式： list, random
volume 0.7 播放器音量
lrctype 0 歌词格式类型
listfolded false 指定音乐播放列表是否折叠
storagename metingjs LocalStorage 中存储播放器设定的键名
autoplay true 自动播放，移动端浏览器暂时不支持此功能
mutex true 该选项开启时，如果同页面有其他 aplayer 播放，该播放器会暂停
listmaxheight 340px 播放列表的最大长度
preload auto 音乐文件预载入模式，可选项： none, metadata, auto
theme #ad7a86 播放器风格色彩设置

5. 添加音乐页面到导航菜单内即可

``` java
 menu:
    - { key: "音乐", link: "/playlist/", icon: "iconfont icon-music" }
```

### 8. 添加友链导航页

目前不会，只会添加一个超链接，等待完善！

### 9. Markdown语法笔记

1.打出空格的方式
　　　①使用 &emsp；来打出空格，注意最后一个分号是英文格式，此处为中文格式。
　　　②使用全角的空格。这种方法需要设置输入法。我使用的搜狗输入法，需要在输入法上右键点入属性设置，开启shift+space的快捷键才可切换至全角。

### 10. 首页文章封面的图片

之前都是用统一的图片当文章封面图片，比较统一，但是这样使得文章的辨识度不高，所以选择优化一下，在新建md文件的时候，在文件中加一个cover属性+图片地址就可以实现每篇文章自定义封面。Nice！

### 11. 博客文章评论系统已完善

本站评论系统Twikoo，一个简洁、安全、免费的静态网站评论系统，刚好本站主题对于这个评论系统兼容。

相比于一些国外的评论系统，这个访问速度要快上不少。


### 12. 站内音乐系统努力开发中...

### 13. 站内分享功能，喜欢的文章可以分享至qq，微博，微信以及其他社交软件。
在以下第三方随便选了一个，好像都是免费的。可点击查看。但是我不会弄，再见。
[AddThis](https://www.addthis.com/)
[sharejs](https://github.com/overtrue/share.js/)
[AddToAny](https://www.addtoany.com/)

### 14. 主页文章显示评论数，文章字数统计模块显示评论次数

### 15. 新增站内在线聊天功能。
[tidio](https://www.tidio.com) 

发送信息我就能在后台实时看到喔。

### 16. 新增实时访客地图
在项目根目录`source`下`_data`新建了`widget.yml`文件。


### 18. 新增b站追番页面(追番页面已删)

防止请求次数过多插件不再自动获取番剧数据，所以请根据自己的需要在 `hexo generate` 或 `hexo deploy `之前使用以下命令更新番剧数据！

``` bash
$ hexo bangumi -u 
```
删除数据命令:

``` bash
$ hexo bangumi -d
```

### 19. 新增天气显示(和风天气)
理论上根据用户定位显示当地天气

### 20. 服务器安全加固
> + 创建秘钥对，绑定服务器
+ 取消密码验证登录服务器
+ 修改默认SSH端口
+ 禁ping

### 21. 导航栏变更
将技术外链跟资源分享页面合并，新增关于本站页面


