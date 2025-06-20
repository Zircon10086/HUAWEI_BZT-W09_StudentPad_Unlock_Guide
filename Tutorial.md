# BZT-W09 平板刷机教程

## 0. 基本原理

使用麒麟处理器的设备独有一个工厂测试模式，即 VCOM 模式，在这种模式下，我们可以上传任何 xloader 和 fastboot 而无需检查。  
我们可以短接主板中的测试点进入 VCOM 模式。  
对于麒麟970、k710、710a、980、990，有研究报告了这一点，因此，华为在推送这些芯片的 OTA 更新时禁用了 VCOM 模式。  
而其他麒麟处理器没有收到更新，没有受到影响。  
BZT-W09 使用麒麟659处理器。


---

## 请到[Release](https://github.com/Zircon10086/HUAWEI_BZT-W09_StudentPad_Unlock_Guide/releases)下载工具包，解压好备用

---

## 1. 安装测试点驱动程序

### 1.1 打开 TestPointDriver 文件，运行 DriverSetup，安装完成后重启电脑，确保驱动生效。

---

## 2. 拆开平板 & 短接

### 2.1 拆解说明

建议从平板上方塑料天线起手，插入薄片后前后滑动

![起手处](Images/Teardown1.jpg)

滑动到边角时，先将翘片（图中使用了美工刀片）垂直插入缝隙，再缓慢撬开边缘。  

![翘边角](Images/Teardown2.jpg)

注意动作要轻缓，屏幕玻璃很薄，易碎。  
屏幕脱离设备后，先翻到右侧面，将接口左侧的压片抬起，再取下屏幕排线，最后取下屏幕。  
注意不要弯折屏幕排线，否则会损坏排线导致触控等问题。

![注意排线](Images/Teardown3.jpg)

![拆排线](Images/Teardown4.jpg)


### 2.2 短接操作

使用镊子或者剪刀将如图所示的测试点和金属屏蔽罩短接，保持短接的情况下，用数据线连接电脑。  

![用导电物连接](Images/TestPoint.jpg)

如果驱动安装正确，在设备管理器中“端口（COM和LPT）”下会出现 “HUAWEI USB COM 1.0”，  
若已连接但驱动未正确安装则会显示 “USB SER”。

### 2.3 重新装回屏幕排线

参照 2.1 重新装回屏幕排线。

---

## 3. 解锁 BootLoader

### 3.1 使用 potatoNV 解锁

启动 potatoNV，Target device 选择 HUAWEI USB COM 1.0，Bootloader 选择 Kirin 65x(B)。两个复选框都不要选择。  
点击 START，如一切顺利会出现如下图所示的界面，最后一行是设备新的 BL 解锁码，复制解锁码。

### 3.2 运行解锁命令

打开 adb&hs2t 文件夹，运行 cmd.exe，输入：

```bash

adb fastboot oem unlock [解锁码]

```

### 3.3 屏幕确认解锁

此时平板屏幕会亮起，询问是否解锁 BootLoader，点按上音量键选择 Yes，点按电源键确认。  
如果屏幕没有显示任何内容，就先点按上音量键，再点按电源键（盲操作）。

### 3.4 自动重启

此时平板会自动重启，进入格式化界面。此时平板应当提示格式化失败，这是正常的，重启即可。

---

## 4. 刷机

### 4.1 再次短接，运行 PotatoNV

#### 4.1.1 （可选）备份分区

你可以使用 hs2t 对设备分区进行备份，请参考 README 提到的 hs2t 项目。

### 4.2 运行刷机脚本

打开 “BZT-W09_update_BZT-W09_all_cn.app最简” 文件夹，运行 0flash最简.bat

此程序仅仅会刷入 Data 和 Version 分区。  
如果你希望继续刷写其他的分区，请运行 “BZT-W09_UPDATE.APP” 内的 0flash.bat

**注意**: 刷写分区存在风险，对于当前设备，仅仅刷入 Data 和 Version 分区就足够了。

---

## 5. (可选)获取 Root、刷入 TWRP

### 5.1 获取 Root

短接设备，运行 PotatoNV，使用指令：

```bash

adb fastboot flash ramdisk [magisk8文件路径]

```

开机后安装 Magisk，会提示需要修复环境，按照指示修复重启后，即可取得 Root 权限。

### 5.2 刷入 TWRP

短接设备，运行 PotatoNV，使用指令：

```bash

adb fastboot flash recovery_ramdisk [twrp8文件路径]

```

**注意**: TWRP 并非针对 C5 开发，因此请勿使用该 TWRP 执行安装系统包、恢复出厂设置等操作。

---

## 6. 大功告成！
