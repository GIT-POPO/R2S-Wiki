## NanoPi-R2S 操作手册  

欢迎查看 NanoPi-R2S 操作手册。本页旨在为 [NanoPi R2S Club](https://t.me/nanopir2sshell) ，收集与解答 NanoPi-R2S 相关问题。  
如有更好的意见与建议，随时欢迎PR。  
或者群内相关发言时直接 @popo  

### 1、R2S 基本介绍  

NanoPi R2S 是友善电子团队最新推出的一款实现满速率双千兆的、完全开源的 IoT 应用神器。  

NanoPi R2S 使用 RK3328 Soc，有两个千兆网络，1G DDR4 内存，支持 Docker CE, 完全开源，用于企业物联网二次开发，个人定制 NAS，家庭物联网网关等。  

* __1.1 开发板参数介绍__  

  > Soc: Rockchip RK3328, Quad-core Cortex-A53  
  > DDR4 内存: 1GB  
  > 网口：  
  > RTL8211E（10 / 100 / 1000M 以太网口 x 1）  
  > RTL8153B（USB 3.0 转 10 / 100 / 1000M 以太网口 x 1）  
  > USB 2.0 接口: USB Type-A x1  
  > TF 卡槽 x 1  
  > MicroUSB: 供电和 Slave 功能  
  > Debug Serial Port: 3.3V TTL 电平，3Pin 2.54mm 间距排针  
  > LED 灯: LED x 3  
  > 按键: Reset 按键 x 1（可用户自定义功能）  
  > 开发板尺寸: 55.6 x 52mm  
  > 供电: 直流 5V / 2A  
  > 使用温度: 0℃ 到 80℃  

  > [友善官方详细介绍](https://wiki.friendlyarm.com/wiki/index.php/NanoPi_R2S/zh)  

---  
* __1.2 灯光按键介绍__  

  * 灯光前面板由 WAN LAN 绿色指示灯 和 SYS 红色指示灯 组成，WAN LAN 指示灯在部分固件下不能亮起，SYS 指示灯在开机状态时，是红色双闪状态。  
  * 如果遇到红灯常亮等特殊状态，则说明系统未启动，或者 TF卡 不兼容等状态。插入网线连接其他设备后，网口高低电平灯闪烁，则说明链路状态正常。  
  * 若开机后网口灯不正常，或者熄灭，则说明系统未启动或遇到错误。  
  * Reset 按键目前未定义。  
  *<注意！7 月 17 日后类原生固件，SYS 灯引导时闪烁，启动后常亮和 LAN 口网口灯不亮是正常现象，等待上游更新。>*  
  *<7月22日后加入新 Patch 解决 LAN 口网口灯不亮的问题。电平灯和 WAN 相反。>*  

---  
### 2、R2S 所需配件  

* __2.1 电源线和电源的选择__  

  R2S 推荐使用 5V 2A 的 USB 电源，stress 烤机下满载电流 0.8A。  
  建议使用线径粗，接口质量好的 USB-A 至 MicroUSB 的数据线，否则概率出现问题。  
  *<接口可更改为 USB-C 接口>*  

---  
* __2.2 TF 卡的选择__  

  TF 卡的兼容性和稳定性决定着刷机的成功与否和后续使用上的稳定性。  

  * 2.2.1 TF 卡推荐  

    * 本群推荐使用 “朗科与京东联名的 Class10 / SDHC / A1 的 TF 卡”。  
    *<京东提供后续保修及换新服务>*  

    * 另外大佬推荐 “SanDisk Industrial 闪迪工业卡” 。MLC 颗粒，均衡磨损带来更长寿命，高至 85 度的工作温度，同样适合 R2S 的工作环境。  
    
    * 如果想没有任何问题，可以牺牲外观选用 TF-eMMC 的特殊 TF 卡启动，详讯群内 Darya。（只适合刷机狂魔）  

  * 2.2.2 TF 卡刷写建议  

    * 刷写软件建议使用 [win32diskimager](https://win32diskimager.download/) 和 [rufus](https://rufus.ie/)。  
    * 我们建议在刷写原生固件前，进行覆盖格式化或进行快速格式化后填入 1GB 左右的大文件。  
    * 以及再刷写成功后进行 `firstboot -y && reboot now` 的操作。（仅支持 SquashFS）  

  * 2.2.3 TF 卡的其他常见问题  

    **Q: 为什么启动不了系统？**  
    A: 首先根据上文确定是不是设置错误。比如 LAN WAN 互换，比如未自动获取 IP 地址。建议根据 2.2.2 的教程重新刷写TF卡。  
    *<另外部分 TF 卡不兼容 R2S，甚至兼容友善 EXT4 不兼容 SquashFS>*  

    **Q: 为什么 TF 卡读取不了/卡容量减少/刷卡掉速?**  
    A: 建议使用专业软件删除分区，重新格式化 TF 卡。或更换读卡器。出现严重问题联系销售商换新。  

    **Q: 刷了 SquashFS 的系统为什么原来的配置还在？**  
    A: 因为 TF 卡的特性，Upper Layer 很难被抹掉。当固件 RootFS 大小相同时会出现此类状况。 [详情链接](https://openwrt.org/docs/guide-user/troubleshooting/failsafe_and_factory_reset)  
    解决方案：使用 SSH 等工具进 入R2S 输入：  
    ```  
    jffs2reset -y && reboot now
    ```  
    
    其他问题请先学会善用 [搜索引擎](https://www.google.com)，尝试自行解决，对于无法自行解决的问题欢迎阅读 [提问的艺术](https://github.com/betaseeker/How-To-Ask-Questions) 后在 Telegram 群 [NanoPi R2S Club](https://t.me/nanopir2sshell) 中提出、讨论。 

---  
* __2.3 散热选择__  

  RK3328 在日常使用中，发热量偏大。需要散热来保证不会达到温度墙而掉频。  
  RK3328 的温度墙是 85℃  

  * 2.3.1 铝合金外壳  

    所有铝合金外壳皆出自 Telegram 群 [NanoPi R2S Club](https://t.me/nanopir2sshell)。  
    感谢群友们为 NanoPi R2S 所做的贡献。  
    现有友善官方外壳发售，类似于本群一代外壳。  

    __购买外壳可加入 Telegram 群 [NanoPi R2S Club](https://t.me/nanopir2sshell)，看群置顶。或 [淘宝链接](https://taoquan.taobao.com/coupon/unify_apply.htm?sellerId=1613492699&activityId=156945e116dc477780f99dedf3ed91b3)，领券下单更优惠。__  

    * 2.3.1.1 外壳区别  

      外壳目前分为一代外壳，二代外壳，二代半外壳和三代外壳。  

      > 一代外壳：为铝合金构造，外表做氧化发黑处理，黑色高级纹理搭配顶部散热鳍片。TF 卡 和 USB 做开孔处理，其中限量版首批 300 件有特殊 Logo。  

      > 二代外壳：为铝合金构造，外表做磨砂氧化处理，保持铝合金原色，去掉顶部鳍片和 TF 卡槽，整体更协调，颜值在线。  

      > 二代半外壳：为铝合金构造，外表做磨砂氧化处理，保持铝合金原色，相比于二代加入重新设计高级 TF 卡槽，插卡取卡更便捷。  

      > 三代外壳：在二代半外壳的基础上，加入 0.91 英寸 OLED 屏幕（详见 2.5 条目），简单安装方法，信息获取更高效。  
      
      > 友善官方外壳：类似于一代外壳，用料变薄，优化 TF 卡口 和 外形设计。Soc 铝柱变短，靠硅脂垫连接 Soc，背后无压片。  

    * 2.3.1.2 安装方法  

      暂略。如有安装困难或温度过高，不明安装方法可加入 Telegram 群 [NanoPi R2S Club](https://t.me/nanopir2sshell)  

    * 2.3.1.3 半导体和风扇  

      半导体制冷片采用珀耳帖效应，安装在铝壳上可显著降低 Soc 温度。  
      注意做好热面散热和温控，否则会损坏制冷片和凝水导致 R2S 主板进液。  
      外壳外加装风扇会进一步降低 Soc 的温度。  
      交流讨论可加入 Telegram 群 [NanoPi R2S Club](https://t.me/nanopir2sshell)  

    * 2.3.1.4 常见问题  

      **Q: 铝合金外壳能降低到多少度？**  
      A: 这个要根据室温和空气流通速度来决定。铝合金外壳的最主要目的是满负载情况下能不碰温度墙，稳定 1608MHz 的 Soc 主频不掉频，全力输出。以以往的经验来说，待机状态下 Soc 温度是室温 +20℃ 以内，弱电箱内 +25℃ 以内。如果过高考虑下安装问题和硅脂接触面积。  

      **Q: 绝缘胶带需要贴吗？**  
      A: 可贴可不贴。  

      **Q: 硅脂溢出到主板上怎么办？**  
      A: 大部分硅脂不导电，没影响。  

      **Q: 板子安装不进去怎么办？**  
      A: R2S的主板是从大主板上裁切下来的，会有一定的公差。用指甲刀锉一下边缘毛边。  

      **Q: 硅脂垫是干什么的？可以不用吗？**  
      A: 硅脂垫是贴主板后侧到铝壳底盖的，主要充当散热器弹簧的用途，给Soc和铝柱一个额外的压力，帮助更好散热。  
      
      **Q: 群内外壳和友善外壳有什么区别？**  
      A: 群内外壳已经众筹到最后一批，卖一个少一个，即将售罄绝版。拥有 OLED 的非凡乐趣和更佳的散热能力。友善外壳靠硅脂垫连接，散热会差一些，但是正常使用和非长时间暴力烤鸡不会碰触温度墙，可正常使用。  
      
      其他问题请先学会善用 [搜索引擎](https://www.google.com/)，尝试自行解决，对于无法自行解决的问题欢迎阅读 [提问的艺术](https://github.com/betaseeker/How-To-Ask-Questions) 后在 Telegram 群 [NanoPi R2S Club](https://t.me/nanopir2sshell) 中提出、讨论。  

  * 2.3.2 风扇外壳  

    无论是亚克力定制的外壳还是原装风扇散热片（裸奔情况下），都可以显著降低待机温度，满足日常使用。但在极端满载情况下，还是会撞温度墙。如有极端需求还需暴力改造。  

  * 2.3.3 其他被动散热  

    无论是原装散热片还是加装其他散热片，只要满足不装外壳，存放于空气强流通处，都能显著降低待机温度。  

---  
* __2.4 USB 网卡__  

  * FriendlyWrt 可编译进 811ac 等 USB 网卡，设置为 N 模式，5GHz。强制频宽 40MHz，信道 40。重启后，可以跑到 100Mbps 左右。  
  * 原生 OpenWrt 可选用 MT7601 等网卡，具体参照固件支持。  

---  
* __2.5 OLED 屏幕__  

  目前 OLED 屏幕分为 “0.96 寸狼牙山五壮士版”，和 “三代铝壳自带 0.91 寸屏幕”。  

  “0.91寸屏幕” 可使用 natelol 大佬的 [luci-app-oled](https://github.com/NateLol/luci-app-oled) 来选择显示内容（部分固件有编译进）  

---  
* __2.6 其他外设__  

  狼牙山五壮士等特殊外设。  

---  
### 3、R2S 主要功能  

* __3.1 路由器__  

  双千兆网口的开发板做路由器很合适。  

  * 3.1.1 主路由  

    主要利用方式。  

  * 3.1.2 单臂路由  

    接 VLAN 交换机，单线复用。参考 IEEE 802.1q 协议等资料。  

  * 3.1.3 “旁路由”  

    极不推荐。  

---  
* __3.2 Linux 服务器和 Docker 容器__  

  支持 Armbian 稳定版 和 Ubuntu 等版本，绝大多数系统下支持 Docker，如 Armbian 的 Docker OpenWrt，OpenWrt 下 Docker Awtrix 等等操作。  
  详见 4.3 和 4.4 条目。  

---  
* __3.3 电视盒子__  

  *<尚未开发>*  

---  
* __3.4 暖手宝__  

  *<皮一下很开心。————Nick Bot>*  

---  
### 4、R2S 主要固件介绍  

* __4.1 原生 OpenWrt 固件__  

  * 4.1.1 原生支持 OpenWrt 固件  

    * 2020 年 7 月 28 日，OpenWrt 官方主线加入了对 NanoPi R2S 的支持，取得了阶段性胜利。感谢为此付出努力的群友们，感谢 [OpenWrt](https://openwrt.org/) 官方和 [CTCGFW](https://github.com/project-openwrt) 项目组，以及世界各地为此努力的朋友们。  
    
    源码地址：[openwrt/openwrt](https://github.com/openwrt/openwrt)  

  * 4.1.2 群友基于原生 OpenWrt 源码的预编译固件

    源于开源社区的努力，使用 OpenWrt 官方源码编译，大部分为自用固件， __有特殊需求请自行编译，不提供技术支持__ ，建议采用已有固件，自行甄选使用。固件分为 EXT4 版和 SquashFS 版，除了启动 SquashFS 的固件一直失败，没有其他解决方案时，才建议使用 EXT4 固件。EXT4 固件不能使用 `firstboot` 等命令，误操作后无法重置系统，下列类原生固件均提供 SquashFS 镜像。  
    
    __固件介绍详见各固件仓库主页的 README 和 Releases。__ 部分超频固件保持 1.45V 电压超频至 1608MHz，发热不变，更高性能。部分固件自带 Soc 调频，不懂不要动。电压表 1GHz 下都相同，不建议频率低于 800MHz，一切问题后果自负。  
    
    有任何奇怪问题 SquashFS 可执行重置，部分固件带了 `fuck` 组件，使用更佳。依然不建议使用备份和升级保留备份。  
      
    QiuSimons（404 大佬）  
    地址：[project-openwrt/R2S-OpenWrt](https://github.com/project-openwrt/R2S-OpenWrt)  
      
    Quintus Chu（502 大佬）  
    地址：[quintus-lab/Openwrt-R2S](https://github.com/quintus-lab/Openwrt-R2S)  
      
    CN_SZTL（高大全版）  
    地址：[Telegram @nanopi\_r2s](https://t.me/nanopi\_r2s)  
    *<包含 18.06 版本>*  
      
    Kane Green  
    地址：[KaneGreen/R2S-OpenWrt](https://github.com/KaneGreen/R2S-OpenWrt)  
      
    Nick Bot  
    地址：[nicksun98/R2S-OpenWrt](https://github.com/nicksun98/R2S-OpenWrt)  
      
    墨子冥  
    地址：[msylgj/R2S-OpenWrt](https://github.com/msylgj/R2S-OpenWrt)  
      
    欢迎各位大佬提供自己的固件地址。谢谢大家为开源社区做出的努力和贡献。  
      
  * 4.1.3 基于 Jayanta525 patch 的原生固件尝试  

    基于 [Jayanta525](https://github.com/jayanta525) 的 patch 编译，是原生支持前的重要阶段。但是有未改好 IRQ 等等问题，使用舒适度和性能完整度落后于 OpenWrt 官方以及群友版本。  
    目前 Lean 的源库是基于此编译。Chuck 采用友善 IRQ 修改版的固件是少数做好优化的固件。  

    Chuck  
    地址：[fanck0605/openwrt-nanopi-r2s](https://github.com/fanck0605/openwrt-nanopi-r2s)  
    *<注意 Branch>*  
    
---  
* __4.2 FriendlyWrt 固件__  
  
  已不推荐使用，内容已转移，[点击查看](https://github.com/nicksun98/R2S-Wiki/blob/master/friendlywrt.md)  
 

---  
* __4.3 Armbian 固件__  

  * 来自 [NanoPi R2S Club](https://t.me/nanopir2sshell) 群主 Darya 的博客  
  
    地址: [e.g.daryasyr.com](https://e.g.daryasyr.com)  

---  
* __4.4 Linux 发行版固件__  

  * 等待 [NanoPi R2S Club](https://t.me/nanopir2sshell) 群主 Darya 补充，敬请期待！  

---  
* __4.5 在线刷机__  

  * NanoPi R2S 支持在线升级 / 降级 / 更改固件。  

  * 4.5.1 FriendlyWrt 到 FriendlyWrt  

    * 部分固件内置有 SongChenwen 大佬的 [luci-app-r2sflasher](https://github.com/songchenwen/nanopi-r2s/tree/master/luci-app-r2sflasher)，可实现图形化刷机。  

    * 或者使用 Gary 大佬的在线刷机脚本：  
    ```  
    wget -q https://github.com/quintus-lab/Openwrt-R2S/raw/master/script/update.sh && sh ./update.sh
    ```  
       *<脚本 3 4 选项的 ardanzhu 固件链接已失效>*  

  * 4.5.2 FriendlyWrt 到 OpenWrt  

    建议使用 dd 命令进行写盘：  

    先将要刷的固件解压到 img，通过 `scp` 命令或者文件上传功能上传到 /tmp/upload/ 文件夹内。  
    通过 SSH 逐步运行下方的代码，将 <openwrt.img> 替换为您上传的固件名称（注意，不要加尖括号）。  
    ```  
    dd if=/tmp/upload/<openwrt.img> of=/dev/mmcblk0 conv=fsync
    reboot
    ```  
  * 4.5.3 OpenWrt 到 OpenWrt  

    也可以使用 dd 命令，不过建议使用内置的 sysupgrade 命令：  

    确保固件是 .img.gz 格式，通过 `scp` 命令上传到 /tmp 文件夹。  
    通过 SSH 逐步运行下方的代码，将 <openwrt.img.gz> 替换为您上传的固件名称（注意，不要加尖括号）。  
    ```  
    sysupgrade -v /tmp/<openwrt.img.gz>
    reboot
    ```  
    等待重启后，重连 SSH，输入：  
    ```  
    firstboot -y && reboot
    ```  

    也可以通过 LuCI 的方式进行更新：  

      LuCI 页面，进入系统-备份/升级，选择刷写新的固件，上传要刷写的固件 .img.gz。  
      核对 MD5 值和 SHA256 值，选择刷写固件，这时您的 R2S 会重启。  
      *（注意！同 RootFS 大小的固件，这里保不保存配置都是保存配置，TF 卡转 emmc 的除外）*  

      等待重启后，重连 SSH，输入：  
      ```  
      firstboot -y && reboot
      ```  

---  
* __4.6 常见问题__  
 
  **Q: 开机后网口灯亮，分配不到 IP / 分配到 169.254.x.x？**  
  A: 确认您刷的固件插入正确的网口，如正确，手动分配子网内 IP，若可以进入后台，恢复到出厂设置（执行重置），进行干净部署。若未解决，进行换网口操作。  

  **Q: 固件开启 BBR 加速了吗？**  
  A: 原生固件默认打开 BBR 加速，友善固件在“网络 - Turbo ACC 加速”打开。5.4 内核的 BBR 已修复万兆掉速 BUG。  

  **Q: Flow 加速 / 软件流量分载 / Shortcut-FE 加速要开吗？**  
  A: Flow Offloading / Shortcut-FE 等建议带宽超过 500Mbps 的用户打开。请勿同时开启，切换使用时建议重启以避免不必要的问题。  

  **Q: HWNAT / 硬件流量分载要开吗？**  
  A: 硬件流量分载仅支持 MTK762x，即使勾选也没有任何意义。  

  **Q: DNS 加速要开吗？**  
  A: 不建议。  

  **Q: Fullcone NAT 哪里开？可以开吗？**  
  A: 在“网络 - 防火墙 - Fullcone”打开，建议打开。找不到的可能是主题存在兼容性问题，可以在防火墙的 WAN 口设置里更改。  
  
  **Q: 类原生固件可以使用 sysupgrade 吗？**  
  A: 可以上传 img.gz 文件在线升级，不建议保留配置。RootFS 相同的固件，不保存配置也是保存配置，TF 卡特性决定，参考 2.2.3 条目。  

  **Q: 建议使用备份吗？**  
  A: 不建议，后果自负。  

  **Q: 为什么更改不了 LAN IP / 管理地址？**  
  A: 这是 OpenWrt 19.07 带来的新 Feature，防止不懂的人乱改。点击保存并应用旁边向下的箭头，点击强制应用，或参考 5.1.1 条目。  

  其他问题请先学会善用 [搜索引擎](https://www.google.com/)，尝试自行解决，对于无法自行解决的问题欢迎阅读 [提问的艺术](https://github.com/betaseeker/How-To-Ask-Questions) 后在 Telegram 群 [NanoPi R2S Club](https://t.me/nanopir2sshell) 中提出、讨论。  

---  
### 5、OpenWrt 使用基本介绍  

* __5.1 SSH 的使用__  

  * Windows 用户：下载 [putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)  
  * Linux / MacOSX 用户：终端输入：  
    ```  
    ssh -p <你的 DropBear 端口号，默认22> root@<你的 R2S 的 IP 地址>
    ```  
  * （注意，去掉尖括号。建议修改端口号为非 22，LAN Only。输入密码不会有任何显示。 OpenWrt 缺省只有 root 一个用户）  
    
  * 5.1.1 SSH 的初步体验  
   
    * 更改 LAN IP（注意，不要加尖括号）
    ```
    uci set network.lan.ipaddr=<新的 LAN 地址>
    uci commit network
    /etc/init.d/network restart
    ```
   
    * 使用 vi / vim 命令 ([vi / vim 命令教程](https://www.runoob.com/linux/linux-vim.html))  
      *换网口 ETH，改 WAN 口、LAN 口配置，更改 MAC 地址等均可通过 network 文件更改，希望大家举一反三，一通百通*  
    ```  
    vim /etc/config/network
    /etc/init.d/network restart
    ```  
   
    * 使用 SSH 查看设备温度  
    ```    
    cat /sys/class/thermal/thermal_zone0/temp
    ```  
    * 使用 SSH 查看设备 Soc 频率  
    ```  
    cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
    ```  
   * 使用 cputemp.sh 脚本查看温度频率  
    
      下载脚本到 /bin 路径下  
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
