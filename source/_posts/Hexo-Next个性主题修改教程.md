---
title: Hexo-Next+GitHub搭建个人博客&个性化配置
date: 2018-09-09 16:18:11
tags: [Hexo-Next,教程]
categories: 教程
description: 此文章用来记录博主的建站历程，以便备用，或者供人参考。
---
<!-- more -->
# 前言 #
此篇文章主要针对Hexo-Next主题下的Mist风格，如果有出入，请酌情参考。
** 文章最后会总结一下修改心得，授人以鱼不如授人以渔！ **
# 开始 #
> 如果你想深入了解或者想要更多细节的话请观看官方文档
  以下可能需要VPN才能访问
  [hexo](https://hexo.io/zh-cn/index.html)
  [hexo-next](https://theme-next.iissnan.com/)

首先，我们明确一下建立博客需要的要素：
* 一个成熟的博客模版（最好能提供各种风格，并且让程序员修改风格时候方便明了，不需要了解整个前端架构）
* 一个服务器，能够让我们将博客部署上去以便供人访问

那么有没有什么方法能够解决以上这两个问题呢？
于是乎，Hexo+GitHub这个方法出现了！也就是你现在看到的博客采用的方法。

## 前期准备 ##
首先，你需要安装
> [node.js](http://nodejs.cn/)
[git](https://git-scm.com/downloads)

其中node.js是hexo运行需要的环境，git是你部署博客时的需要。
接下来我们准备安装hexo。
```shell
// node.js安装hexo模块 -g代表-global全局安装
$ npm install hexo-cli -g
// hexo init [文件夹名]
$ hexo init blog

$ cd blog

// 安装npm的模块
$ npm install

// 本地启动hexo
$ hexo server
```

打开浏览器输入localhost:4000查看
如果你有看到Hexo几个大字说明安装Hexo成功

如果你不喜欢Hexo原生主题的话，就可以继续下载不同的主题，这里推荐<font color="#000000">Next</font>
```shell 
$ cd [your-hexo-site]
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
再打开<font color="#9b4400">[your-hexo-site]/_config.yml</font>（Hexo配置文件）
```yaml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```
这步目的就是将主题改为next
```shell
$ hexo clean  //清除缓存
$ hexo generate  //重新生成代码
$hexo server  //部署到本地

//然后打开浏览器访问 localhost:4000 查看效果
```

#  主题修改 #
这里主要介绍一下一些不常见的修改，基础部分请参照官方文档。

## 设定自定义首页 ##
网上的办法一看都很复杂，需要清空仓库之类的。
这里的首页指登录你域名后展示的页面，正常情况下就是你的博客主页。
但是如果你想用自定义的页面作为首页，再通过首页进入博客呢？
先介绍一下hexo的大致链路。
首先，通过<font color="#000000">hexo generate</font>生成一个对外发布的文件夹<font color="#9b4400">[your-hexo-site]/public</font>，public中的index.html文件就是我们访问站点时的初始页面，所以我们要做的就是把生成的博客首页放在另外一个文件夹中，让我们希望展现的首页替代原来的这个index.html，再在自定义首页中放一个相对链接可以跳转到博客首页。下面是让每次的<font color="#000000">hexo generate</font>都能按照这个思路生成的操作。
1. 打开Hexo配置文件<font color="#9b4400">[your-hexo-site]/_config.yml</font>
   大家看一下英文注释应该就可以了解到这边是运行<font color="#000000">hexo generate</font>时生成首页的配置。这里重点关注一下我们要改的path。
<font color="#000000">path</font>：即生成首页路径，默认首页在<font color="#9b4400">[your-hexo-site]/public/index.html</font>中。将其改成'blog'后，就会生成在<font color="#9b4400">[your-hexo-site]/public/blog/index.html</font>
```yaml
# Home page setting
# path: Root path for your blogs index page. (default = '')
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: '/blog/'
  per_page: 10
  order_by: -date
```
2. 我们将自定义主页放在<font color="#9b4400">[your-hexo-site]/themes/next/source</font>下，并将其改名为index.html。
因为在生成public文件夹时，hexo会将主题下的source文件原样拷贝，所以这时候你的首页文件就会自动被拷贝过去。
3. 有时候我们的自定义首页只想在进入开始时装装逼，不想在阅读文章时点击首页都跳转到自定义首页再进入博客首页，这时修改<font color="#9b4400">[your-hexo-site]/themes/next/_config.yml</font>。把home的地址改为/blog/就会访问public下的blog文件夹中首页！
```yaml
menu:
  home: /blog/ || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
```

## 添加评论、统计功能and建立标签、分类 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/_config.yml</font>注意是在next下的站点配置文件而不是之前的根目录下的配置文件！
具体设置请参照[<font color="#000000">Hexo-next主题设定</font>](https://theme-next.iissnan.com/getting-started.html)
第三方服务推荐的是开启评论&统计，我在这里使用的是<font color="#000000">来必力</font>&<font color="#000000">不蒜子</font>。

## 给页面添加背景图片 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_custom/custom.styl</font>，添加以下代码
```css
body{   
        background:url(图片链接);
        background-size:cover;
        background-repeat:no-repeat;
        background-attachment:fixed;
        background-position:center;
     }
```
其中的css样式可以根据你要的来改，以上内容可以让一个背景不重复的居中在页面上展示

## 文字背景色以及半透明设置 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_custom/custom.styl</font>，添加以下代码
```css
.content {
            border-radius: 10px;
            margin-top: 60px;
            background:rgba(颜色rgb,透明度) none repeat scroll !important;
         }
```
其中<font color="#000000">border-radius</font>是给文章背景设置圆角，<font color="#000000">margin-top</font>是设置文章到顶部的距离，其中属性可根据自己的需要进行调整。

## 侧边栏的背景图片以及伪半透明设置 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_custom/custom.styl</font>，添加以下代码
```css
.sidebar {
	background: url(/images/sidebar_2.png);
	background-size: cover;
    background-position:center;
    background-repeat:no-repeat;
}
```
是不是很奇怪，这里为什么没有关于透明度的调整，因为在这你如果加<font color="#000000">opacity</font>的话会导致文字的背景色一起变化。
这里提供两种方法：
一种是改用背景颜色，就像文字背景色一样<font color="#000000">background:rgba(颜色rgb,透明度) none repeat scroll !important</font>。
另外一种如果你执意想要使用图片的话，就使用PS对图片进行透明度调整，这样文字就不会随着背景改变。

## 来必力背景色 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_custom/custom.styl</font>，添加以下代码
```css
#lv-container {
    border-radius: 10px;
    background:rgba(#72C5FE,70%) none repeat scroll !important;
}
```

## 网页底部文字颜色以及背景色 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_custom/custom.styl</font>，添加以下代码
```css
.footer-inner {color: #71CFFF;}

.footer {
	background:rgba(#EBFFC9,70%) none repeat scroll !important;
}
```

## 更改分类计数和标签颜色 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_custom/custom.styl</font>，添加以下代码
```css
// 目录
.category-list-count {
	color: #F8FFC8 !important;
}
// 词云
.tag-cloud a {
	display: inline-block;
    margin: 10px;
	color: #00FF00 !important;
}
```
感觉对这种“配角”的更改教程比较少。之后几个也是对少见的配置修改。

## 更改侧边栏内作者以及描述 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_common/components/sidebar/sidebar-author.styl</font>，修改以下代码。
```css
// 这是修改你名字的字体大小&颜色
.site-author-name {
  margin: $site-author-name-margin;
  text-align: $site-author-name-align;
  color: #FFFFFF;
  //$site-author-name-color;
  font-weight: $site-author-name-weight;
  font-size: 24px;

}

// 修改你名字下的描述
.site-description {
  margin-top: $site-description-margin-top;
  text-align: $site-description-align;
  font-size: $site-description-font-size;
  color: #FFF9D8;
  //$site-description-color;
}
```

## 更改侧边栏日志以及其计数字体 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_common/components/sidebar/site-state.styl</font>，修改以下代码。
```css
// 计数
.site-state-item-count {
  display: block;
  text-align: center;
  color: #C3FF8B;
  //$site-state-item-count-color;
  font-weight: $font-weight-bold;
  font-size: $site-state-item-count-font-size;
}
// “日志”字体修改
.site-state-item-name {
  font-size: 16px
  //$site-state-item-name-font-size;
  color: #FFFFFF;
  //$site-state-item-name-color;
}
```

## 更改侧边栏上方未激活时颜色 ##
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_common/components/sidebar/sidebar-nav.styl</font>，修改以下代码。
```css
  cursor: pointer;
  border-bottom: 1px solid transparent;
  font-size: 14px;

  color: #ABEBC6;
  //$sidebar-nav-color;
  &:hover { color: $sidebar-nav-hover-color; }
}
```
这个是未激活状态下的“文章目录”和“站点概览”颜色，如果你有兴趣修改激活状态下颜色的话可以在文章最后提供的方法去找一找。

## 修改文章间隔 ##
我们可以看到Mist格式下的文章是连在一起的，这样对区分每篇文章来说不是很合适。这块网上修改教程几乎找不到，似乎hexo不支持这种方式，只好自己DIY了，这里我们观察首页文章由这几部分组成。
```css
post-expand
	post-header
	post-body
	post-footer
	post-eof
```
可以看到<font color="#000000">post-eof</font>就是我们要的文章之间间隔，在Mist风格下他被隐藏了。
这里修改的时候还要注意，由于你是有背景的，如果只是单单让他显现出来，会导致这里突兀的空一块，所以你这里的设置要和背景颜色统一。
打开<font color="#9b4400">[your-hexo-site]/themes/next/source/css/_custom/custom.styl</font>，添加以下代码
```css
.posts-expand {
  .post-eof {
    display: block;
    margin: $post-eof-margin-top auto $post-eof-margin-bottom;
    width: 100%;
    height: 20px;
	//opacity:0.0;
    background: url(/images/background.jpg);
    background-size: cover;
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-position: center;
    text-align: center;
  }
}
```
这样你文章之间的分割就显得很和谐，这里的<font color="#000000">height: 20px</font>，根据你需要进行调节。

# DIY心得 #
一个博客他的元素很多，不是所有地方都是有教程的，所以这时候你要学会自己找到修改什么地方。
如果你是前端同学，对于css修改肯定得心应手，所以这边是针对没有接触过前端的朋友的。
你鼠标右键想要修改的地方，点击检查，就知道他对应的模块了，一般情况下我们会选择在_custom中修改，这样管控起来方便，如果想要还原直接清空_custom就行。
修改的格式你就可以参考上面的，看到class中是什么，你就.class，有时候你会发现修改没有效果，这是需要加上!important，这样可以强制覆盖原先的样式。
同时你可以在检查模式（f12）对样式进行修改，去掉或者勾上样式前面勾勾，或者修改他的值，这样可以直接观察到效果。

<font color="#b0a4e3">PS1：发现修改没有效果，就只能直接修改next源文件啦。我就是这么修改侧边栏的。</font>
<font color="#b0a4e3">PS2: CSDN的阅读更多可以用检查-去掉hidden勾勾解决，如果想要了解完整教程的话可以下面评论区留言 ‎( *¯ ³¯*)♡ㄘゅ</font>

# 总结 #
至于如何上传GitHub之类的，实在写不动啦，请自行搜索。
大家可以趁机熟悉下Git！下面是推荐了解的部分，等有人看时候在更新吧（逃
接下来你还可以了解：
 <font color="#b0a4e3">1.如何解决github+Hexo的博客多终端同步问题。
 2.如何优雅的在文章中插入图片。
 3.MarkDown语法。
 4.如何修改网站文件后自动更新博客。</font>