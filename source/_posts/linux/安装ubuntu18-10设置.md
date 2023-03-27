---
title: 安装ubuntu18.10设置
date: 2020-11-10 22:26:03
tags:
---

1.宿主机访问虚拟机Web服务
> 设置桥接网络
```sh
$ sudo apt install net-tools
$ ifconfig
```

2.调节滚轮速度
```sh
$ sudo apt install imwheel
$ sudo vim ~/.imwheelrc

".*"
None,      Up,   Button4, 5
None,      Down, Button5, 5
Control_L, Up,   Control_L|Button4
Control_L, Down, Control_L|Button5
Shift_L,   Up,   Shift_L|Button4
Shift_L,   Down, Shift_L|Button5

$ sudo imwheel
```

3.安装zsh
```sh
$ sudo apt install zsh
$ wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
$ export ZSH_CUSTOM=/home/weiwenhao/.oh-my-zsh
$ cd $ZSH_CUSTOM/plugins
$ git clone git://github.com/zsh-users/zsh-autosuggestions
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
$ sudo apt install :q!
$ sudo vim ~/.zshrc

# 在plugins中添加
plugins=(
  git
  autojump
  zsh-autosuggestions
  zsh-syntax-highlighting
)

# 在末尾添加
[[ -s /etc/profile.d/autojump.sh ]] && . /etc/profile.d/autojump.sh

$ source ~/.zshrc
```

4.在宿主机和虚拟机之间复制粘贴/拖动
> 设置共享文件夹 /mnt/hgfs/
```sh
$ sudo apt autoremove open-vm-tools
$ sudo apt install open-vm-tools
$ sudo apt install open-vm-tools-dkms
$ sudo apt install open-vm-tools-desktop
```

5.安装 VMware Tools
```sh
$ tar xzvf VMwareTools.tar.gz
$ sudo ./vmware-install.pl -d

# 卸载
$ sudo ./bin/vmware-uninstall.pl -d
```

6.安装环境
```sh
$ sudo apt install gcc -y
$ sudo apt install g++ -y
$ sudo apt install make -y
$ sudo apt install vim -y
$ sudo apt install unrar -y
```

7.设置环境变量
> /etc/.bashrc /etc/profile
```sh
$ sudo vim vi /etc/profile

PATH=THE_SET_GLOBLE_ENV_PATH:$PATH;
export PATH

$ source /etc/profile
```

8.安装显卡驱动
> ps: NVIDIA 2070 nvidia-418
> https://www.nvidia.cn/Download/driverResults.aspx/145265/cn
```sh
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
$ sudo apt install nvidia-418
$ sudo apt install mesa-common-dev
$ sudo apt install freeglut3-dev

$ nvidia-smi
$ nvidia-settings
```

9.清理包
```sh
# 清理旧版本的软件缓存
$ sudo apt autoclean

# 清理所有软件缓存
$ sudo apt clean

# 删除系统不再使用的孤立软件
$ sudo apt autoremove
```

10.删除软件
```sh
# 删除libreoffice
$ sudo apt remove libreoffice-common

# 删除Amazon
$ sudo apt remove unity-webapps-common

# 删除一些不常用的软件
$ sudo apt remove thunderbird totem rhythmbox empathy brasero simple-scan gnome-mahjongg aisleriot gnome-mines cheese transmission-common gnome-orca webbrowser-app gnome-sudoku

$ sudo apt remove onboard deja-dup
```

11.更新
```sh
$ sudo apt update
$ sudo apt upgrade
```

12.自定义分辨率
```sh
$ sudo vim /etc/X11/xorg.conf
粘贴以下内容：
Section "Monitor"
Identifier "Configured Monitor"
Modeline "2560x1440_60.00"  312.25  2560 2752 3024 3488  1440 1443 1448 1493 -hsync +vsync
Option "PreferredMode" "2560x1440_60.00"
EndSection
Section "Screen"
Identifier "Default Screen"
Monitor "Configured Monitor"
Device "Configured Video Device"
EndSection
Section "Device"
Identifier "Configured Video Device"
EndSection

$ sudo rm /home/weiwenhao/.config/monitors.xml
$ reboot
```
