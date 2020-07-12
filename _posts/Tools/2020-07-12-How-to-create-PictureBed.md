---
layout:     post
title:      Github + PicGo + jsDelivr 创建稳定、免费图床
subtitle:   
date:       2020-07-12 11:00:00 +0800
author:     Chwyatt
header-img: 
header-mask: 0.0
catalog: true
categories: Tools
tags:
    - PicGo
    - PictureBed
---

## 什么是图床

简单讲，图床就是一个你在网络上存放图片的地方。

很多公司都提供了这样一个地方让你存放图片，比如微博，简书，七牛云等，但这些毕竟是公司资源，说不定哪天就不能访问了。

所以，自己构建一个图床就显得比较重要了。

## 创建图床的工具选择

GitHub + PicGo + jsDelivr 的方式是目前比较主流的方式。

1. GitHub 用来存放图片，目前投入微软怀抱，后台硬，不会随便倒闭
2. [PicGo ](https://github.com/Molunerfinn/PicGo) 是一个用于快速上传图片并获取图片 URL 链接的工具，通过该链接访问网络图片资源
3. [jsDelivr](https://www.jsdelivr.com/) 是用于开源项目的免费公共CDN(Content Delivery Network，即内容分发网络)，用来加速GitHub的访问速度

## 如何创建图床

1. 在GitHub上新建一个仓库

   ![image-20200711165320294](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200711223511.png)

2. 在GitHub中生成一个Token（[为什么要此Token？](https://www.r-bloggers.com/a-better-way-to-manage-your-github-personal-access-tokens/)[^1]）

   * 进入个人设置 `Settings -> Developer settings -> Personal access tokens -> Generate new token`

     ![image-20200711170501440](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200711223553.png)

   * 在`Note`中填好描述，勾选`repo`，然后点击`Generate token`生成一个Token

     ![image-20200711170931613](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712105006.png)

   * 注意这个Token只会显示一次，所以，需要保存下来

     ![image-20200711171922655](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712110314.png)

3. 配置PicGo

   * 前往PicGo的GitHub仓库下载，[传送门](https://github.com/Molunerfinn/PicGo/releases)。如果下载较慢，可以复制地址到迅雷下载，非常快！

   * 下载后点击安装，弹出配置窗口

     ![image-20200711210103011](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712110330.png)

     - 设定仓库名：按照 `用户名/图床仓库名` 的格式填写
     - 设定分支名：`master`
     - 设定Token：填写之前在GitHub生成的Token
     - 指定存储路径：填写想要储存的路径，如 `BlogImg/`，这样就会在仓库下创建一个名为`BlogImg` 的文件夹，图片将会储存在此文件夹中
     - 设定自定义域名：图片上传后，PicGo 会按照 `自定义域名+储存路径+上传的图片名` 的方式生成访问链接，并放到粘贴板上

   * 如下是我的配置

     ![image-20200711214441130](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712110359.png)

     - 设定仓库名：我的GitHub用户名为`chenshifangcheng`，图床仓库名为 `PictureBed`，则填写为 `chenshifangcheng/PictureBed`

     - 指定存储路径：**注意**，不能写成`BlogImg`，即少掉 `/`，否则无法生成文件夹，比如：

       https://raw.githubusercontent.com/chenshifangcheng/PictureBed/master/BlogImgimage-20200711165320294.png

       而不是：

       https://raw.githubusercontent.com/chenshifangcheng/PictureBed/master/BlogImg/image-20200711165320294.png

     - 设定自定义域名：我的自定义域名设置为 `https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed`，

       因为我们要使用 jsDelivr 加速访问，所以自定义域名可以设置为 `https://cdn.jsdelivr.net/gh/用户名/图床仓库名 `，上传完毕后，就可以通过 `https://cdn.jsdelivr.net/gh/用户名/图床仓库名/储存路径/上传的图片名` 加速访问我们的图片了。存储路径为`BlogImg/`,  所以通过`https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/上传的图片名` 来加速访问。

       **设定自定义域名前后链接对比：**

       https://raw.githubusercontent.com/chenshifangcheng/PictureBed/master/BlogImg/image-20200711165320294.png

       vs

       https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/image-20200711165320294.png

   * 打开`时间戳重命名`

     上传图片时，如果多次上传同一张图片的话，文件名会一样，导致文件已存在而报错，在上传图片时使用时间戳不会因为文件名重复而报错。

     ![image-20200711212916711](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712110404.png)

     **文件名前后对比：**

     https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/image-20200711165320294.png

     vs

     https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200711214309.png

4. 至此，就可以通过PicGo的`上传区`上传本地图片到GitHub图床了

   ![image-20200712110725619](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712110725.png)

   可以直接拖拽图片、点击上传、截图到剪贴板上直接上传。非常方便快捷！

---



[^1]: Access Token也是GitHub的一种认证方式，在PicGo的配置中需要用到
