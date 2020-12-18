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

- cuda和cudnn使用yay进行安装，可进行版本选择。

- tensorflow-gpu在conda虚拟环境中使用pip安装，可以避免安装cpu版本而导致首先启用cpu版本。

- 测试tensorflow-gpu版GPU启用的代码。

  ```python
  import tensorflow as tf
  import timeit
  import os
  
  os.environ['TF_CPP_MIN_LOG_LEVEL'] = '2'  # 代码用于忽略级别 2 及以下的消息（级别 1 是提示，级别 2 是警告，级别 3 是错误）。
  
  with tf.device('/cpu:0'):
      cpu_a = tf.random.normal([10000, 1000])
      cpu_b = tf.random.normal([1000, 2000])
      print(cpu_a.device, cpu_b.device)
  
  with tf.device('/gpu:0'):
      gpu_a = tf.random.normal([10000, 1000])
      gpu_b = tf.random.normal([1000, 2000])
      print(gpu_a.device, gpu_b.device)
  
  def cpu_run():
      with tf.device('/cpu:0'):
          c = tf.matmul(cpu_a, cpu_b)
      return c
      
  def gpu_run():
      with tf.device('/gpu:0'):
          c = tf.matmul(gpu_a, gpu_b)
      return c
      
  # warm up
  cpu_time = timeit.timeit(cpu_run, number=10)
  gpu_time = timeit.timeit(gpu_run, number=10)
  print('warmup:', cpu_time, gpu_time)
  
  cpu_time = timeit.timeit(cpu_run, number=10)
  gpu_time = timeit.timeit(gpu_run, number=10)
  print('run time:', cpu_time, gpu_time)
  
  print('GPU', tf.test.is_gpu_available())
  tf.config.list_physical_devices('GPU')
  ```

  输出结果中有GPU True，并且GPU速度相比CPU有明显提升，就说明环境配置成功了。

### 3.安装显卡驱动

- 由于笔记本电脑同时有Nvidia和Intel集成显卡，主板不支持屏蔽集成显卡，因此在manjaro系统下，需要选择支持两个显卡的驱动程序版本，当前最新版本的Nvidia和Intel双显卡驱动已全部更新为prime版本，bumblebee版本不再支持高版本Linux内核。

所使用系统是linux59内核，在安装显卡驱动这里踩了无数坑，还好Manjaro的live CD安装U盘极其好用。

- 由于安装系统时选择了开源驱动，导致了所安装闭源驱动与系统默认开源驱动之间不兼容，导致开机黑屏。
- 各种显卡切换辅助软件和显卡驱动之间不兼容。

最终选择的解决方案。

- 首先卸载所有现有的开源闭源驱动、卸载bumblebee、卸载bbswitch。（非prime版本的原有显卡驱动相关的软件都卸载干净）

  - 驱动用mhwd卸载。卸载前用`mhwd -li`命令查看现安装驱动情况。

    `mhwd -r pci video-linux`

    卸载完成后再用`mhwd -li`命令查看是否卸载干净。

  - bumblebee和bbswitch等用pacman卸载。

- 安装prime版本Nvidia和Intel双显卡驱动。

  - 驱动用mhwd安装。

    `mhwd -i pci video-hybrid-intel-nvidia-455xx-prime`

    安装前用`mhwd`命令查看现在支持安装的驱动。

  - 安装完成后，/etc/modprobe.d文件夹中存在mhwd-gpu.conf文件，其中应有屏蔽nouveau驱动的字段。

- 安装optimus-manager-qt-git版。用yay安装。

  - 这是一个图形化的显卡切换软件，但是这里主要想要使用的其能够直观显示当前的显卡模式，到底是用的Intel集显还是Nvidia独显。

  - 直接使用软件的显卡切换功能效果并不理想，无法完成Nvidia独显的切换。因此通过修改配置文件，实现开机采用独显的效果，路径为 /etc/optimus-manager/optimus-manager.conf

    修改其中的startup-mode项使其为nvidia。

- 使用`inix -G`命令查看当前显卡设备情况，如果Nvidia显卡启用则此时的nvidia-smi命令和nvidia-settings命令应已有效。可重新启动看能否正常进入桌面。

### 4.利用Live CD处理异常情况

如果安装显卡驱动后重启出现黑屏或者卡Logo等情况进不去系统，不要惊慌，利用Live CD的安装U盘可以完成异常处理。

- BIOS设置U盘启动，进入Live CD系统。类似Windows的PE系统。但功能更强大。主要体现在可以挂载原系统，进行安装和删除软件，可以上网，并且可以利用硬盘原系统上网。

- 打开终端，用`manjaro-chroot -a`命令查看硬盘中manjaro系统名称，输入编号后回车，编号从1开始。将自动将硬盘中系统挂载。
- 利用mhwd和pacman处理异常，先清空再重装等操作均可。
- 可利用`pacman -S linuxXX`命令，重新安装内核，XX为内核版本号。
- 可利用`update-grub`命令重建引导。

enjoy coding！