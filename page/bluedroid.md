---
layout: page
title: "Bluedroid与BluZ，蓝牙测试方法的变动（基于bludroid和BlueZ的对比）"
description: ""
---
{% include JB/setup %}

https://my.oschina.net/u/994235/blog/300406
```
Bluedroid与BluZ，蓝牙测试方法的变动（基于bludroid和BlueZ的对比）
 收藏
sflfqx  
发表于 3年前  阅读 2410 收藏 1 点赞 1 评论 0
  

android4.2以后，增加了bludroid，在做测试时，会发现与之前的bluez的测试，有着较大的变动。下面罗列一些bluedroid的不同点，以及之前bluez的测试命令验证（该部分是用bluez做的测试，针对bluedroid的测试后续会补充）。



对蓝牙栈bluedroid的测试变动：

1. 已经没有 bttest 的测试工具，也就说没有bt_enable(), bt_disable()的功能来打开和关闭蓝牙

2. 一些Bluedroid中没有的测试功能。

hcitool， hciconfig，rctest， l2test，Sdptool。而蓝牙 FTM 的测试工具还是有的。

3. bluedroid中所有的log均可以在log cat中查看，不像之前的bluez，一部分在log cat中，一部分在 kernel log中。并且，bluedroid对不同的profile和层次的log进行了分类（对BTM, HCI, L2CAP, RFCOMM, OBEX），在bt_stack.conf中可以进行配置 ，并想android中的Log一样，可以对输出的log做输出等级的调整（0-6来表示）。





对bluez的测试命令：

网上已经有很多资料了，找到一篇不错的资料：

原文地址：http://blog.chinaunix.net/uid-25909619-id-3554423.html      这里感谢作者的分享！

由于原文作者虽说明了蓝牙是用的BlueZ的蓝牙栈，但未注明所使用的手机版本和型号，为了保险起见，下面是针对里面的命令做下测试。

我的环境：Android 4.1.1 蓝牙栈：BlueZ



命令行测试蓝牙

1. 命令行控制蓝牙开关
adb shell
cd /data/data/com.android.providers.settings/databases
sqlite3 settings.db
select * from secure where name="bluetooth_on"; （查看是否打开）
update secure set value=1 where name="bluetooth_on";  (这里的value=1是打开，0是关闭)
select * from secure where name="bluetooth_on"; （确认是否更改成功）

reboot <重启手机生效>



2. 命令行操作蓝牙
Android原生包括高通QRD用的是blueZ的蓝牙协议栈，有提供两个工具：hciconfig和hcitool用于调试蓝牙，开始调试前首先需要将这些工具Push到手机上：
adb remount
adb push hciconfig /system/xbin
adb push hcitool /system/xbin
adb shell
chmod -R 777 /system/xbin
要注意的是，这些工具只适用于blueZ，象MTK用的是bluetoothangel就不适用了

常用的一些命令：
hciconfig -a （查看蓝牙地址，芯片状态等等）
hcitool scan （进行蓝牙搜索，并列出搜索到的设备名称和设备地址）
hciconfig hciX piscan （开启Inquiry Scan和Page Scan，使手机处于可被搜索和可连接状态）
可以使用hciconfig --help以及hcitool --help来查询其它的功能，尤其要提的是hcitool cmd这个命令，通过这个命令可以发送任何的HCI Command，大部分蓝牙功能都可以通过发送HCI Command来实现，具体HCI Command格式可以查询蓝牙Spec

进入测试模式的命令：
hcitool cmd 0x06 0x0003 （Enter Test Mode）
hcitool cmd 0x03 0x0005 0x02 0x00 0x02 （Auto Accept All Connections）
hcitool cmd 0x03 0x001A 0x03 （Page Inquiry Scans）
hcitool cmd 0x03 0x0020 0x00 （Disable Authentication）
hcitool cmd 0x03 0x0022 0x00 （Disable Encryption）

Qualcomm bt test :

the follow commands to bring up bt through adb shell:

 #echo 1 > /sys/class/rfkill/rfkill0/state

#hci_qcomm_init -vvv -e

#hciattach /dev/ttyHS0 qualcomm-ibs 3000000

#hciconfig hci0 up

#hcitool scan

The follow commands are used to enter test mode.

 #bttest disable

#bttest enable

#bttest is_enabled

#bttest enable_dut_mode




测试后，均可正常使用。对bluedroid的实际测试，之后会回来补充。



bluedroid支持的测试工具：

btnvtool  btool     btrftest  btrftestd bttest
```