## NanoPi-R2S操作手册

```
欢迎查看 NanoPi-R2S 操作手册。本页旨在为 NanoPi R2S Club ，收集与解答 NanoPi-R2S 相关问题。
如有更好的意见与建议，随时欢迎PR。
或者群内相关发言时直接 @popo
```
### 1、R2S基本介绍

NanoPi R2S是友善电子团队最新推出的一款实现满速率双千兆的、完全开源的IoT应用神器。
NanoPi R2S 使用RK3328 Soc，有两个千兆网络，1G DDR4内存，支持Docker CE, 完全开源，用于企业物联网二次开发，个人定制NAS，家庭物联网网关等。

* 1.1 __开发板介绍__

> Soc: Rockchip RK3328, Quad-core Cortex-A53  
> DDR4内存: 1GB  
> 网口：  
> RTL8211E (10/100/1000M以太网口 x 1)  
> RTL8153B (USB3.0转10/100/1000M以太网口 x 1)  
> USB2.0接口: USB Type-A x1  
> TF卡槽 x 1  
> MicroUSB: 供电和Slave功能  
> Debug Serial Port: 3.3V TTL电平，3Pin 2.54mm间距排针  
> LED灯: LED x 3  
> 按键: Reset按键 x 1 (可用户自定义功能)  
> 开发板尺寸: 55.6 x 52mm  
> 供电: 直流5V/2A  
> 使用温度: 0℃到80℃

> [详细介绍](https://wiki.friendlyarm.com/wiki/index.php/NanoPi_R2S/zh)

---
* 1.2 __灯光按键介绍__

灯光前面板由wan lan绿色指示灯和sys红色指示灯组成，wan lan指示灯在部分固件下不能亮起，sys指示灯在开机状态时，是红色双闪状态。  
如果遇到红灯常亮等特殊状态，则说明系统未启动，或者TF卡不兼容等状态。插入网线连接其他设备后，网口高低电平灯闪烁，则说明链路状态正常。  
若开机后网口灯不正常，或者熄灭，则说明系统未启动或遇到错误。  
reset按键目前未使用。

---
### 2、R2S所需配件

* 2.1 __电源线和电源的选择__

R2S推荐使用5V2A的USB电源，stress烤机下满载电流0.8A。  
建议使用线径粗，接口质量好的USB-A至MicroUSB的数据线，否则概率出现问题。  
*<接口可更改为USB-C接口>*

---
* 2.2 __TF卡的选择__

TF卡的兼容性和稳定性决定着刷机的成功与否和后续使用上的稳定性。

* 2.2.1 TF卡推荐

本群推荐使用朗科与京东联名的Class10/SDHC/A1的TF卡。  
*<京东提供后续保修及换新服务>*

另外大佬推荐SanDisk Industrial闪迪工业卡。MLC颗粒，均衡磨损带来更长寿命，高至85度的工作温度，更适合R2S的工作环境。

* 2.2.2 TF卡刷写建议

刷写软件建议使用win32diskimager和rufus。  
我们建议在刷写原生固件前，进行覆盖格式化或进行快速格式化后填入1GB左右的大文件。  
以及再刷写成功后进行`firstboot -y && reboot now`的操作。

* 2.2.3 TF卡的其他常见问题

Q:为什么启动不了系统？  
A:首先根据上文确定是不是设置错误。比如lan wan互换，比如未自动获取IP地址。建议根据2.2.2的教程重新刷写TF卡。  
*<另外部分TF卡不兼容R2S，甚至兼容友善EXT4不兼容SquashFS>*

Q:为什么TF卡读取不了/卡容量减少/刷卡掉速?  
A:建议使用专业软件删除分区，重新格式化TF卡。或更换读卡器。出现严重问题联系销售商换新。

Q:刷了SquashFS的系统为什么原来的配置还在？  
A:因为TF卡的特性，Upper Layer很难被抹掉。当固件Rootfs大小相同时会出现此类状况。  
使用SSH等工具进入R2S：  
```
umount /overlay && jffs2reset && reboot now
```
[详情链接](https://openwrt.org/docs/guide-user/troubleshooting/failsafe_and_factory_reset)

---
* 2.3 __散热选择__

RK3328在日常使用中，发热量偏大。需要散热来保证不会达到温度墙而掉频。  
在OpenWrt中，温度墙是85℃

* 2.3.1 铝合金外壳

所有铝合金外壳皆出自Telegram群[NanoPi R2S Club](https://t.me/nanopir2sshell)。  
感谢群友们为NanoPi R2S所做的贡献。

* 2.3.1.1 外壳区别

外壳目前分为一代外壳，二代外壳，二代半外壳和三代外壳。


* 2.3.1.2 安装方法

* 2.3.1.3 半导体和风扇

* 2.3.1.4 常见问题

* 2.3.2 风扇外壳

* 2.3.3其他被动散热

---
* 2.4 __USB网卡__

---
* 2.5 __OLED屏幕__

---
* 2.6 __其他外设__

---
### 3、主要功能

—3.1 路由器
——3.1.1 主路由
——3.1.2 单臂路由
——3.1.3 “旁路由”
—3.2 Linux服务器
—3.3 电视盒子（尚未开发）
—3.4 暖手宝（快来补充，我编不下去了）
4、主要固件介绍
—4.1 原生OpenWrt固件
——4.1.1 原生支持OpenWrt固件（开发中）
——4.1.2 Snapshot版原生固件
———4.1.2.1 QiuSimons原生固件
———4.1.2.2 Quintus Chu原生固件
———4.1.2.3 XXXXX原生固件（自己补充）
—4.2 Friendlywrt固件
——4.2.1 友善官方固件
——4.2.2 大神固件
———4.2.2.1 Dayong固件
———4.2.2.2 Chuck固件
———4.2.2.3 Quintus Chu友善固件
———4.2.2.4 QiuSimons固件（原生luci）
———4.2.2.5 SongChenwen固件
———4.2.2.6 RT固件
———4.2.2.7 XXXXX固件（自己补充）
—4.3 Armbian固件
—4.4 Linux发行版固件
5、OpenWrt使用基本介绍
