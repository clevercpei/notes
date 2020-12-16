# Manjaro Linux 配置 tensorflow-gpu环境

## 硬件情况

### 1.笔记本电脑

![](/home/peterchen/Pictures/Screenshots/system_info.png)

### 2.选择软件版本

- Python 3.8.6
- conda 4.9.2
- pip 20.2
- cuda 11.0
- cudnn 8.0
- tensorflow-gpu 2.4.0
- Nvidia驱动版本455.45.01

注意：显卡驱动版本、tensorflow-gpu版本、cuda和cudnn版本一定要对应起来，否则会出现由于库缺失而导致的tensorflow找不到gpu的情况。对应情况可以google。

### 3.安装显卡驱动

- 由于笔记本电脑同时有Nvidia和Intel集成显卡，主板不支持屏蔽集成显卡，因此在manjaro系统下，需要选择支持两个显卡的驱动程序版本，当前最新版本的Nvidia和Intel双显卡驱动已全部更新为prime版本，bumblebee版本不再支持高版本Linux内核。

所使用系统是linux59内核，在安装显卡驱动这里踩了无数坑，还好Manjaro的live CD安装U盘极其好用。

- 由于安装系统时选择了开源驱动，导致了所安装闭源驱动与系统默认开源驱动之间不兼容，导致开机黑屏。
- 各种显卡切换辅助软件和显卡驱动之间不兼容。

最终选择的解决方案。

- 首先卸载所有现有的开源闭源驱动、卸载bumblebee。