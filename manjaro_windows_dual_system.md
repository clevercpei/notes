# Manjaro 和 Windows双系统

### 1。更新Manjaro后，启动项找不到Windows，直接进入更新版本的Manjaro

- 首先修改/etc/default/grub文件，加入

`GRUB_DISABLE_OS_PROBER=xtrue`

![](/home/peterchen/Pictures/Screenshots/grub_setting.png)

- 执行命令  `sudo grub-mkconfig`
- 执行命令  `sudo update-grub`
- 重新启动

