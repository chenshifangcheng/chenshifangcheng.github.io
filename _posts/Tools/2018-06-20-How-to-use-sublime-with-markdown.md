---
layout:     post
title:      Sublime写Markdown
subtitle:   Sublime的安装、使用；结合MarkdownEditing、MarkdownPreview
date:       2018-06-20
author:     Chwyatt
header-img: img/postimg/post-1-sublime-text-3-660x330-bg.jpg
header-mask: 0.0
catalog: true
categories: Tools
tags:
    - Sublime
    - Markdown
---

> 本文环境基于 Ubuntu16.04 + Sublime Text 3。

## Sublime安装

Sublime的安装包括`apt安装`，`tar包安装`以及`deb安装`。

### apt安装

首先去[官网](https://www.sublimetext.com/)下载页面，选择[apt](https://www.sublimetext.com/docs/3/linux_repositories.html#apt)，根据指示敲命令就行。这种安装是最推荐的方式。然而，由于**网络环境等**因素，安装可能会失败，这时可以考虑以下2种安装方式。

### tar包安装

在[Download页面](https://www.sublimetext.com/3)选择64bit或32bit tarball。目前大多数电脑配置都上64位了，我的环境也如此，所以我下载了64位的安装包。

下载下来是一个压缩包`sublime_text_3_build_3176_x64.tar.bz2`，解压后为`sublime_text_3`，我把它放在路径`/home/<user>/bin/sublime_text_3`下，目录结构如下：

![目录结构](/img/postimg/post-1-sublime-installed-folder-directory-structure.jpg)

直接双击`sublime_text`就可以运行Sublime了。然而每次都要进入到这个路径双击太麻烦，可以考虑创建应用快捷图标desktop，方法如下：

1. 进入ubuntu系统的`/usr/share/applications`，这个文件夹下面存放着系统中所有的快捷图标，我们也要在这里创建一个`sublime_text.desktop`，这样就可以点击图标启动软件。
2. 将解压后的Sublime安装包里的`sublime_text.desktop`拷贝到`/usr/share/applications`。
3. `sudo vim /usr/share/applications/sublime_text.desktop`来修改启动路径， 如`Exec=/home/<user>/bin/sublime_text_3/sublime_text %F`，注意需要可执行文件**完整路径**，用户主目录不可使用`~`代替。
4. 从`/home/<user>/bin/sublime_text_3/Icon`中选择一个合适的icon，然后修改图标路径`Icon=/home/<user>/bin/sublime_text_3/Icon/32x32/sublime-text.png`，注意**图片后缀要加上**。

至此，就可以从Applications里直接启动Sublime了。

关于创建应用快捷图标有2种方式：

- 模仿别的`.desktop`文件，如果不熟悉的话，成功的概率比较低。
- 使用`gnome-panel`，这个工具能够帮助我们创建快捷图标，确保功能上的正常，但是图标需要自己修改以下路径。
    - 安装gnome-panel，`sudo apt-get install gnome-panel`
    - 使用gnome-panel， `gnome-desktop-item-edit [选线m] [路径] [指令]`
    - `gnome-desktop-item-edit /usr/share/applications/ --create-new`这条命令就是在`/usr/share/applications/`下创建一个新的图标，会弹出对话框，填写name, excu, comment 等信息。创建成功了，可以在该目录下看到，而且点击的时候就能打开软件。

### deb安装

上网搜索下载deb包，如`sublime-text_build-3083_amd64.deb`，然后执行`sudo dpkg -i sublime-text_build-3083_amd64.deb`即可。

### Sublime使用

分享2个不错的Sublime使用文档，[Sublime Text 使用手册](https://www.w3cschool.cn/sublimetext/)和[Sublime Text 3 中文文档](https://www.w3cschool.cn/sublimetext3/)。

### Sublime说明

安装路径`/home/<user>/bin/sublime_text_3/Packages`下都是Sublime默认安装插件，如`C++.sublime-package`、`Git Formats.sublime-package`。

路径`/home/<user>/.config/sublime-text-3/Installed Packages`下都是下载的插件，如`Package Control.sublime-package`、`MarkdownEditing.sublime-package`、`MarkdownPreview.sublime-package`，
其后缀名都是`.sublime-package`。

路径`/home/<user>/.config/sublime-text-3/Packages/User`下都是用户的一些设置，包括Settings，Keymap等，如`Preferences.sublime-settings`、`Package Control.sublime-settings`、`Default (Linux).sublime-keymap`等，
其中后缀名包括`.sublime-settings`、`.sublime-keymap`等。


## Package Control安装

Sublime最强大的地方莫过于插件功能，这些插件都是由Sublime Text的package manager来管理。我们需要安装Package Control（自己本身也是一个插件）来管理这些插件。详情见[官网](https://packagecontrol.io/)。
Package Control安装有`Simple`和`Manual`2种方式。

### Simple方式

进入[安装界面](https://packagecontrol.io/installation)，拷贝对应code，通过快捷键<kbd>ctrl+`</kbd>来调出Sublime控制台, 将对应code粘贴到console，敲回车，等待一会儿status bar会输出安装成功与否信息。
由于网络环境等原因，这种方式有可能会安装不成功，这时可以考虑手动安装(Manual方式)。

### Manual方式

进入[安装界面](https://packagecontrol.io/installation)，点击下载`Package Control.sublime-package`，下载下来为`Package Control.sublime-package`,解压后为`Package Control.sublime-package_FILES`。
但是这边**不需要解压**，直接将`Package Control.sublime-package`拷贝到`/home/<user>/.config/sublime-text-3/Installed Packages`目录下，重启Sublime，就可以使用Package Control了。

### Package Control使用

通过快捷键<kbd>Ctrl+Shift+P</kbd>或点击`Preferences->Package Control`（安装Package Control成功后才会有这个menu）调出命令面板，输入关键字，可以看到诸如
`Package Control: install package`、`Package Control: remove package`等功能，如下图：

![](/img/postimg/post-1-package-control-function-list.jpg)

## MarkdownEditing插件

想要通过Sublime+Markdown来写文章，安装相应的插件才会手顺，写起Markdown才会有感觉。目前Sublime Text 3已经支持高亮显示*Standard Markdown*和*MultiMarkdown*语法，如果不需要预览功能或是GFM(GitHub Flavored Markdown)支持，可以直接使用。然而我恰好需要预览功能及GFM支持。

MarkdownEditing插件使得在Sublime写Markdown变成一件很cool的事情。当你使用Markdown语法书写时，会高亮显示语法，会部分WYSIWYG（What you see is what you get）。当然，MarkdownPreview插件会使得你预览所有内容，将在后面介绍。

### MarkdownEditing安装

可以通过Package Control安装或手动安装，推荐用Package Control，**会自动更新**（Package Control automatically download the package and keeps it up-to-date. Manual installation is required if you need to tweak the code.）。

1. 通过快捷键<kbd>Ctrl+Shift+P</kbd>或点击`Preferences->Package Control`调出命令面板，输入`Package Control: install package`，敲回车，等一会儿，Sublime会从Package Manager获取packages list，即请求远程插件仓库的索引。接着会弹出插件安装面板。
2. 在插件安装面板输入关键字markdown，选择`MarkdownEditing`并点击安装即可。
3. 安装完毕，在目录`/home/<user>/.config/sublime-text-3/Installed Packages`中多了`MarkdownEditing.sublime-package`

### MarkdownEditing使用

最权威的使用方法当然是看官方文档*README.md*了，点击`Preferences->Package Settings->Markdown Editing->README`即可。

下面是我常用到的一些特性：

- 安装后针对 md\mdown\txt 格式文件启用插件。可在其user的`sublime-settings`中覆盖设置。
- 支持3种Markdown格式: Standard Markdown, GitHub flavored Markdown, MultiMarkdown。默认为GFM，可在`View->Syntax->MarkdownEditing`中来修改默认值。
- 快捷键操作
    - 插入图片：Shift + Win + K
    - 插入**inline**链接：Ctrl + Alt + V
    - 插入**reference**链接：Ctrl + Alt + R
- Code snippet（通过<kbd>Ctrl+Shift+P</kbd>>输入Snippet也可以调出）
    - 输入`mdi + tab`会自动插入图片标记，`![Alt text](/path/to/img.jpg "Optional title")`
    - 输入`mdl + tab`会自动生成链接标记，`[](link)`
- 集成了除了主题外`Knockdown`的所有特性，包括`Sublime Markdown Extended`、`SmartMarkdown`、`MarkdownTOC`。

## MarkdownPreview插件

MarkdownPreview是用来在浏览器里预览markdown文档的，内嵌Python Markdown解析器（offline），也可以使用GitHub Markdown API（online）。

### MarkdownPreview安装

1. 通过快捷键<kbd>Ctrl+Shift+P</kbd>或点击`Preferences->Package Control`调出命令面板，输入`Package Control: install package`，敲回车。
2. 在插件安装面板输入关键字markdown，选择`MarkdownPreview`并点击安装即可。
3. 安装完毕，在目录`/home/<user>/.config/sublime-text-3/Installed Packages`中多了`MarkdownPreview.sublime-package`。

### MarkdownPreview使用

最权威的使用方法请看[官方文档](https://facelessuser.github.io/MarkdownPreview/)。

通过快捷键<kbd>Ctrl+Shift+P</kbd>或点击`Preferences->Package Control`调出命令面板，输入然后选中`Markdown Preview: Preview in Browser`，即可预览当前markdown文档。

下面是我常用到的一些特性：

- 由于markdown preview默认没有快捷键，每次预览都需要调出命令面板，这样显得不cool，可以添加快捷键。
    - 点击`Preferences->Keybindings`，在User map里添加 `{ "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"}  }`，这样通过快捷键<kbd>Alt+m</kbd>即可预览。"parser":"markdown"也可设置为"parser":"github"，改为使用Github在线API解析markdown。
    - 如果要选择解析器，在User map里添加 `{ "keys": ["alt+m"], "command": "markdown_preview_select", "args": {"target": "browser"} }`。
- 如果要控制哪个浏览器来预览，则可在Settings里修改默认值
  ```
  /*
    Sets the default opener for HTML files

    default - Use the system default HTML viewer
    other - Set a full path to any executable. ex: /Applications/Google Chrome Canary.app or /Applications/Firefox.app
    */
    "browser": "default",
  ```
- 通过`LiveReload`插件来实时更新预览。

## 其他

还有一款预览插件叫`OmniMarkupPreviewer`，这款插件功能很强大，大家可以试试。

另外，很不幸的是在Ubuntu下Sublime无法输入中文，也是很悲凉，解决办法[看这里](https://www.jianshu.com/p/bf05fb3a4709)。
