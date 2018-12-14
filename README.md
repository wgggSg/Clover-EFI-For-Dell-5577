# Clover-EFI-For-Dell-5577

  从2018年10月1日利用课下时间折腾Hackintosh，断断续续，中间也曾因配置文件不完美放弃折腾投奔ArchLinux，最后还是遵循了真香定律。  
  双十二弃Linux，回归黑果，以前没能解决的问题不知道为什么突然解决了，也就顺其自然地折腾好了属于自己电脑的Clover文件，分享一下。  
  PS：这份EFI配置文件事实上也不全是自己劳动的成果，其中结合了Dell 7520（好像是这个，具体型号忘记了...可能是DELL主板有相似之处吧，这个型号完美EFI的其中一部分文件在我的本子上是可用的）和Dell 5577在osx 10.13下的EFI文件。（感谢前辈们的劳动成果了，谢谢鸭~）
  
    进入GitHub，从这里用到了不少东西，也学到了很多，此次也算是对这个环境的一份贡献吧。

## 本人配置

 硬件类型|型号
 ---- | ----- 
 笔记本型号|Dell 5577 (Inspiron 15pr-5645b)
 CPU|Intel Core i5-7300HQ
 内存|镁光 英睿达 8G 2400Mhz x 2
 SSD|SAMSUNG SM951 512G M.2 NVME
 HDD|西数 1T
 显卡（已屏蔽）|NVIDIA GTX1050 4G
 板载网卡|Realtek RTL8168H/8111H PCI Express Gigabit Ethernet
 无线网卡|DW1560/BCM94352z 802.11ac Wireless Network Adapter
 声卡|Realtek ALC 256
 显示屏|京东方 15.6 IPS 72色域
 
  本人也是先后折腾了不少硬件（也有折腾黑苹果之前换的），所以总体配置也是和笔记本原版有所差别，不过感觉除了网卡（BCM94352z），换的硬件都是对无关紧要的，同型号食用应该问题不大。
