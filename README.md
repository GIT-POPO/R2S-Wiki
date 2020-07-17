## NanoPi-R2S操作手册

欢迎查看 NanoPi-R2S 操作手册。本页旨在为 [NanoPi R2S Club](https://t.me/nanopir2sshell) ，收集与解答 NanoPi-R2S 相关问题。
如有更好的意见与建议，随时欢迎PR。
或者群内相关发言时直接 @popo  

### 1、R2S基本介绍

*NanoPi R2S 是友善电子团队最新推出的一款实现满速率双千兆的、完全开源的 IoT 应用神器。*

*NanoPi R2S 使用 RK3328 Soc，有两个千兆网络，1G DDR4内存，支持 Docker CE, 完全开源，用于企业物联网二次开发，个人定制 NAS，家庭物联网网关等。*

* __1.1 开发板参数介绍__

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

  * 灯光前面板由 wan lan绿色指示灯 和 sys红色指示灯 组成，wan lan指示灯在部分固件下不能亮起，sys指示灯在开机状态时，是红色双闪状态。  
  * 如果遇到红灯常亮等特殊状态，则说明系统未启动，或者TF卡不兼容等状态。插入网线连接其他设备后，网口高低电平灯闪烁，则说明链路状态正常。  
  * 若开机后网口灯不正常，或者熄灭，则说明系统未启动或遇到错误。  
  * reset按键目前未使用。

---
### 2、R2S所需配件

* __2.1 电源线和电源的选择__

  R2S 推荐使用 5V2A 的 USB电源，stress烤机下满载电流0.8A。  
  建议使用线径粗，接口质量好的 USB-A 至 MicroUSB 的数据线，否则概率出现问题。  
  *<接口可更改为USB-C接口>*

---
* __2.2 TF卡的选择__

  TF卡的兼容性和稳定性决定着刷机的成功与否和后续使用上的稳定性。

  * 2.2.1 TF卡推荐

    * 本群推荐使用 “朗科与京东联名的Class10/SDHC/A1的TF卡”。  
    *<京东提供后续保修及换新服务>*

    * 另外大佬推荐 “SanDisk Industrial闪迪工业卡” 。MLC颗粒，均衡磨损带来更长寿命，高至85度的工作温度，同样适合R2S的工作环境。

  * 2.2.2 TF卡刷写建议

    * 刷写软件建议使用 [win32diskimager](https://win32diskimager.download/) 和 [rufus](https://rufus.ie/)。  
    * 我们建议在刷写原生固件前，进行覆盖格式化或进行快速格式化后填入1GB左右的大文件。  
    * 以及再刷写成功后进行 `firstboot -y && reboot now` 的操作。

  * 2.2.3 TF卡的其他常见问题

    **Q: 为什么启动不了系统？**  
    A: 首先根据上文确定是不是设置错误。比如lan wan互换，比如未自动获取IP地址。建议根据2.2.2的教程重新刷写TF卡。  
    *<另外部分TF卡不兼容R2S，甚至兼容友善EXT4不兼容SquashFS>*

    **Q: 为什么TF卡读取不了/卡容量减少/刷卡掉速?**  
    A: 建议使用专业软件删除分区，重新格式化TF卡。或更换读卡器。出现严重问题联系销售商换新。

    **Q: 刷了SquashFS的系统为什么原来的配置还在？**  
    A: 因为TF卡的特性，Upper Layer很难被抹掉。当固件Rootfs大小相同时会出现此类状况。 [详情链接](https://openwrt.org/docs/guide-user/troubleshooting/failsafe_and_factory_reset)  
    解决方案：使用SSH等工具进入R2S输入：  
    ```
    jffs2reset -y && reboot now
    ```
  

---
* __2.3 散热选择__

  RK3328 在日常使用中，发热量偏大。需要散热来保证不会达到温度墙而掉频。  
  RK3328 的温度墙是 85℃

  * 2.3.1 铝合金外壳

    所有铝合金外壳皆出自 Telegram群 [NanoPi R2S Club](https://t.me/nanopir2sshell)。  
    感谢群友们为 NanoPi R2S 所做的贡献。

    __购买外壳可加入 Telegram群 [NanoPi R2S Club](https://t.me/nanopir2sshell)，看群置顶。或 [淘宝链接](https://taoquan.taobao.com/coupon/unify_apply.htm?sellerId=1613492699&activityId=156945e116dc477780f99dedf3ed91b3)，领券下单更优惠。__

    * 2.3.1.1 外壳区别

      外壳目前分为一代外壳，二代外壳，二代半外壳和三代外壳。

      > 一代外壳：为铝合金构造，外表做氧化发黑处理，黑色高级纹理搭配顶部散热鳍片。TF卡和USB做开孔处理，其中限量版首批300件有特殊logo。

      > 二代外壳：为铝合金构造，外表做磨砂氧化处理，保持铝合金原色，去掉顶部鳍片和TF卡槽，整体更协调，颜值在线。

      > 二代半外壳：为铝合金构造，外表做磨砂氧化处理，保持铝合金原色，相比于二代加入重新设计高级TF卡槽，插卡取卡更便捷。

      > 三代外壳：在二代半外壳的基础上，加入0.91英寸OLED屏幕(详见2.5条目)，简单安装方法，信息获取更高效。

    * 2.3.1.2 安装方法

      暂略。如有安装困难或温度过高，不明安装方法可加入 Telegram群 [NanoPi R2S Club](https://t.me/nanopir2sshell)

    * 2.3.1.3 半导体和风扇

      半导体制冷片采用珀耳帖效应，安装在铝壳上可显著降低Soc温度。  
      注意做好热面散热和温控，否则会损坏制冷片和凝水导致R2S主板进液。  
      外壳外加装风扇会进一步降低Soc的温度。
      交流讨论可加入 Telegram群 [NanoPi R2S Club](https://t.me/nanopir2sshell)

    * 2.3.1.4 常见问题

      **Q: 铝合金外壳能降低到多少度？**  
      A: 这个要根据室温和空气流通速度来决定。铝合金外壳的最主要目的是满负载情况下能不碰温度墙，稳定1.512GHz的Soc主频不掉频，全力输出。以以往的经验来说，待机状态下Soc温度是室温+20以内，弱电箱内+25以内。如果过高考虑下安装问题和硅脂接触面积。

      **Q: 绝缘胶带需要贴吗？**  
      A: 可贴可不贴。

      **Q: 硅脂溢出到主板上怎么办？**  
      A: 大部分硅脂不导电，没影响。

      **Q: 板子安装不进去怎么办？**  
      A: R2S的主板是从大主板上裁切下来的，会有一定的公差。用指甲刀锉一下边缘毛边。

      **Q: 硅脂垫是干什么的？可以不用吗？**  
      A: 硅脂垫是贴主板后侧到铝壳底盖的，主要充当散热器弹簧的用途，给Soc和铝柱一个额外的压力，帮助更好散热。

  * 2.3.2 风扇外壳

    无论是亚克力定制的外壳还是原装风扇散热片(裸奔情况下。)，都可以显著降低待机温度，满足日常使用。但在极端满载情况下，还是会撞温度墙。如有极端需求还需暴力改造。

  * 2.3.3其他被动散热

    无论是原装散热片还是加装其他散热片，只要满足不装外壳，存放于空气强流通处，都能显著降低待机温度。

---
* __2.4 USB网卡__

  * FriendlyWrt 可编译进 811ac等USB网卡，设置为N模式，5GHz。强制频宽40MHz，信道40。重启后，可以跑到100Mbps左右。  
  * 原生OpenWrt 可选用 MT7601等网卡，具体参照固件支持。

---
* __2.5 OLED屏幕__

  目前 OLED屏幕分为 “0.96寸狼牙山五壮士版”，和 “三代铝壳自带0.91寸屏幕”。

  “0.91寸屏幕” 可使用 natelol 大佬的 [luci-app-oled](https://github.com/NateLol/luci-app-oled) 来选择显示内容(部分固件有编译进)

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

    接 VLAN 交换机，单线复用。参考 IEEE 802.1q 协议等资料。

  * 3.1.3 “旁路由”

    极不推荐。

---
* __3.2 Linux服务器和Docker容器__

  支持 Armbian 稳定版 和 Ubuntu 等版本，绝大多数系统下支持 docker，如 Armbian 的 docker OpenWrt，OpenWrt 下 Docker Awtrix 等等操作。  
  详见4.3和4.4条目。

---
* __3.3 电视盒子__

  *<尚未开发>*

---
* __3.4 暖手宝__

  *<皮一下很开心。————Nick Bot>*

---
### 4、主要固件介绍

* __4.1 原生OpenWrt固件__

  * 4.1.1 原生支持 OpenWrt 固件

    * [开发中](https://github.com/openwrt/openwrt/pull/2945)

  * 4.1.2 Snapshot 版原生固件

    * 源于开源社区的努力。

    * 4.1.2.1 QiuSimons 原生固件（404大佬）

      __自用固件__ ,分docker版和无docker版。  
      目前支持OLED和部分USB网卡，具体内容详见readme。  
      已托管给ctcgfw  
      *<注意，lan wan已互换>*  
      地址：https://github.com/project-openwrt/R2S-OpenWrt  
      IP: 192.168.1.1 密码: 无

    * 4.1.2.2 Quintus Chu原生固件（502大佬）

      __自用固件__ ,分OpenWrt版和FriendlyWrt版，FriendlyWrt版详见4.2.2.3条目。  
      固件分为slim版和full版，目前支持OLED和部分USB网卡，具体内容详见readme和releases。  
      地址：https://github.com/quintus-lab/Openwrt-R2S  
      IP: 192.168.1.1 密码: 无

    * 4.1.2.3 Kane Green的原生固件

      __自用固件__ ,高大全固件，发布于Actions。  
      具体内容详见readme。  
      地址：https://github.com/KaneGreen/R2S-OpenWrt  
      IP: 192.168.1.1 密码: 无

    * 4.1.2.4 Nick Bot的原生固件

      __自用固件__ ,极简固件，发布于releases。  
      目前不支持OLED和无线USB网卡，具体内容详见readme和releases。  
      地址：https://github.com/nicksun98/R2S-OpenWrt  
      IP: 192.168.1.1 密码: 无

    * 4.1.2.5 子冥大佬固件

      __自用固件__ ,极简固件，发布于releases。  
      支持0.91寸和0.96寸OLED, 具体内容详见readme和releases。  
      *<注意，lan wan已互换>*  
      地址：https://github.com/msylgj/R2S-OpenWrt  
      IP: 192.168.2.1 密码: password

    * 4.1.2.6 其他大佬的原生固件

      目前Lean的源库支援NanoPi R2S的原生编译。但是目前未改好irq等等问题，使用舒适度和性能完整度暂时落后于本群进度。

      欢迎各位大佬提供自己的固件地址。谢谢大家为开源社区做出的努力和贡献。

---
* __4.2 Friendlywrt固件__

  基于 [友善官方源码](https://wiki.friendlyarm.com/wiki/index.php/How_to_Build_FriendlyWrt/zh) 和 开源社区 的努力。来自 armbian内核 和 openwrt上层 杂交而成。

  * 4.2.1 友善官方固件

    * 官方固件，[下载地址](http://download.friendlyarm.com/nanopir2s)

  * 4.2.2 来自开源社区的大神固件


    * 4.2.2.1 Dayong固件

      分为Lean版，Lienol版(已停更)，Minimal版。最早的稳定固件作者。  
      发布于releases，支持811ac网卡，具体内容详见readme。  
      地址：https://github.com/klever1988/nanopi-openwrt  
      IP: 192.168.2.1 密码: password

    * 4.2.2.2 Chuck固件

      __自用固件__ ,来自于Chuck魔改内核。  
      发布于releases，具体内容详见readme和releases。  
      *<注意，lan wan已互换>*  
      地址：https://github.com/fanck0605/nanopi-r2s  
      IP: 192.168.2.1 密码: password


    * 4.2.2.3 Quintus Chu友善固件（502大佬）

      __自用固件__ ,分OpenWrt版和FriendlyWrt版，OpenWrt版详见4.1.2.2条目。  
      固件分为slim版，opt版，tiny版，另有多拨特别版。支持811ac网卡，具体内容详见readme和releases。  
      地址：https://github.com/quintus-lab/Openwrt-R2S  
      IP: 192.168.2.1 密码: password

    * 4.2.2.4 QiuSimons固件/原生luci（404大佬）

      __自用固件__ ,杂交19.07原生openwrt的luci

    * 4.2.2.5 SongChenwen固件

      分为FwF版和FriendlyWRT版，使用Chuck的魔改内核，使用支持Clash的passwall魔改版本，具体内容详见readme和releases。  
      地址：https://github.com/songchenwen/nanopi-r2s  
      IP: 192.168.2.1 密码: password

    * 4.2.2.6 RT固件

      恩山大佬，RT。(固件略过时，暂未及时更新)  
      地址：https://t.me/R2Spasswall  
      IP: 192.168.2.1 密码: password

    * 4.2.2.7 eSir固件

      YouTuber技术大神，eSir。(固件略过时，暂未及时更新)  
      地址：https://drive.google.com/drive/folders/1DPy8lXhg2btbHsTvpUGVZF2k1rmT7DXA  
      IP: 192.168.5.1 密码: password

    * 4.2.2.8 其他大佬的固件

      欢迎各位大佬提供自己的固件地址。谢谢大家为开源社区做出的努力和贡献。

---
* __4.3 Armbian固件__
  * 等待[NanoPi R2S Club](https://t.me/nanopir2sshell)群主Darya补充，敬请期待！

---
* __4.4 Linux发行版固件__

  * 等待[NanoPi R2S Club](https://t.me/nanopir2sshell)群主Darya补充，敬请期待！

---
* __4.5 在线刷机__

  * NanoPi R2S 支持在线升级/降级/更改固件。

  * 4.5.1 FriendlyWrt 到 FriendlyWrt

    * 部分固件内置有SongChenwen大佬的[luci-app-r2sflasher](https://github.com/songchenwen/nanopi-r2s/tree/master/luci-app-r2sflasher)，可实现图形化刷机。

    * 或者使用Gary大佬的在线刷机脚本：
    ```
    wget -q https://github.com/quintus-lab/Openwrt-R2S/raw/master/script/update.sh && sh ./update.sh
    ```
          *<3 4的ardanzhu固件链接已失效>*

  * 4.5.2 FriendlyWrt 到 Openwrt

    建议使用dd命令进行写盘：

    先将要刷的固件解压到img，通过scp命令或者文件上传功能上传到tmp/upload/文件夹内。  
    通过SSH逐步运行下方的代码，将<openwrt.img>替换为您上传的固件名称（注意，不要加尖括号）。
    ```
    dd if=/tmp/upload/<openwrt.img> of=/dev/mmcblk0 conv=fsync

    reboot
    ```
  * 4.5.3 Openwrt 到 Openwrt

    也可以使用dd命令，不过建议使用内置的sysupgrade命令：

    确保固件是.img.gz格式，通过scp命令上传到tmp文件夹。  
    通过SSH逐步运行下方的代码，将<openwrt.img.gz>替换为您上传的固件名称（注意，不要加尖括号）。
    ```
    sysupgrade -v tmp/<openwrt.img.gz>

    reboot
    ```
    等待重启后，重连SSH，输入：
    ```
    firstboot -y && reboot
    ```

    也可以通过luci的方式进行更新：

      luci页面，进入系统-备份/升级，选择刷写新的固件，上传要刷写的固件.img.gz。  
      核对MD5值和SHA256值，选择刷写固件，这时您的R2S会重启。  
      *(注意！同Rootfs大小的固件，这里保不保存配置都是保存配置，TF卡转emmc的除外)*

      等待重启后，重连SSH，输入：
      ```
      firstboot -y && reboot
      ```

---
* __4.6 常见问题__
 
  **Q: 开机后网口灯亮，分配不到IP/分配到169.254.x.x？**  
  A: 确认您刷的固件插入正确的网口，如正确，手动分配子网内IP，若可以进入后台，恢复到出厂设置（执行重置），进行干净部署。若未解决，进行换网口操作。

  **Q: 固件开启bbr加速了吗？**  
  A: 原生固件默认打开bbr加速，友善固件在网络-turbo acc加速打开。5.4内核的bbr没万兆bug。

  **Q: flow加速/软件流量分载/sfe加速要开吗？**  
  A: flow offloading/sfe等建议带宽超过500Mbps的用户打开。不要同时开，开一个就好。

  **Q: HWNAT/硬件流量分载要开吗？**  
  A: 又不是MTK76xx，不能开！（开东西前一定知道你在干什么！）

  **Q: DNS加速要开吗？**  
  A: 不建议。

  **Q: Fullcone NAT哪里开？可以开吗？**  
  A: 在网络-防火墙-Fullcone打开，建议打开。找不到的可能是主题存在兼容性问题，可以在防火墙的wan口设置里更改。

  **Q: 建议使用备份吗？**  
  A: 不建议，后果自负。

  **Q: 为什么更改不了lan IP/管理地址？**  
  A: 这是OpenWrt 19.07带来的新Feature，防止不懂的人乱改。点击保存并应用旁边向下的箭头，点击强制应用，或参考5.1.1条目。

  其他问题可以在Telegram群[NanoPi R2S Club](https://t.me/nanopir2sshell)询问群主和管理。

---
### 5、OpenWrt使用基本介绍

* __5.1 SSH的使用__

  * Windows用户：下载[putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)  
  * macOS Linux用户：终端输入：
    ```
    ssh -p <你的dropbear端口号，默认22> root@<你的R2S的IP地址>
    ```
  * （注意，去掉尖括号。建议修改端口号为非22，lan only。输入密码不会提示位数。openwrt只有root一个用户）
    
  * 5.1.1 SSH的初步体验

    * 使用vi/vim命令[vi/vim命令教程](https://www.runoob.com/linux/linux-vim.html)  
      *更改lan IP，换网口eth，改wan口、lan口配置，更改mac地址等均可通过network文件更改，希望大家举一反三，一通百通*
    ```
    vim /etc/config/network
    ```
   
    * 使用SSH查看设备温度
    ```    
    cat /sys/class/thermal/thermal_zone0/temp
    ```
    * 使用SSH查看设备Soc频率
    ```
    cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
    ```
   * 使用cputemp.sh脚本查看温度频率
    
      下载脚本到/bin路径下
      ```
      wget https://raw.githubusercontent.com/nicksun98/Others/master/cputemp.sh -O /bin/cputemp.sh
      ```
      使用脚本
      ```
      sh /bin/cputemp.sh
      ```
      退出使用 `Ctrl + C`

---
## 未完待续，欢迎补充！
