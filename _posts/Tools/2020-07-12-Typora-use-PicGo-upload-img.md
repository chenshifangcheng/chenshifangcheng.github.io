---
layout:     post
title:      Typora如何通过PicGo自动上传图片到图床
subtitle:   包括本地图片，网络图片以及截图
date:       2020-07-12 12:00:00 +0800
author:     Chwyatt
header-img: 
header-mask: 0.0
catalog: true
categories: Tools
tags:
    - PicGo
    - Typora
---

在上一篇博文中，我们通过 `GitHub+PicGo+jsDelivr` 构建了一个非常稳定的图床。

在我们写博客的过程中，经常需要贴很多图片，不管是本地、网络、还是截图；一般情况是先将图片上传到图床，然后将链接粘贴到编辑器中，比如我之前使用 [Sublime写Markdown](https://chenshifangcheng.github.io/tools/2018/06/20/How-to-use-sublime-with-markdown/) 或 Typora 时就需要这样，但这样很不方便！

然而，好消息是，Typora 编辑器在新版本上(≥ 0.9.9.32 on macOS or 0.9.84 on Windows / Linux) 增加了一个 `上传图片` 的功能，通过第三方app或脚本在插入图片时将图片上传到网络，然后文档中会直接插入图片的图床链接。

接下来介绍Typora如何通过PicGo来自动/手动上传图片到我的图床

## 前提

根据我的上一篇博文《Github + PicGo + jsDelivr 创建稳定、免费图床》来创建一个GitHub图床

## 设置Typora插入图片时自动上传图片

![](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712114705.png)

1. 进入“文件 -> 偏好设置 -> 图片”，在“插入图片时”区域，下拉框选择“上传图片”，勾选如下三项（勾选即开启自动上传）：

   * 对本地位置的图片应用上述规则

     * 即当插入本地图片（包括从菜单中插入，截图，copy & paste，drag & drop）时，会自动上传该图片

     * 插入截图时，首先会将截图自动保存在本地，然后再自动上传

       本地路径：C:\Users\\<User>\AppData\Roaming\Typora\typora-user-images

   * 对网络位置的图片应用上述规则

     *  重新上传网络图片到你的图床（我试验没成功）

   * 允许根据 YAML 设置自动上传图片

     * 通过读取 YAML 配置来决定是否自动上传图片

     * 在勾选该选项的前提下，在 Markdown 文件的 YAML front matter 中添加如下配置，则Typora会在你插入图片时自动上传图片：

       `typora-copy-images-to: upload`

2. 在“上传服务设定”区域：

   * 上传服务：下拉框选择 “PicGo(app)”，即通过PicGo app来上传图片

   * PicGo路径：选择PicGo的安装路径

   * 下载PicGo(app)：如果本地没有安装，点击该按钮时会引导到PicGo安装页面

   * 验证图片上传选项：点击该按钮来验证当前配置是否能成功上传图片到图床

     * 如果成功的话，会将Typora的两个logo上传到你的图床，我的上传到如下路径了

       https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712095247.png

       https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712095248.png

       ![](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712121151.png)

## 选择手动上传图片

1. 如下场景需手动上传：
   * 自动上传没有成功
   * 未开启自动上传图片的功能，即没有勾选“插入图片时”区域的选项

2. 图片拖入、上传或粘贴到 Typora后，右键图片会看到一个“上传图片”的选项，点击上传即可

   ![image-20200712123354421](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712124010.png)

3. 如果文件中有许多本地图片没有上传，则可以通过如下菜单一次性上传

   `菜单→ 格式 → 图像 → 上传所有本地图片`

   ![image-20200712124000782](https://cdn.jsdelivr.net/gh/chenshifangcheng/PictureBed/BlogImg/20200712124000.png)

## 参考链接

[https://support.typora.io/Upload-Image/](https://support.typora.io/Upload-Image/)