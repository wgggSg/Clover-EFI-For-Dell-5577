# Clover-EFI-For-Dell-5577

  从2018年10月1日利用课下时间折腾Hackintosh，断断续续，中间也曾因  配置文件不完美放弃折腾投奔*ArchLinux*，最后还是遵循了真香定律（手动滑稽）  
  双十二弃坑Linux，回归黑果，以前没能解决的问题不知道为什么突然解决了，也就顺其自然地折腾好了属于自己电脑的Clover文件，分享一下。  
  ### 这份配置文件结合了很多前辈的经验和教程，网友们太厉害了！  
  * ***daliansky***大佬制作的`教程`,`EFI`,`P-little`,`OC-little`
  * Dell 7520（好像是这个，具体型号忘记了...）  
       可能是DELL主板有相似之处吧，这个型号完美EFI的其中一部分文件在我的本子上是可用的  
  * Dell 5577在*osx 10.13*下的EFI文件。   
  
  *（感谢前辈们的劳动成果了，谢谢鸭~）*
  
    进入GitHub，从这里用到了不少网友们的劳动成果，也学到了很多实用的知识，我也想分享一下我的学习成果，希望自己能够对这个环境贡献一下:)

## 配置及改动

 硬件类型|型号
 ---- | ----- 
 笔记本型号|Dell 5577 (Inspiron 15pr-5645b)
 CPU|Intel Core i5-7300HQ（HD630）
 内存（扩容）|镁光 英睿达 8G 2400Mhz x 2
 SSD（更换）|SAMSUNG SM951 512G M.2 NVME
 HDD|西数 1T
 独显（***屏蔽***）|NVIDIA GTX1050 4G
 板载网卡|Realtek RTL8168H/8111H PCI Express Gigabit Ethernet
 无线网卡（更换）|DW1560/BCM94352z 802.11ac Wireless Network Adapter
 声卡|Realtek ALC 256
 显示屏（更换）|京东方 15.6 IPS 72色域
 
  我给电脑先后更换了不少硬件，一些硬件和DELL5577原配置有所差别，不过感觉除了网卡（***BCM94352z***），换的硬件都是对无关紧要的，同型号食用应该问题不大。

## 日志

### 2020.05.15 macOS 10.15.4 **再次重构引导文件**
这几天为了学习（脸红）又装上了黑苹果，发现10.15.4已经装不上了......于是又开始折腾了一下，发现以前的结构太乱了，于是决定重构！腾出了几天晚上的宝（游）贵（戏）的时光，经过了不懈的研究，终于基本完成！！继续分享一下～
#### 再次重构引导文件
* 重构全部/ACPI内容，全部采用hotpatch方式修补DSDT（主要工作）
  * 电池
  * 调节亮度
  * 键盘快捷键 *降低亮度* `Fn`+`F11`, *增加亮度* `Fn`+`F12`, *休眠* `Fn`+`Insert` 等
  * 触控板修正 **改变之前低效的轮询驱动方式，采用中断驱动方式，太丝滑了！！**   
  *弄完了触摸板我真的，成 就 感 爆棚！！！太 爽 了*
  * x86电源管理（CPU）
* 删除了很多config.plist中的冗余选项（config.plist）及冗余文件（/driver,/kext...）
* 更新Clover及kext驱动
  * `Clover Release v5.0 r5117`
  * `AirportBrcmFixup v2.0.7`
  * `AppleALC v1.5.0`
  * `Lilu v1.4.4`
  * `WhateverGreen v1.3.9`  
  ······
* 耳机问题（在家里用不太到，暂时没解决）  

          在疫情期间，在网课和学习计划交叉的百忙之中腾出休息时间来折腾黑苹果真是太累了，但是折腾完之后却感觉特别 爽 ，这就是黑苹果带给我的快乐吧，嘿嘿
### 2020.01.08 小修复
* 修复Catalina（10.15）DW1560蓝牙失效问题<br>
  * `BrcmPatchRAM2.kext` -> `BrcmPatchRAM3.kext`<br>
  * 添加 `BrcmBluetoothInjector.kext`<br>

            由于我升级到10.15版本之后没有再退回到10.14尝试，所以如果使用Mojave（10.14）版本，
            可能需要删除 BrcmBluetoothInjector.kext 并将 BrcmPatchRAM3.kext 替换为 BrcmPatchRAM2.kext（位于/CLOVER/kexts/Other/off文件夹内）
* 关机速度恢复

### 2019.10.13 支持***Catalina***
#### 更新内容
* 可更新至Catalina
* 更新Clover至v2.5k r5096
* 更新部分驱动`lilu v1.3.8` `AppleALC v1.4.2` `WhatEverGreen v1.3.2`
* 删除部分无用文件

#### Catalina
![Catalina 2019-10-12 PM10.58.21](https://raw.githubusercontent.com/wgggSg/Clover-EFI-For-Dell-5577/master/ScreenSnap/Catalina%202019-10-12%20PM10.58.21.jpg)
* 大部分功能正常
* `BCM94352z`Wi-Fi开机会卡一段时间才可用，蓝牙不可用
* 关机速度变慢
* 其余问题有待发现

### 2019.4.24 **日志**
#### 音频插口声音解决方案！！！
**步骤：**
* 解压 *TOOLS.zip* 压缩包
* 一： `EFI/CLOVER/kexts/other/` 中存放 **CodecCommander.kext** 文件 (*TOOLS/* 下) ，插入耳机**播放音乐**，执行 *install安装.command* 文件 (`TOOLS/ALCPlugFix/` 下) ，重启
* 二： 执行 *uninstall卸载.command* 文件，删除 **CodecCommander.kext** 文件，将 **VerbStub.kext** 文件 (*TOOLS/ComboJack_Installer* 下) 放到 *EFI/CLOVER/kexts/other/* 下，执行 **install.sh**，重启即可

* （感谢一起吃苹果的群友分享！！）

      刚开始我很困惑为什么ALCPlugFix及CodecCommander.kext要安装后再卸载，于是我看了一下install和uninstall的代码，发现原来是为了在/usr/bin中保留hda-verb文件，而在ComboJack_Installer中也有hda-verb文件，所以我觉得第一步是可以简化的，即：
      sudo cp -a "$path/hda-verb" /usr/bin
      sudo chmod 755 /usr/bin/hda-verb
      sudo chown root:wheel /usr/bin/hda-verb
      但是原步骤中指出必须先安装CodecCommander.kext，怕又是一个无比深的坑，再加上懒癌晚期（雾），我也没有继续再折腾哈哈哈哈哈，有时间的小伙伴有兴趣可以弄一下啊，或者可能有细节我没注意到，希望大佬指正！！！
      2019.10.13:实践证明是我想多了qwq...一定要严格按照步骤执行！！

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
  从2018年10月1日利用课下时间折腾Hackintosh，断断续续，中间也曾因  配置文件不完美放弃折腾投奔*ArchLinux*，最后还是遵循了真香定律（手动滑稽） 
  双十二弃Linux，回归黑果，以前没能解决的问题不知道为什么突然解决了，也就顺其自然地折腾好了属于自己电脑的Clover文件，分享一下。  
  #### 这份EFI配置文件不全是自己的劳动成果，其中结合了很多前辈的Clover配置文件  
  * **daliansky**大佬制作的EFI配置文件及教程
  * Dell 7520（好像是这个，具体型号忘记了...）  
       可能是DELL主板有相似之处吧，这个型号完美EFI的其中一部分文件在我的本子上是可用的  
  * Dell 5577在*osx 10.13*下的EFI文件。   
  
  *（感谢前辈们的劳动成果了，谢谢鸭~）*
  #### 特性（存在的问题）
* 开机到苹果标志进度条在头部偶尔闪退，再开一次机才可完美读条
* 键盘出现过一次睡眠后唤醒后不能用的情况（USB外接键盘可用，猜测是PS2的问题），重启就好了，但是再也没出现过就没再管
* 系统自带菜单栏电池百分比不会自动更新，用第三方软件却可以更新；偶尔开机需手动打开菜单栏电池显示
* 刚开机耳机插孔不能用，需睡眠唤醒才可用
* 键盘FN仍不能调节亮度（在系统快捷键中手动设置快捷键可调节）

         进入GitHub，从这里用到了不少网友们的劳动成果，也学到了很多实用的知识，我也想分享一下我的学习成果，希望自己能够对这个环境贡献一下:)