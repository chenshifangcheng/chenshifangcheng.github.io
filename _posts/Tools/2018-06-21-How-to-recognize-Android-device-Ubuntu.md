---
layout:     post
title:      解决Ubuntu下adb无法识别android手机的问题
subtitle:   
date:       2018-06-21
author:     Chwyatt
header-img: 
header-mask: 0.3
catalog: false
categories: Tools
tags:
    - adb
    - Ubuntu
---

> 作者@陈式方程 转载请注明出处
> 原文链接：https://www.jianshu.com/p/f39c20703383

在Ubuntu下进行Android开发的时候会遇到手机无法识别的问题，手机插上后执行`adb root`会显示没有权限：
> xxxx@xxxx-PC:~$ adb root
adb: unable to connect for root: insufficient permissions for device: verify udev rules.
See [http://developer.android.com/tools/device.html] for more information.

遇到这种情况需要在Ubuntu设置下。根据以下步骤进行设置后会解决这个问题，我所使用的开发环境是Ubuntu16.04。

**1. 首先插上手机，终端执行`lsusb`，这样便可以查看当前连接电脑的设备**
> xxxx@xxxx-PC:~/.android$ lsusb
Bus 002 Device 002: ID 8087:8000 Intel Corp. 
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 8087:8008 Intel Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
**Bus 003 Device 018: ID 0fce:31ff XiaoMi Mobile** 
Bus 003 Device 003: ID 1c4f:0034 SiGma Micro 
Bus 003 Device 002: ID 413c:2107 Dell Computer Corp. 
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

可以发现，插入的手机的VendorID是**0fce**, ProductID是**31ff**。
如果不能知道哪个是插入的手机，可以拔掉手机后再执行一次`lsusb`，这样对比两次显示的结果就可以找到插入的手机。

**2. 终端执行如下命令**
> xxxx@xxxx-PC:~$ sudo vim /etc/udev/rules.d/53-android.rules

53-android.rules文件可能不存在，那就创建。
注意，这个53-android.rules 文件名字应该是随意命名的，好像数字50，51，52，53等等都可以，我只验证过50和53。  

**3. 增加如下内容到53-android.rules**
> SUBSYSTEM=="usb", SYSFS{idVendor}=="**0fce**", MODE="0666"
> SUBSYSTEM=="usb_device", SYSFS{idVendor}=="**0fce**", MODE=="0666"           
> SUBSYSTEM=="usb", ATTR{idVendor}=="**0fce**", ATTR{idProduct}=="**31ff**", SYMLINK+="android_adb"

注意，`SUBSYSTEM=="usb", SYSFS{idVendor}=="**0fce**", MODE="0666"` 这句是给 ubuntu 7.01 以后的系统识别用的.
而`SUBSYSTEM=="usb_device", SYSFS{idVendor}=="**0fce**", MODE=="0666"` 是给 Ubuntu 7.01之前的系统识别用的，相当于系统兼容。

**4. 接着运行如下命令**
> xxxx@xxxx-PC:/etc/udev/rules.d$ sudo chmod a+rx /etc/udev/rules.d/53-android.rules
xxxx@xxxx-PC:/etc/udev/rules.d$ sudo /etc/init.d/udev restart
[ ok ] Restarting udev (via systemctl): udev.service.

注意，`sudo /etc/init.d/udev restart` 也可以为 `sudo service udev restart    //or restart udev`

**5. 在android sdk的tools目录下运行（这一步很重要，必须要sudo，否则没效果）**
- 首先找到adb命令的路径
> xxxx@xxxx-PC:~$ which adb
/home/xxxx/bin/AndroidSDK/Sdk/platform-tools/adb

- 注意，如果不进入adb路径执行会出现如下错误
> xxxx@xxxx-PC:~$ sudo adb kill-server
[sudo] password for xxxx: 
sudo: adb: command not found

- 进入到adb命令路径后，执行如下命令
> xxxx@xxxx-PC:~/bin/AndroidSDK/Sdk/platform-tools$ sudo ./adb kill-server
> xxxx@xxxx-PC:~/bin/AndroidSDK/Sdk/platform-tools$ sudo ./adb devices
> \* daemon not running. starting it now on port 5037 *
> \* daemon started successfully *
> List of devices attached 

到这一步了，正常情况下应该会有设备显示出来。但结果发现 List of devices attached 下面没有设备出现，这就意味着 adb不识别新的USB 设备，纠结了。

**6. 如果跟我一样悲惨，请执行如下操作**
> cd ~/.android/
> vim adb_usb.ini

注意，如果没有`.android` 和 `adb_usb.ini`，可以自己新建。
另外如果有 `adb_usb.ini`，它的内容一般如下：
```
# ANDROID 3RD PARTY USB VENDOR ID LIST -- DO NOT EDIT.
# USE 'android update adb' TO GENERATE.
# 1 USB VENDOR ID PER LINE
```

**7. 在 `adb_usb.ini` 中添加前面获得的VendorID内容**
> 0x***0fce***

注意，要加十六进制符号**0x**

**8. 保存，关闭，执行如下命令**
> xxxx@xxxx-PC:~/bin/AndroidSDK/Sdk/platform-tools$ sudo ./adb kill-server
xxxx@xxxx-PC:~/bin/AndroidSDK/Sdk/platform-tools$ sudo ./adb devices

此时，List of devices attached 下面会有设备出现了。

至此，结束。