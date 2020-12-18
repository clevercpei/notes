# 当前内核

uname -a

# 显卡驱动状态

inxi -G

# PCI 驱动

mhwd

# grub

update-grub

# 内核管理

mhwd-kernel

# conda 换源

conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --set show_channel_urls yes

sudo conda update conda
sudo conda update anaconda
conda update --all
conda activate name
conda deactivate

# 清理系统

sudo pacman -R $(pacman -Qdtq)
sudo pacman -Scc
sudo journalctl --vacuum-size=50M
sudo rm /var/lib/systemd/coredump/\*

# 如果开始菜单中所有程序一直闪烁

sudo rm ~/.config/mimeapps.list ~/.config/mimeapps.list_old

# 软链接 vim 和 neovim 配置文件

ln -s ~/.vim ~/.config/nvim
ln -s ~/.vim/vimrc ~/.config/nvim/init.vim

# yay 换源

yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
yay -P -g

# 清理 yay 缓存

rm -rf ~/.cache/yay
