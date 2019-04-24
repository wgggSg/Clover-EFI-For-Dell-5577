# Clover-EFI-For-Dell-5577

  从2018年10月1日利用课下时间折腾Hackintosh，断断续续，中间也曾因配置文件不完美放弃折腾投奔*ArchLinux*，最后还是遵循了真香定律（手动滑稽）。  
  双十二弃Linux，回归黑果，以前没能解决的问题不知道为什么突然解决了，也就顺其自然地折腾好了属于自己电脑的Clover文件，分享一下。  
  ### 这份EFI配置文件不全是自己的劳动成果，其中结合了很多前辈的Clover配置文件  
  * 黑果小兵大大 **macOS Mojave 10.14.2 18C54 正式版 with Clover 4792原版镜像** 中含有的EFI配置文件 
  * Dell 7520（好像是这个，具体型号忘记了...）  
       可能是DELL主板有相似之处吧，这个型号完美EFI的其中一部分文件在我的本子上是可用的  
  * Dell 5577在*osx 10.13*下的EFI文件。   
  
  *（感谢前辈们的劳动成果了，谢谢鸭~）*
  
    进入GitHub，从这里用到了不少东西，也学到了很多，此次也算是对这个环境的一份贡献吧。

## 本人配置

 硬件类型|型号
 ---- | ----- 
 笔记本型号|Dell 5577 (Inspiron 15pr-5645b)
 CPU|Intel Core i5-7300HQ
 内存|镁光 英睿达 8G 2400Mhz x 2
 SSD|SAMSUNG SM951 512G M.2 NVME
 HDD|西数 1T
 显卡（***已屏蔽***）|NVIDIA GTX1050 4G
 板载网卡|Realtek RTL8168H/8111H PCI Express Gigabit Ethernet
 无线网卡|DW1560/BCM94352z 802.11ac Wireless Network Adapter
 声卡|Realtek ALC 256
 显示屏|京东方 15.6 IPS 72色域
 
  本人也是先后折腾了不少硬件（也有折腾黑苹果之前换的），所以总体配置也是和笔记本原版配置有所差别，不过感觉除了网卡（***BCM94352z***），换的硬件都是对无关紧要的，同型号食用应该问题不大。

## 日志
### 2019.4.24
#### 音频插口声音解决方案！！！
**步骤：**
* 解压 *TOOLS.zip* 压缩包
* 一： *EFI/CLOVER/kexts/other/* 中存放 **CodecCommander.kext** 文件 (*TOOLS/* 下) ，插入耳机**播放音乐**，执行 *install安装.command* 文件 (*TOOLS/ALCPlugFix/* 下) ，重启
* 二： 执行 *uninstall卸载.command* 文件，删除 **CodecCommander.kext** 文件，将 **VerbStub.kext** 文件 (*TOOLS/ComboJack_Installer* 下) 放到 *EFI/CLOVER/kexts/other/* 下，执行 **install.sh**，重启即可

* （感谢一起吃苹果的群友分享！！）

      刚开始我很困惑为什么ALCPlugFix及CodecCommander.kext要安装后再卸载，于是我看了一下install和uninstall的代码，发现原来是为了在/usr/bin中保留hda-verb文件，而在ComboJack_Installer中也有hda-verb文件，所以我觉得第一步是可以简化的，即：
      sudo cp -a "$path/hda-verb" /usr/bin
      sudo chmod 755 /usr/bin/hda-verb
      sudo chown root:wheel /usr/bin/hda-verb
      但是原步骤中指出必须先安装CodecCommander.kext，怕又是一个无比深的坑，再加上懒癌晚期（雾），我也没有继续再折腾哈哈哈哈哈，有时间的小伙伴有兴趣可以弄一下啊，或者可能有细节我没注意到，希望大佬指正！！！

### 2019.3.26
* **更新Clover4903及部分驱动版本**

### 2019.2.9 上传 **全新EFI**
#### 更新内容
* **弃用旧的EFI，全面更新**
* **修复HDMI**
使用FB注入59160000，可外接显示器（可惜不能输出音频）
* **修复不定期闪退，修复开机偶尔闪退**
在折腾了半年黑果后，对系统各个方面的理解加深了很多，这次配置了全新EFI，各个驱动及配置各方面兼容性更好了，系统更加稳定了，所以很多奇怪的问题也不会出现了
* **电池百分比可以自动更新了**
* **FN键小太阳可以完美调节亮度了**
* CPU **14档变频**
#### 仍然存在的问题
* **耳机可以识别，可以调音量，** 但是没声音.....QwQ
* **还是声音的问题，HDMI声音不可输出**
* 其他问题有待发现


### 2018.11.14 上传
* 开机到苹果标志进度条在头部偶尔闪退，再开一次机才可完美读条
* 键盘出现过一次睡眠后唤醒后不能用的情况（USB外接键盘可用，猜测是PS2的问题），重启就好了，但是再也没出现过就没再管
* 系统自带菜单栏电池百分比不会自动更新，用第三方软件却可以更新；偶尔开机需手动打开菜单栏电池显示
* 刚开机耳机插孔不能用，需睡眠唤醒才可用
* 键盘FN仍不能调节亮度（在系统快捷键中手动设置快捷键可调节）

      自我认为不存在完美的黑苹果，无法判断我的Clover配置文件完美程度，所以没用 xx%完美黑苹果 来写简介。
