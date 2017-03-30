---
title: 'hexo搭建github个人主页'
date: 2017-03-29 13:54:15
tags: [hexo,笔记]
categories: 笔记
---

由于最近比较清闲，自己看了点东西，看完后回头想想，好像什么也没记住。一味的只是看终究还是不行，看完一遍只留下一点浅浅的印象，这样不行，所以我就想着也弄个个人主页吧，没事记点笔记，写写工作，谈谈生活，美滋滋。

用hexo搭建的时候，应该是没有接触过这面，git也一直用可视化工具，经常弄了半个小时一个小时才完成想要的东西，感觉自己像个白痴。真是年纪大了脑子也不灵光了，满满的危机感。废话比较多了，直接上搭建步骤，免得以后又忘了。。。

## 配置环境
---

hexo是一款基于Node.js的静态博客框架，so...

#### 安装Node

[下载地址](https://nodejs.org/en/download/)
一路安装就行，注意path要勾上，默认就是打勾的，所以基本没有什么问题。

#### 安装Git

[下载地址](http://git-scm.com/download/)

#### 安装Hexo

选个你喜欢的位置，比如E://hexo
```
    $ cd e://hexo
    $ npm install hexo-cli -g 
    $ hexo init blog
    $ cd blog
    $ npm install
    $ hexo g 
    $ hexo s 
```
上面的命令执行完成之后就可以打开浏览器查看http://localhost:4000/
    
** 注意：** ctrl+c可以关掉本地服务，一般情况是不需要关掉的，你直接改你想要的东西改完，刷新一下网页就ok了

几个常用的命令：
 1.hexo g(hexo generate) 生成静态文件，会在当前目录下生成一个叫做public的文件夹
 2.hexo s(hexo server) 启动本地服务，用来预览博客
 3.hexo d(hexo deploy) 部署到远端(比如github)
 4.hexo n "new"(hexo new "new") 新建文章

## Hexo主题相关
---

港真，hexo默认的主题真的不怎么好看，作为一个有个性的人，怎么可以用官方demo主题，果断研究如果安装主题。找了好久主题，最后还是用了最多人用的next，别说我每个性，我不用官方的就已经是很有个性了。
附上[Next官网](http://theme-next.iissnan.com/)
#### 安装主题

1.安装Next
```
    $ hexo clean
    $ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

2.使用主题

打开根目录_config.yml配置文件中的theme属性，将其设置为next。
```
# Extensions
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
theme: next
```

3.查看主题

```
    $ hexo g # 生成
    $ hexo s # 启动本地web服务器   
```

#### 个性化主题

好吧，所谓的个性化。在next官网基本都有api，很完整。

1.首先是站点配置文件，就是你根目录下的_config.yml
```
# Site 网站
title: 慎思而笃行                //标题
subtitle: 终极奥义·复制粘贴      //副标题
description: 是你。你会像他一样  //描述就是个性签名（一个微笑）
author: wenzi                  //作者
language: zh-Hans              //网站语言
timezone:                      //时区默认为电脑的时区
```
2.主题配置文件,主题next下的_config.yml
next支持三种主题，打开相应的注释就可以了，还是挺方便的
```
# Schemes
# scheme: Muse
scheme: Mist
# scheme: Pisces
```
网站图标，最好用ico格式，百度ioc，一大堆转换网站

![](http://onifre9rp.bkt.clouddn.com/ico.png)
```
# Put your favicon.ico into `hexo-site/source/` directory.
favicon: images/favicon.ico
```
菜单，出了主题自带的也可以自己创建
```
menu:
  home: /
  categories: /categories
  archives: /archives
  about: /about
  tags: /tags

menu_icons:
  enable: false
  #KeyMapsToMenuItemKey: NameOfTheIconFromFontAwesome
  home: home
  about: user
  categories: th
  schedule: calendar
  tags: tags
  archives: archive
  sitemap: sitemap
  commonweal: heartbeat
```
外链网站
```
social:
  GitHub: https://github.com/wenzi5519

social_icons:
  enable: true
  GitHub: github
```
打赏功能
```
donate: 
  enable: true
  text: Enjoy it ? Donate me !  欣赏此文？求鼓励，求支持！
  alipay: 
  wechat: 
```
*更具体的可以参考官网

## Git相关
---

git作为一名开发人员，应该都用过，不过应该也有像我这样的，用确实用了，但是还是不会用，实力尴尬。就这个项目，写一点我部署项目的时候用到的git命令好了

#### 首先是hexo的部署方法，不过我没有成功。
在根目录的，站点配置文件里面
```
deploy:
  type: git
  repo: https://github.com/wenzi5519/wenzi5519.github.io.git
  branch: master
```
然后执行
```
$ npm install hexo-deloyer-git --save
$ hexo d
```
就可以部署完成了。

#### 使用Git命令部署

首先配置git
```
$ git config --global user.name "username"
$ git config --global user.email "username@example.com"
```
检测电脑有没有ssh
```
ls -al ~/.ssh
```
如果没有生成新的ssh Key,一路回车
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
然后看下ssh-agent是否可运行
```
$ ssh-agent -s
```
添加ssh key 
```
$ ssh-add ~/.ssh/id_rsa
```
复制key
```
clip < ~/.ssh/id_rsa.pub
```
然后在github右上角点头像-->setting-->Personal settings-->ssh key创建
测试一下试试
```
$ ssh -T git@github.com
```
接下来就是push到github了
```
$ git add .
$ git commit -m “change description”
$ git push origin hexo
```
*这里新建一个hexo分支是为了方便可以在不同的电脑上写博客

** 已有环境 **
已经在这台电脑上写过博客了，那么可以在已有的工作目录下同步之前写的博客。
```
$ git pull
```
然后就可以写新的博客了，写好之后 hexo g -d 部署到远端
接着就是push到github
** 新的环境 **
新的电脑，我们需要先把项目下载到本地，然后在hexo初始化
```
$ git clone https://github.com/wenzi5519/wenzi5519.github.io.git
$ cd wenzi5519.github.io
$ npm install hexo
$ npm install hexo-deployer-git --save
```
然后就是和已有环境一样的操作了，部署推送。