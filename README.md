## NanoPi-R2S操作手册

```
欢迎查看 NanoPi-R2S 操作手册。本页旨在为 NanoPi R2S Club ，收集与解答 NanoPi-R2S 相关问题。
如有更好的意见与建议，随时欢迎PR。
或者群内相关发言时直接 @popo
```
### 1、R2S基本介绍

NanoPi R2S是友善电子团队最新推出的一款实现满速率双千兆的、完全开源的IoT应用神器。
NanoPi R2S 使用RK3328 Soc，有两个千兆网络，1G DDR4内存，支持Docker CE, 完全开源，用于企业物联网二次开发，个人定制NAS，家庭物联网网关等。

* __1.1 开发板介绍__

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
* __1.2 灯光按键介绍__

灯光前面板由wan lan绿色指示灯和sys红色指示灯组成，wan lan指示灯在部分固件下不能亮起，sys指示灯在开机状态时，是红色双闪状态。  
如果遇到红灯常亮等特殊状态，则说明系统未启动，或者TF卡不兼容等状态。插入网线连接其他设备后，网口高低电平灯闪烁，则说明链路状态正常。  
若开机后网口灯不正常，或者熄灭，则说明系统未启动或遇到错误。  
reset按键目前未使用。

---
### 2、R2S所需配件

* __2.1 电源线和电源的选择__

R2S推荐使用5V2A的USB电源，stress烤机下满载电流0.8A。  
建议使用线径粗，接口质量好的USB-A至MicroUSB的数据线，否则概率出现问题。  
*<接口可更改为USB-C接口>*

---
* __2.2 TF卡的选择__

TF卡的兼容性和稳定性决定着刷机的成功与否和后续使用上的稳定性。

* 2.2.1 TF卡推荐

本群推荐使用朗科与京东联名的Class10/SDHC/A1的TF卡。  
*<京东提供后续保修及换新服务>*

另外大佬推荐SanDisk Industrial闪迪工业卡。MLC颗粒，均衡磨损带来更长寿命，高至85度的工作温度，同样适合R2S的工作环境。

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
jffs2reset && reboot now
```
[详情链接](https://openwrt.org/docs/guide-user/troubleshooting/failsafe_and_factory_reset)

---
* __2.3 散热选择__

RK3328在日常使用中，发热量偏大。需要散热来保证不会达到温度墙而掉频。  
RK3328的温度墙是85℃

* 2.3.1 铝合金外壳

所有铝合金外壳皆出自Telegram群[NanoPi R2S Club](https://t.me/nanopir2sshell)。  
感谢群友们为NanoPi R2S所做的贡献。

__购买外壳可加入Telegram群[NanoPi R2S Club](https://t.me/nanopir2sshell)，看群置顶。或[淘宝链接](https://taoquan.taobao.com/coupon/unify_apply.htm?sellerId=1613492699&activityId=156945e116dc477780f99dedf3ed91b3),领券下单更优惠。__

* 2.3.1.1 外壳区别

外壳目前分为一代外壳，二代外壳，二代半外壳和三代外壳。

> 一代外壳：为铝合金构造，外表做氧化发黑处理，黑色高级纹理搭配顶部散热鳍片。TF卡和USB做开孔处理，其中限量版首批300件有特殊logo。

> 二代外壳：为铝合金构造，外表做磨砂氧化处理，保持铝合金原色，去掉顶部鳍片和TF卡槽，整体更协调，颜值在线。

> 二代半外壳：为铝合金构造，外表做磨砂氧化处理，保持铝合金原色，相比于二代加入重新设计高级TF卡槽，插卡取卡更便捷。

> 三代外壳：在二代半外壳的基础上，加入0.91英寸OLED屏幕(详见2.5条目)，简单安装方法，信息获取更高效。

* 2.3.1.2 安装方法

暂略。如有安装困难或温度过高，不明安装方法可加入Telegram群[NanoPi R2S Club](https://t.me/nanopir2sshell)

* 2.3.1.3 半导体和风扇

半导体制冷片采用珀耳帖效应，安装在铝壳上可显著降低Soc温度。  
注意做好热面散热和温控，否则会损坏制冷片和凝水导致R2S主板进液。  
外壳外加装风扇会进一步降低Soc的温度。
交流讨论可加入Telegram群[NanoPi R2S Club](https://t.me/nanopir2sshell)

* 2.3.1.4 常见问题

Q:铝合金外壳能降低到多少度？  
A:这个要根据室温和空气流通速度来决定。铝合金外壳的最主要目的是满负载情况下能不碰温度墙，稳定1.512GHz的Soc主频不掉频，全力输出。以以往的经验来说，待机状态下Soc温度是室温+20以内，弱电箱内+25以内。如果过高考虑下安装问题和硅脂接触面积。

Q:绝缘胶带需要贴吗？  
A:可贴可不贴。

Q:硅脂溢出到主板上怎么办？  
A:大部分硅脂不导电，没影响。

Q:板子安装不进去怎么办？  
A:R2S的主板是从大主板上裁切下来的，会有一定的公差。用指甲刀锉一下边缘毛边。

Q:硅脂垫是干什么的？可以不用吗？  
A:硅脂垫是贴主板后侧到铝壳底盖的，主要充当散热器弹簧的用途，给Soc和铝柱一个额外的压力，帮助更好散热。

* 2.3.2 风扇外壳

无论是亚克力定制的外壳或者原装散热片自带的风扇，还是原装黄色壳子安装风扇，都不能在满载下不碰温度墙，故不推荐。但是可以降低待机温度。

* 2.3.3其他被动散热

无论是原装散热片还是加装其他散热片，都不能在满载下不碰温度墙，特别是在原装黄壳子内，故不推荐。但是可以降低待机温度。

---
* __2.4 USB网卡__

FriendlyWrt可编译进811ac等USB网卡，设置为N模式，5GHz。强制频宽40MHz，信道40。重启后，可以跑到100Mbps左右。  
原生OpenWrt可选用MT7601等网卡，具体参照固件支持。

---
* __2.5 OLED屏幕__

目前OLED屏幕分为0.96寸狼牙山五壮士版，和三代铝壳自带0.91寸屏幕。

0.91寸屏幕可使用natelol大佬的[luci-app-oled](https://github.com/NateLol/luci-app-oled)来选择显示内容(部分固件有编译进)

---
* __2.6 其他外设__

狼牙山五壮士等特殊外设。

---
### 3、主要功能

* __3.1 路由器__

双千兆网口的开发板做路由器很合适。

* 3.1.1 主路由

主要利用方式。

* 3.1.2 单臂路由

接VLAN交换机，单线复用。参考IEEE 802.1q协议等资料。

* 3.1.3 “旁路由”

极不推荐。

---
* __3.2 Linux服务器和Docker容器__

支持Armbian稳定版和Ubuntu等版本，绝大多数系统下支持docker，如Armbian的docker OpenWrt，OpenWrt下Docker Awtrix等等操作。  
详见4.3和4.4条目。

* __3.3 电视盒子__

*<尚未开发>*

* __3.4 暖手宝__

*<皮一下很开心。————Nick Bot>*

---
### 4、主要固件介绍

* __4.1 原生OpenWrt固件__

* 4.1.1 原生支持OpenWrt固件

[开发中](https://github.com/openwrt/openwrt/pull/2945)

* 4.1.2 Snapshot版原生固件

源于开源社区的努力。

* 4.1.2.1 QiuSimons原生固件

__自用固件__ ,分docker版和无docker版。  
目前支持OLED和部分USB网卡，具体内容详见readme。  
已托管给ctcgfw  
地址：https://github.com/project-openwrt/R2S-OpenWrt

* 4.1.2.2 Quintus Chu原生固件

__自用固件__ ,分OpenWrt版和FriendlyWrt版，FriendlyWrt版详见4.2.2.3条目。  
固件分为slim版和full版，目前支持OLED和部分USB网卡，具体内容详见readme和releases。  
地址：https://github.com/quintus-lab/Openwrt-R2S

* 4.1.2.3 Kane Green的原生固件

__自用固件__ ,高大全固件，发布于Actions。  
具体内容详见readme。  
地址：https://github.com/KaneGreen/R2S-OpenWrt

* 4.1.2.4 Nick Bot的原生固件

__自用固件__ ,极简固件，发布于releases。  
目前不支持OLED和无线USB网卡，具体内容详见readme和releases。  
地址：https://github.com/nicksun98/R2S-OpenWrt

* 4.1.2.5 其他大佬的原生固件

欢迎各位大佬提供自己的固件地址。谢谢大家为开源社区做出的努力和贡献。

---
* __4.2 Friendlywrt固件__



* 4.2.1 友善官方固件


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
