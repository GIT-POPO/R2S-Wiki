  FriendlyWrt 固件基于 [友善官方源码](https://wiki.friendlyarm.com/wiki/index.php/How_to_Build_FriendlyWrt/zh) 和 开源社区 的努力。来自 Armbian 内核 和 OpenWrt 主线源码 杂交而成。存在防火墙阻断 IPv4 流量，WAN 口不能重播，Crontab 等各种问题，可以靠脚本守护。  

 * 1、友善官方固件  

    * 官方固件，[下载地址](http://download.friendlyarm.com/nanopir2s)  

  * 1.2、来自开源社区的友善固件  

      Dayong  
      地址：[klever1988/nanopi-openwrt](https://github.com/klever1988/nanopi-openwrt)  
      
      Chuck（已弃）  
      地址：[fanck0605/nanopi-r2s](https://github.com/fanck0605/nanopi-r2s)  
      
      Song Chenwen  
      地址：[songchenwen/nanopi-r2s](https://github.com/songchenwen/nanopi-r2s)  
      
      RT（过于古早）  
      地址：[Telegram @R2Spasswall](https://t.me/R2Spasswall)  
      
      eSir（过于古早）  
      地址：https://drive.google.com/drive/folders/1DPy8lXhg2btbHsTvpUGVZF2k1rmT7DXA  

 * 2、在线升级固件  

    * 部分固件内置有 SongChenwen 大佬的 [luci-app-r2sflasher](https://github.com/songchenwen/nanopi-r2s/tree/master/luci-app-r2sflasher)，可实现图形化刷机。  

    * 或者使用 Gary 大佬的在线刷机脚本：  
    ```  
    wget -q https://github.com/quintus-lab/Openwrt-R2S/raw/master/script/update.sh && sh ./update.sh
    ```  
       *<脚本 3 4 选项的 ardanzhu 固件链接已失效>*  
