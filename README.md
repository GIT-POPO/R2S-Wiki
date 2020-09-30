## NanoPi-R2S 操作手册

  欢迎查看 NanoPi-R2S 操作手册。本页旨在为 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw) ，收集与解答 NanoPi-R2S 相关问题。  
  如有更好的意见与建议，随时欢迎 PR。  
  或者群内相关发言时直接 @popo  

### 1、R2S 基本介绍

  [NanoPi R2S](https://wiki.friendlyarm.com/wiki/index.php/NanoPi_R2S/zh) 是友善电子团队最新推出的一款实现满速率双千兆的、完全开源的 IoT 应用神器。  

  NanoPi R2S 使用 RK3328 Soc，有两个千兆网络，1G DDR4 内存，支持 Docker CE，完全开源，用于企业物联网二次开发，个人定制 NAS，家庭物联网网关等。  

#### 1.1 开发板参数介绍

<details>
  <summary>点击展开查看详细内容</summary>
  <pre>
  > Soc： Rockchip RK3328, Quad-core Cortex-A53  
  > 内存： DDR4, 1 GB  
  > 网口：  
    RTL8211E（10 / 100 / 1000 Mbps 以太网口 x 1）  
    RTL8153B（USB 3.0 转 10 / 100 / 1000 Mbps 以太网口 x 1）  
  > USB 接口：USB 2.0 Type-A x1  
  > TF 卡槽 x 1  
  > MicroUSB：供电和 Slave 功能  
  > Debug Serial Port：3.3V TTL 电平，3 Pin 2.54 mm 间距排针  
  > LED 灯：LED x 3  
  > 按键：Reset 按键 x 1（可用户自定义功能）  
  > 开发板尺寸：55.6 x 52 mm  
  > 供电：直流 5V / 2A  
  > 使用温度：0℃ 到 80℃  
  </pre>
</details>

---

#### 1.2 灯光按键介绍

* 灯光前面板由 WAN LAN 绿色指示灯 和 SYS 红色指示灯 组成，WAN LAN 指示灯在部分固件下不能亮起，SYS 指示灯在开机状态时，是红色双闪状态。  

* 如果遇到红灯常亮等特殊状态，则说明系统未启动，或者 TF卡 不兼容等状态。插入网线连接其他设备后，网口高低电平灯闪烁，则说明链路状态正常。  

* 若开机后网口灯不正常，或者熄灭，则说明系统未启动或遇到错误。    

* 类原生固件灯光说明：  
    
  > 7 月 17 日后，SYS 灯引导时闪烁，启动后常亮和 LAN 口网口灯不亮是正常现象。  
  > 7 月 22 日后，加入新 Patch 解决 LAN 口网口灯不亮的问题。电平灯和 WAN 相反（接入后黄灯常亮，传输数据时绿灯闪烁）。  
  > 7 月 28 日后，官方正式编写代码支持 LAN 口电平灯显示，状态和 WAN 相同（接入后绿灯常亮，传输数据时黄灯闪烁）。  
  > 至此，一切灯光正常。   

---

### 2、R2S 所需配件

#### 2.1 电源线和电源的选择

  R2S 推荐使用 5V / 2A 的 USB 电源，stress 烤机下满载电流 0.8A。  

  建议使用质量可靠的 USB 线缆 USB-A 或 USB-C 至 MicroUSB。  

  *<注意请慎用 QC 类的充电头供电，以免电压识别有误供电电压过高损坏设备>*  

---

#### 2.2 TF 储存卡的选择及相关问题

  NanoPi R2S 没有内置储存器，使用外置插入式 TF 卡。TF 卡的兼容性和稳定性决定着固件刷写的成功与否，以及后续使用更新固件的稳定性。  

##### 2.2.1 TF 卡推荐

* 本群推荐使用 “朗科与京东联名的 Class10 / SDHC / A1 的 TF 卡”  

  *<主要是便宜、能用、且拥有京东提供后续保修及换新服务>*  

* 某宝等渠道的 “SanDisk Industrial 闪迪工业卡” 。从网友的反馈来看属 MLC 类型 TF 卡，且没 TLC 的擦写缺陷，适合频繁刷机。相比 TLC 拥有更长使用寿命，以及高至 85 度的工作温度。但渠道来源存在风险，请自行判断。  

* 另外可选用 TF 转 eMMC 的特殊 TF 卡，使用上稳定性最佳，但会破坏整体外观。详讯群内 Darya（适合刷写各种系统固件测试等频繁读写使用)。  

##### 2.2.2 TF 卡刷写固件

* 写卡软件：  

  > Windows 下建议使用 [win32diskimager](https://win32diskimager.download) 和 [rufus](https://rufus.ie)。  
  >
  > macOS 建议使用 [balenaEtcher](https://www.balena.io/etcher/)（macOS 和 Linux 也可直接在命令行下用 dd 写入）  

* 普通市售 TLC 类型的 TF 卡，由于可能存在"擦除缺陷"，建议在刷写固件前，尽量做覆盖填充，可选择完全格式化或进行快速格式化后填入一些大文件。  

* 如选用 SquashFS 格式固件，建议刷写成功后 SSH 登入运行命令 `firstboot -y && reboot now` 完全清理。  

* 覆盖写入可被写入精心设计的 2GB 空文件所代替，节约大量时间。无残留，获得更好体验。  

  > 下载地址： [zero2GB.img.gz](https://github.com/nicksun98/Others/raw/master/zero2GB.img.gz) （ 文件大小仅为约 2.1 MB / 1.9 MiB 左右 ）  
  >
  > 使用方法：先使用写卡软件写入 zero2GB.img.gz ，拔出读卡器并重新插入，再写入真正的软件包即可。  
  >
  > 感谢群友 Kane Green 的辛苦付出。  

##### 2.2.3 TF 卡的其他常见问题

* Q: 为什么启动不了系统？  
  A: 首先根据上文确定是不是设置错误。比如 LAN WAN 互换，比如未自动获取 IP 地址。建议根据 2.2.2 的教程重新刷写TF卡。  
    *<另外部分 TF 卡不兼容 R2S，常见能使用兼容 ext4 格式固件而不能使用 SquashFS 格式固件启动>*  

* Q: 为什么 TF 卡读取不了/卡容量减少/刷卡掉速？  
  A: 建议使用专业软件删除分区，重新格式化 TF 卡。或更换读卡器。出现严重问题联系销售商换新。  

* Q: 刷了 SquashFS 的系统为什么原来的配置还在？  
  A: 因为 TLC 类型 TF 卡的特性，SquashFS 格式的系统存在的 Upper Layer 可能没有被擦除。特别是同系列的固件 RootFS 大小相同时会出现此类状况。[详情链接](https://openwrt.org/docs/guide-user/troubleshooting/failsafe_and_factory_reset)  

  解决方案：使用 SSH 等工具进 入R2S 输入：  
  ```shell  
  jffs2reset -y && reboot now
  ```
* Q: 为什么重启后配置丢失？  
  A: 如果您使用的是 SquashFS 文件系统的固件，请再刷机后参照上一条执行清理 Upper Layer 或者 重新挂载 Overlay 分区。如果您使用的是 Ext4 文件系统的固件，请确定 TF 卡是可写入的状态，确定写入的文件及更改已切实写入TF卡，而不是存在于运行 RAM 中。如果遇到连续出现问题或者卡写保护等情况，建议更换 TF 卡，具体请参照 2.2.1 条目。  

  **其他问题请先学会善用 [搜索引擎](https://www.google.com)，尝试自行解决，对于无法自行解决的问题欢迎阅读 [提问的艺术](https://github.com/betaseeker/How-To-Ask-Questions) 后在 Telegram 群 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw) 中提出、讨论。**  

---

#### 2.3 散热选择

* RK3328 是 4 核 A53，在日常使用中，发热量偏大。需要散热来保证不会达到温度墙而掉频。RK3328 的温度墙是 85℃。  
* 本节主要介绍由 R2S 群组发起打造的被动散热铝合金外壳，以及友善厂家前后推出的外壳及其它散热措施。  

##### 2.3.1 铝合金外壳  

  所有铝合金外壳皆出自 Telegram 群 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw)。感谢群友们为 NanoPi R2S 所做的贡献。  
  友善厂家现推出外壳发售，神似 R2S Club 群组的第一代外壳。  
  
  __来自 Telegram 群 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw) 的众筹定制外壳已经全部售罄，感谢大家半年多的努力。经历了三代半的变化，外壳变成现在成熟的样子，每一代的进步都离不开大家的帮助，非常非常感谢为此努力的所有人。__

###### 2.3.1.1 铝合金外壳区别  

  外壳目前分为一代外壳，二代外壳，二代半外壳和三代外壳。  

  > 一代外壳：为铝合金构造，外表做氧化发黑处理，黑色高级纹理搭配顶部散热鳍片。TF 卡 和 USB 做开孔处理，其中限量版首批 300 件有特殊 Logo。  

  > 二代外壳：为铝合金构造，外表做磨砂氧化处理，保持铝合金原色，去掉顶部鳍片和 TF 卡槽，整体更协调，颜值在线。  

  > 二代半外壳：为铝合金构造，外表做磨砂氧化处理，保持铝合金原色，相比于二代加入重新设计开孔 TF 卡槽，插卡取卡更便捷。  

  > 三代外壳：在二代半外壳的基础上，加入 0.91 英寸 OLED 屏幕（详见 2.5 条目），简单安装方法，信息获取更高效。  

  > 友善厂家外壳：类似于一代外壳，用料变薄，优化 TF 卡口 和 外形设计，外观精致。但 Soc 铝柱变短，靠硅脂垫连接 Soc，背后无压片。  

###### 2.3.1.2 安装方法  

  暂略。正确安装才能保证良好散热效果，安装正确的铝合金外壳绝对能应付包括不透风弱电箱等非极端环境下正常使用。如有安装困难或温度过高，不明安装方法可加入 Telegram 群 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw)  

###### 2.3.1.3 半导体和风扇  

  半导体制冷片采用珀耳帖效应，安装在铝壳上可显著降低 Soc 温度。  
  注意做好热面散热和温控，否则会损坏制冷片和凝水导致 R2S 主板进液。  
  外壳外加装风扇会进一步降低 Soc 的温度。  
  交流讨论可加入 Telegram 群 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw)  

###### 2.3.1.4 常见问题  

* Q: 铝合金外壳能降低到多少度？  
    A: 这个要根据室温和空气流通速度来决定。铝合金外壳的最主要目的是满负载情况下能不碰温度墙，稳定 1608 MHz 的 Soc 主频不掉频，全力输出。以以往的经验来说，待机状态下 Soc 温度是室温 +20℃ 以内，弱电箱内 +25℃ 以内。如果温度过高应检查安装问题特别是散热柱与 CPU 是否紧密接触，应使用散热硅脂等确保有良好接触面导热。  

* Q: 绝缘胶带需要贴吗？  
    A: 可贴可不贴。 尽量注意做好绝缘保护，否则会因短路引起关机保护。  

* Q: 硅脂溢出到主板上怎么办？  
    A: 大部分硅脂不导电，没影响。  

* Q: 板子安装不进去怎么办？  
    A: R2S的主板制造过程是从整块大主板上裁切，会有一定的公差。用指甲刀锉一下边缘毛边。  

* Q: 硅脂垫是干什么的？可以不用吗？  
    A: 硅脂垫是贴主板后侧到铝壳底盖的，主要充当散热器弹簧的用途，给 Soc 和铝柱一个额外的压力，帮助更好散热。  

* Q: 群内外壳和友善外壳有什么区别？  
    A: 群内外壳已经众筹到最后一批，卖一个少一个，即将售罄绝版。拥有 OLED 的非凡乐趣和更佳的散热能力。友善外壳靠硅脂垫连接，散热会差一些，但是正常使用和非长时间暴力烤鸡不会碰触温度墙，可正常使用。  

  **其他问题请先学会善用 [搜索引擎](https://www.google.com)，尝试自行解决，对于无法自行解决的问题欢迎阅读 [提问的艺术](https://github.com/betaseeker/How-To-Ask-Questions) 后在 Telegram 群 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw) 中提出、讨论。**  

##### 2.3.2 风扇外壳

  无论是网售亚克力定制的外壳还是原装风扇散热片（裸奔情况下）或其它自制的主动风扇散热方案，都可以显著降低待机温度，满足日常使用。但在极端满载情况下，还是会撞温度墙。如有极端需求还需暴力改造。 主动散热风扇也是故障点，注意维护使用。

##### 2.3.3 其他被动散热

  无论是原装散热片还是加装其他散热片，只要满足不装外壳，存放于空气强流通处，都能显著降低待机温度。  

---

#### 2.4 USB 网卡

  Kernel 5.4 对大多数无线网卡的驱动支持都不好，暂无任何值得推荐使用的型号。  
 
  旧的 FriendlyWrt 可编译进 811ac 等 USB 网卡，设置为 N 模式，5 GHz。强制频宽 40 MHz，信道 40。重启后，可以跑到 100 Mbps 左右。  
 
  原生 OpenWrt 可选用 MT7601 等网卡，群友使用 EDUP 1200M USB 网卡可用，未测试。具体参照固件支持。  

  OpenWrt 支持 USB 的 4G 上网卡，具体请参考有编译支持的固件说明（目前主要支持 Hilink 模式）。  

---

#### 2.5 OLED 屏幕

  目前 OLED 屏幕分为 “0.96 寸狼牙山五壮士版”，和 “三代铝壳自带 0.91 寸屏幕”。  

  “ 0.91 寸屏幕 ” 可使用 NateLol 大佬的 [luci-app-oled](https://github.com/NateLol/luci-app-oled) 来选择显示内容（部分固件编译内置）。  

---

#### 2.6 其他外设

  狼牙山五壮士等特殊外设。  

---

### 3、R2S 主要功能  

#### 3.1 路由器

  超小体积的双千兆网口的 ARM 四核 A53 开发板，非常适合做软路由器。  

##### 3.1.1 主路由  

  双网口分别用于LAN、WAN，结构简单，推荐大多数场合使用。R2S Club 群组测试双网口 NAT 转发性能 940 Mbps，已跑满千兆网口。 

##### 3.1.2 单臂路由  

  单网口接 VLAN 交换机，使用 VLAN 区分 LAN、WAN，单线复用。参考 IEEE 802.1q 协议等资料。 （简单 PPPoE-WAN 也可不用 VLAN 隔离，单网口既做 LAN 又兼承载 PPPoE） 

##### 3.1.3 “旁路由”  

  极不推荐。  

---

#### 3.2 Linux 服务器和 Docker 容器

  支持 Armbian 稳定版 和 Ubuntu 等版本，绝大多数系统下支持 Docker，如 Armbian 的 Docker OpenWrt，OpenWrt 下 Docker Awtrix 等等操作。  
  详见 4.3 和 4.4 条目。  

---

#### 3.3 电视盒子

  *<尚未开发>*  

---

#### 3.4 暖手宝

  *<皮一下很开心。————Nick Bot>*  

---

### 4、R2S 主要固件介绍

#### 4.1 原生 OpenWrt 固件

##### 4.1.1 OpenWrt 官方主线支持 R2S

* 2020 年 7 月 28 日，OpenWrt 官方主线加入了对 NanoPi R2S 的正式支持，使用 Longterm Kernel 5.4 内核。感谢为此付出努力的一众网友，感谢 [OpenWrt](https://openwrt.org/) 官方、[Project OpenWrt](https://github.com/project-openwrt) 项目组 和 [Quintus Chu](https://t.me/quintuschu) 大佬，感谢世界各地为开源社区努力贡献的朋友们。  

  源码地址：[openwrt/openwrt](https://github.com/openwrt/openwrt)  

##### 4.1.2 R2S Club 群友基于原生 OpenWrt 源码提供的预编译固件

  源于开源社区的努力，使用 OpenWrt 官方源码，各路大佬按自身所需定制编译。 __有特殊需求者请自行编译，不提供任何免费技术支持__ ，现成固件请自行甄选使用。OpenWrt 固件分为 EXT4 格式版本及 SquashFS 格式版本，若使用 TLC 类型 TF 卡写入 SquashFS 格式的固件启动失败，才建议换用 EXT4 格式固件。SquashFS 格式固件可在误配置等情况下使用恢复固件初始设置重置系统（使用 LuCI 管理页面相关菜单或使用 `firstboot` 等命令），EXT4 格式则不具备此功能。下列基于原生 OpenWrt 源码编译固件均提供 SquashFS 格式，部分提供 EXT4 格式。  

  __固件介绍详见各固件仓库主页的 README 和 Releases。__ 部分超频固件保持 1.45V 电压超频至 1608 MHz，发热不变，更高性能。部分固件自带 Soc 调频，不懂不要动。电压表 1 GHz 下都相同，不建议频率低于 800 MHz，一切问题后果自负。若出现反复重启不能稳定，说明 Soc 体质偏差，建议放弃超频。  

  设置错误或误操作后，SquashFS 可执行重置，部分固件带了 `fuck` 组件清除残留设置，使用更佳。强烈建议在刷写 SquashFS 固件后执行 `fuck` 或者 `firstboot -y && reboot now` ，使用备份和升级保留备份前请务必确保清楚了解所做的操作。  

* QiuSimons（404 大佬）  
  地址：[project-openwrt/R2S-OpenWrt](https://github.com/project-openwrt/R2S-OpenWrt)  

* Quintus Chu（502 大佬）  
  地址：[quintus-lab/Openwrt-R2S](https://github.com/quintus-lab/Openwrt-R2S)  

* CN_SZTL（高大全版）  *<包含 18.06 版本>*  
  地址：[Telegram @nanopi\_r2s](https://t.me/nanopi\_r2s)  

* Kane Green  
  地址：[KaneGreen/R2S-OpenWrt](https://github.com/KaneGreen/R2S-OpenWrt)  

* Nick Bot  
  地址：[nicksun98/R2S-OpenWrt](https://github.com/nicksun98/R2S-OpenWrt)  

* 墨子冥  
  地址：[msylgj/R2S-OpenWrt](https://github.com/msylgj/R2S-OpenWrt)  

* Chuck  *<注意 Branch>*  
  地址：[fanck0605/openwrt-nanopi-r2s](https://github.com/fanck0605/openwrt-nanopi-r2s)  

  
  **欢迎补充，欢迎您为开源社区作出贡献。如果您在 Telegram [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw) 群内询问哪个固件最好用/最稳定等愚蠢问题，您将被移出群聊，因为您没有阅读 Wiki，以及您并没有看到这行文字。**  
  
##### 4.1.3 基于 Jayanta525 源码的类原生固件

  使用 [Jayanta525/openwrt-nanopi-r2s](https://github.com/jayanta525/openwrt-nanopi-r2s) 的源码，是原生支持前的重要阶段。但是有未改好 IRQ 等等问题，使用舒适度和性能完整度较落后于 OpenWrt 官方以及群友发布的版本。  

  “这个家伙很懒，什么都没有留下。”  

---

#### 4.2 FriendlyWrt 固件

  已不推荐使用，内容已转移，[点击查看](/others/friendlywrt.md)  

---

#### 4.3 Armbian 固件

* 地址：[Armbian/nanopi-r2s](https://www.armbian.com/nanopi-r2s/)  

* 教程来自 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw) 群主 Darya 的博客：  

  > 地址: [e.g.daryasyr.com](https://e.g.daryasyr.com)  

---

#### 4.4 其它Linux 发行版固件

* 等待 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw) 群主 Darya 补充，敬请期待！  

---

#### 4.5 NanoPi R2S 在线升级 / 降级 / 更改固件  

##### 4.5.1 使用 LuCI 网页管理界面的"备份/升级"功能更新固件

  LuCI 管理页面，进入"系统-备份/升级"，选择刷写新的固件，上传要刷写的固件应是 .img.gz 或 .img 格式。  

  请自行核对 MD5 值和 SHA256 值确保文件一致性，选择"刷写固件"，R2S 会更新系统并且会自行重启。  

  *<注意！如果是使用 SquashFS 格式的固件，由于 TLC 类型 TF 卡的擦写缺陷，即使选择“不保存配置“仍然会保存配置(使用 EXT4 格式固件，或使用 MLC TF 卡及 TF 卡转 emmc 的除外)>*  
 
  如果是使用普通市售 TLC 类型 TF 卡，建议升级完成重启后，使用SSH进一步清除旧配置：  

  ```shell  
  firstboot -y && reboot now
  ```

  *<注意！ TLC 类型 TF 卡，无论是使用读卡器写卡刷入固件还是在线更新固件，当使用 SquashFS 格式固件时，如出现不能正常启动、或旧配置文件没能清理导致无法正常运作。请反复尝试或写入其它大文件或换用 EXT4 格式固件消除擦写缺陷>*  

##### 4.5.2 命令行下更新固件  

  前提：

> 1. 熟悉简单 SSH 命令行操作  
>
> 2. 通过 scp、sftp 或“文件传输”（部分固件有编译此应用，管理页面“系统 - 文件传输”）等方式，把 .img 或 .img.gz 格式固件上传至 R2S ，例如放在 /tmp 目录下。  

  直接使用 dd 命令把固件 img 全新写入TF卡更新系统，"/dev/mmcblk0" 是 R2S 的 TF 卡设备位置，将 <openwrt.img.gz> 替换成实际文件名（不包括 <> 括号）。  

  ```shell  
  dd if=/tmp/<openwrt.img> of=/dev/mmcblk0 conv=fsync
  reboot
  ```

  使用内置的 sysupgrade 命令：  

  ```shell  
  sysupgrade -v /tmp/<openwrt.img.gz>
  reboot
  ```

  等待重启后，重连 SSH，输入：  

  ```shell  
  firstboot -y && reboot now
  ```
  
##### 4.5.3 从FriendlyWrt 升级到 OpenWrt

  建议使用 dd 命令进行写盘，参考上一条 4.5.2  

  ```shell  
  dd if=/tmp/upload/<openwrt.img> of=/dev/mmcblk0 conv=fsync
  reboot
  ```
  
---

#### 4.6 常见问题

* Q: 开机后网口灯亮，分配不到 IP / 分配到 169.254.x.x ？  
  A: 确认您刷的固件插入正确的网口，如正确，手动分配子网内 IP，若可以进入后台，恢复到出厂设置（执行重置），进行干净部署。若未解决，进行换网口操作。  

* Q: 固件开启 BBR 加速了吗？  
  A: 原生固件默认打开 BBR 加速，部分固件编译带有 Turbo ACC 工具可检查，在“网络 - Turbo ACC 加速”。5.4 内核的 BBR 已修复万兆掉速问题。  

* Q: Flow 加速 / 软件流量分载 / Shortcut-FE 加速要开吗？  
  A: Flow Offloading / Shortcut-FE 等建议带宽超过 500 Mbps 的用户打开。请勿同时开启，切换使用时建议重启以避免不必要的问题。 两者性能基本一致。 

* Q: HWNAT / 硬件流量分载要开吗？  
  A: OpenWrt 的硬件流量分载仅支持 MTK76xx，即使勾选也没有任何意义。  

* Q: DNS 加速要开吗？  
  A: 如你的网络可以正常解析 DNS，非必要情况下无需开启；此项功能可能会与 AdGuardHome、SmartDNS 等发生冲突，请注意。  

* Q: FullCone NAT 哪里开？可以开吗？  
  A: 在 “网络 - 防火墙 - FullCone” 打开，建议打开。部份固件安装有 Turbo ACC 应用，可打开检查。某些主题可能存在显示项兼容性问题，可在防火墙的 WAN 口设置里打开。  

* Q: 类原生固件可以使用 sysupgrade 吗？  
  A: 可以上传 img.gz 文件在线升级，不建议保留配置。RootFS 相同的固件，使用 TLC 类型 TF 卡，可能存在选择“不保存配置”升级也是保存配置，参考 2.2.3 条目。  

* Q: 建议使用备份吗？  
  A: 如果你长期使用某一个人的版本，且未有重大更新，建议保留配置直接升级。跨版本升级请尽量干净部署。  

* Q: 为什么更改不了 LAN IP / 管理地址？  
  A: 这是 OpenWrt 19.07 带来的新 Feature，用于误操作失效后自动回滚。可点击保存并应用旁边向下的箭头，选择“强制应用”，或参考 5.2.2 条目。  

  **其他问题请先学会善用 [搜索引擎](https://www.google.com/)，尝试自行解决，对于无法自行解决的问题欢迎阅读 [提问的艺术](https://github.com/betaseeker/How-To-Ask-Questions) 后在 Telegram 群 [NanoPi R2S Club](https://t.me/joinchat/JcBRDFWlAEMysWbVdPsxFw) 中提出、讨论。**  

---

### 5、OpenWrt 使用基本介绍

#### 5.1 OpenWrt基本知识整理

  假若有意了解 OpenWrt，可参阅佐须之男的 [OpenWrt 知识整理](http://www.openwrt.pro/post-170.html)

#### 5.2 SSH 操作

  使用者应具备一点最基础的系统操作能力，例如学会一点 SSH 操作

##### 5.2.1 SSH 客户端的下载及连接

> Windows 用户：下载 [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)  
>
> Linux / macOS 用户：终端输入：  

  ```shell  
  ssh -p <你的 DropBear 端口号，默认为 22> root@<你的 R2S 的 IP 地址>
  ```

  *<注意，去掉 <> 括号，建议修改端口号为非 22，LAN Only 。输入密码不会有任何回显。 OpenWrt 缺省只有 root 一个用户>*  

##### 5.2.2 SSH 的基本操作

* 更改 LAN IP  

  ```shell
  uci set network.lan.ipaddr=<新的 LAN 地址>
  uci commit network
  /etc/init.d/network restart
  ```
  
  *<注意，不包括 <> 括号。>*
  
* 使用 vi / vim 命令  

  [vi / vim 命令教程](https://www.runoob.com/linux/linux-vim.html)  

  例如:
  
  ```shell  
  vim /etc/config/network
  /etc/init.d/network restart
  ```

  *<换网口 ETH，改 WAN 口、LAN 口配置，更改 MAC 地址等均可通过 network 文件更改，希望大家举一反三，一通百通>*  

* 使用 SSH 查看设备温度  

  ```shell    
  cat /sys/class/thermal/thermal_zone0/temp
  ```

* 使用 SSH 查看设备 Soc 频率  

  ```shell  
  cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
  ```

* 使用 cputemp.sh 脚本查看温度频率  
         
  下载脚本到 /bin 路径下：  

  ```shell  
  wget https://raw.githubusercontent.com/nicksun98/Others/master/cputemp.sh -O /bin/cputemp.sh
  ```

  使用脚本： 
  
  ```shell
  chmod +x /bin/cputemp.sh
  /bin/cputemp.sh
  ```
  或者
  ```shell  
  sh /bin/cputemp.sh
  ```

  退出使用 `Ctrl + C`  

---

## 未完待续，欢迎补充！
