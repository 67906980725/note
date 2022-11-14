# arch gnome init

## archinstall

*如果是同一个硬盘上和其他系统共存先分好区
archinstall 的根据扇区计算大小方式猜不透
现场计算配完最后安装老报错 size 是负的
所以索性先分好区，让archinstall用现成的

*在其他系统上习惯 /boot 和 /boot/efi 分离, /boot/efi 做 esp 分区
archinstall 这样似乎行不通
好在 esp 分区是可以有多个的，
不需要和  win 共用一个 esp 分区
所以有个 /boot 分区就可以了


``` plaintext
进去先 `reflector -c China --sort rate --save /etc/pacman.d/mirrorlist`
没有配置文件时:
执行 archinstall 开始配置
Mirror region: ['China']
Locale Language: zh_CN.UTF-8
Locale encoding: UTF-8
Drive(s): 目 标硬盘
Disk layout:
    手动分区的情况:
  Assign mount-point for a partition: 至少要有 /boot 和 / 两个分区
  Mark/Unmark a partition to be formatted (wipes data): 'Assign mount-point for a partition' 涉及到的分区
  Mark/Unmark a partition as encrypted: 如 / 分区等 用 btrfs 格式时可以加密, /boot 不可以
  Mark/Unmark a partition as bootable (automatic for /boot): /boot 挂载的分区
  Mark/Unmark a partition as compressed (btrfs only): 标记一个分区为压缩(只能 btrfs), 这个不熟
  Set desired filesystem for a partition: 为分区设置所需的文件系统, /boot: fat32, /:btrfs
Bootloader: systemd-bootctl (默认, 简单轻便一些)
Swap: False
Hostname: dong_fang_yao_de_jian
Root password: kan_jian_!
User account: dong_fang_yao add完user记得superuser要选yes, 默认是no
Profile: desktop-gnome 
    Select a graphics driver or leave blank to install all open-source drivers : All open-source (默认) 或 Nvidia (proprietary) 或  AMD / ATI (open-source)
Audio: None 不用选就行
Kernels: ['linux'] 不用动
Additional packages: ['wqy-microhei'] 提前装个中文字体不然装完一开机全是口口
Network configuration: Use NetworkManager 桌面是gnome/kde的话网络配置要选NetworkManager
Timezone: Asia/Shanghai
Automatic time sync (NTP) : True
Optional repositories: ['multilib']
Save configuration 保存到本地以后就不用每次都设置半天了  

*保存后检查下 user_disk_layout.json 里相关分区 wipe 属性是不是  true, 
    有次发现标记了 wipe 但生成的配置里是 false 导致失败的(吐槽一下分区处理是真慢）
*主菜单的选项设置完会自动定位到下一个菜单上，一不小心就可能漏配置

有配置文件时
archinstall --config user_configuration.json --creds user_credentials.json --disk_layouts user_disk_layout.json


贴上自己的配置:

user_configuration.json:
{
    "additional-repositories": [
        "multilib"
    ],
    "config_version": "2.5.1",
    "debug": false,
    "desktop-environment": "gnome",
    "gfx_driver": "Nvidia (proprietary)",
    "harddrives": [
        "/dev/sdb"
    ],
    "hostname": "dong_fang_yao_de_jian",
    "mirror-region": {
        "China": {
            "http://mirror.lzu.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirror.redrock.team/archlinux/$repo/os/$arch": true,
            "http://mirrors.163.com/archlinux/$repo/os/$arch": true,
            "http://mirrors.aliyun.com/archlinux/$repo/os/$arch": true,
            "http://mirrors.bfsu.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.cqu.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.hit.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.neusoft.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.nju.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.shanghaitech.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.wsyu.edu.cn/archlinux/$repo/os/$arch": true,
            "http://mirrors.zju.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirror.redrock.team/archlinux/$repo/os/$arch": true,
            "https://mirrors.aliyun.com/archlinux/$repo/os/$arch": true,
            "https://mirrors.bfsu.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.cqu.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.hit.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.neusoft.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.nju.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.njupt.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.shanghaitech.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.sjtug.sjtu.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.wsyu.edu.cn/archlinux/$repo/os/$arch": true,
            "https://mirrors.xjtu.edu.cn/archlinux/$repo/os/$arch": true
        }
    },
    "nic": {
        "dhcp": true,
        "dns": null,
        "gateway": null,
        "iface": null,
        "ip": null,
        "type": "nm"
    },
    "packages": [
        "wqy-microhei"
    ],
    "no_pkg_lookups": false,
    "offline": false,
    "profile": {
        "path": "/usr/lib/python3.10/site-packages/archinstall/profiles/desktop.py"
    },
    "script": "guided",
    "silent": false,
    "swap": false,
    "sys-encoding": "UTF-8",
    "sys-language": "zh_CN.UTF-8",
    "timezone": "Asia/Shanghai",
    "version": "2.5.1"
}

user_credentials.json:
{
    "!encryption-password": "kan_jian_!",
    "!users": [
        {
            "!password": "kan_jian_!",
            "sudo": true,
            "username": "dong_fang_yao"
        }
    ]
}

user_disk_layout.json:
{
    "/dev/sdb": {
        "partitions": [
            {
                "ESP": false,
                "PARTUUID": "2c2536ee-6172-484e-a68e-38f11056aa01",
                "boot": false,
                "encrypted": false,
                "filesystem": {
                    "format": "vfat"
                },
                "mountpoint": null,
                "size": 204800,
                "start": 2048,
                "type": "primary",
                "wipe": false
            },
            {
                "ESP": false,
                "PARTUUID": "96b1fcae-d0de-45da-8541-0d7adc602480",
                "boot": false,
                "encrypted": false,
                "filesystem": {
                    "format": null
                },
                "mountpoint": null,
                "size": 32768,
                "start": 206848,
                "type": "primary",
                "wipe": false
            },
            {
                "ESP": false,
                "PARTUUID": "96b88dc5-4468-408d-917b-2237321630bd",
                "boot": false,
                "encrypted": false,
                "filesystem": {
                    "format": "ntfs"
                },
                "mountpoint": null,
                "size": 243473220,
                "start": 239616,
                "type": "primary",
                "wipe": false
            },
            {
                "ESP": false,
                "PARTUUID": "f5ca95cc-da9c-4d25-a2d8-3595b30d4192",
                "boot": false,
                "encrypted": false,
                "filesystem": {
                    "format": "ntfs"
                },
                "mountpoint": null,
                "size": 1210368,
                "start": 487184384,
                "type": "primary",
                "wipe": false
            },
            {
                "ESP": true,
                "PARTUUID": "545ca91b-203f-4389-9654-e08bab0d1e6d",
                "boot": true,
                "encrypted": false,
                "filesystem": {
                    "format": "vfat"
                },
                "mountpoint": "/boot",
                "size": 1953792,
                "start": 243714048,
                "type": "primary",
                "wipe": true
            },
            {
                "ESP": false,
                "PARTUUID": "0b8f0f35-0387-4ff0-a7f0-8bc1542b3b05",
                "boot": false,
                "encrypted": true,
                "filesystem": {
                    "format": "btrfs",
                    "mount_options": [
                        "compress=zstd"
                    ]
                },
                "mountpoint": "/",
                "size": 241516544,
                "start": 245667840,
                "type": "primary",
                "wipe": true
            }
        ]
    }
}

```

todo 使用中发现在 livecd 中 gnome wifi 设置中开热点可以正常用， 但安装完后会连不上（arch&fedora）: activation of network connection failed

## 安装完开机后

``` shell
#sudo pacman -S wqy-microhei
#sudo reboot

# 源
sudo pacman -S reflector rsync
echo "
-c China
--threads 16
" | sudo tee -a /etc/xdg/reflector/reflector.conf
sudo reflector --verbose -c China --latest 12 --sort rate --threads 16 --save /etc/pacman.d/mirrorlist 
sudo cp /etc/pacman.conf /etc/pacman.conf.bak
echo '
[archlinuxcn]
# 清华
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
# 官方
#Server = http://repo.archlinuxcn.org/$arch
# 163
#Server = http://mirrors.163.com/archlinux-cn/$arch
' | sudo tee -a /etc/pacman.conf
# multilib
#sudo sed '/#\[multilib\]/,+1 s/^#//' -i /etc/pacman.conf 
sudo pacman -Syy

# keyring
sudo pacman -S archlinuxcn-keyring
# 如果装完很多类似下边的错
# ERROR: 4B1DE545A801D4549BFD3FEF90CB3D62C13D4796 could not be locally signed
# 先检查时钟是否正确
# timedatectl status
# 然后
# sudo rm -rf /etc/pacman.d/gnupg
# sudo pacman-key --init
# sudo pacman-key --populate archlinux
# sudo pacman-key --populate archlinuxcn

# 加入 sudoers
#sudo sed -i "/root ALL/a $USER\ ALL=(ALL:ALL)\ ALL" /etc/sudoers

# 用户目录
cd ~
mkdir desktop download template public document sound picture video
echo "
XDG_DESKTOP_DIR=\"$HOME/desktop\"
XDG_DOWNLOAD_DIR=\"$HOME/download\"
XDG_TEMPLATES_DIR=\"$HOME/template\"
XDG_PUBLICSHARE_DIR=\"$HOME/public\"
XDG_DOCUMENTS_DIR=\"$HOME/document\"
XDG_MUSIC_DIR=\"$HOME/sound\"
XDG_PICTURES_DIR=\"$HOME/picture\"
XDG_VIDEOS_DIR=\"$HOME/video\"
" > ~/.config/user-dirs.dirs

# base
sudo pacman -S base-devel paru python-pip neovim yadm fzf the_silver_searcher bat fd

# firefox
sudo pacman -S firefox firefox-i18n-zh-cn
firefox https://github.com/gorhill/uBlock
firefox https://addons.mozilla.org/zh-CN/firefox/addon/vimium-c/
firefox https://addons.mozilla.org/zh-CN/firefox/addon/traduzir-paginas-web
firefox https://addons.mozilla.org/zh-CN/firefox/addon/tampermonkey
firefox https://greasyfork.org/zh-CN/scripts/405130-%E6%96%87%E6%9C%AC%E9%80%89%E4%B8%AD%E5%A4%8D%E5%88%B6

# xorg
sudo pacman -S xclip flameshot

# hardware
#  bluetooth
sudo pacman -S bluez bluez-utils
sudo systemctl enable --now bluetooth.service
sudo sed -i 's/#AutoEnable=true/AutoEnable=true/;s/AutoEnable=false/AutoEnable=true/' /etc/bluetooth/main.conf
#  light
sudo pacman -S brightnessctl
#  touchpad
sudo pacman -S wmctrl xdotool libinput-gestures
sudo usermod -aG input $USER
cp /etc/libinput-gestures.conf ~/.config/libinput-gestures.conf.bak
echo '
# 三指向上 任务列表
gesture swipe up 3 xdotool key super
# 三指向下 隐藏当前任务
gesture swipe down 3 xdotool key super+h
# 三指向左 下一个任务
gesture swipe left 3 xdotool key alt+Shift+Tab	
# 三指向右 上一个任务
gesture swipe right	3 xdotool key alt+Tab
# 四指向上 应用抽屉
gesture swipe up 4 xdotool key super+a
# 四指向下 通知
gesture swipe down 4 xdotool key super+v
# 四指向左 下一个工作区
gesture swipe left 4 _internal ws_up	
# 四指向右 上一个工作区
gesture swipe right	4 _internal ws_down
# 两指捏合 任务列表
#gesture pinch in	xdotool key super+s
# 两指张开 任务列表
#gesture pinch out	xdotool key super+s
' > ~/.config/libinput-gestures.conf
#sudo reboot
#  关闭蜂鸣
sudo rmmod pcspkr
sudo touch /etc/modprobe.d/nobeep.conf
echo "
# Do not load the pcspkr module on boot
blacklist pcspkr
" | sudo tee -a /etc/modprobe.d/nobeep.conf
sudo touch /etc/modprobe.d/blacklist.conf
echo "
install MODULE /bin/false
" | sudo tee -a /etc/modprobe.d/blacklist.conf
#sudo reboot

# font
#  paru -S nerd-fonts-jetbrains-mono # 和 nerd-fonts-complete 二选一
sudo pacman -S nerd-fonts-complete
paru -S ttf-dejavu ttf-joypixels ttf-material-design-icons wps-office-mui-zh-cn ttf-wps-fonts

# input method
sudo pacman -S fcitx5-im fcitx5-chinese-addons fcitx5-rime fcitx5-qt fcitx5-gtk fcitx5-configtool
paru -S gnome-shell-extension-kimpanel-git # 重启后去"扩展"应用中开启
echo '
# fcitx
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx"
export XMODIFIERS="@im=fcitx"
export INPUT_METHOD="fcitx"
export XIM="fcitx"
export XIM_PROGRAM="fcitx"
export SDL_IM_MODULE="fcitx"
export GLFW_IM_MODULE="ibus"
' >> ~/.profile

# gnome
#  插件
sudo pacman -S chrome-gnome-shell
firefox https://addons.mozilla.org/zh-CN/firefox/addon/gnome-shell-integration/
firefox https://extensions.gnome.org/extension/615/appindicator-support/
firefox https://extensions.gnome.org/extension/1085/simple-net-speed/
firefox https://extensions.gnome.org/extension/779/clipboard-indicator/
#  光标与图标 安装完后到工具-优化设置
sudo pacman -S vimix-cursors tela-circle-icon-theme-git

# note
paru -S nutstore-experimental
echo "
[Desktop Entry]
Encoding=UTF-8
Type=Application
Terminal=false
#Icon=
Terminal=true
Exec=sh -c \"cd $HOME/Nutstore\ Files && nvim .\"
Name=note
Name[zh_CN]=note
Comment=note
Comment[zh_CN]=note
Categories=Net;Tool;
" > ~/.local/share/applications/note.desktop

# 先挂载好存储盘cd进去
#cp -r ./.ssh ~/ && chmod 0600 ~/.ssh/id_rsa # 以确保本地和 github 有 ssh public key, 
#没有的话就 ssh-keygen -t rsa
#xclip -sel clip ~/.ssh/id_rsa.pub # xorg
#firefox https://github.com/settings/ssh/new # 击 "key" 文本框后 ctrl+v 粘贴

# docker
sudo pacman -S docker docker-compose
sudo systemctl enable --now docker
# 若没有 docker 组, 就新建一个 docker 组
sudo groupadd docker
# 将当前用户加入 docker 组中
sudo usermod -aG docker ${USER} 
# 刷新组缓存, 即时生效
newgrp ${USER}
# 更新 docker 组
newgrp docker
# 如果操作镜像时还是提示权限问题, 重启可解
# 验证 docker 服务运行状态
sudo systemctl status docker
# 查看 docker 版本
docker version
# 查看 docker 信息
docker info
# 登录
docker login
# 加速
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io
sudo systemctl restart docker # 要正常拉取镜像需要重启

# 魔法二选一， 装完配置好并开启代理
#  v2ray
sudo pacman -S v2ray qv2ray
#  v2raya
docker run -d \
  --privileged \
  --network=host \
  --name v2raya \
  -e V2RAYA_ADDRESS=0.0.0.0:2017 \
  -v /lib/modules:/lib/modules \
  -v /etc/resolv.conf:/etc/resolv.conf \
  -v /etc/v2raya:/etc/v2raya \
  mzz2017/v2raya
echo "
[Desktop Entry]
Encoding=UTF-8
Type=Application
Terminal=false
#Icon=
Exec=sh -c \"firefox localhost:2017\"
Name=v2raya
Name[zh_CN]=v2raya
Comment=v2raya
Comment[zh_CN]=v2raya
Categories=Net;Tool
" > ~/.local/share/applications/v2raya.desktop
# vpn
#paru -S openfortivpn

# yadm 
cd ~
yadm clone git@github.com:67906980725/dotfiles.git

# script
cd ~/.local
git clone git@github.com:67906980725/script.git
cd script/service
sudo crontab -u root crontab.txt
echo "
[Desktop Entry]
Encoding=UTF-8
Type=Application
Terminal=false
#Icon=
Exec=sh -c \"$HOME/.local/script/app_group/app_group.sh normal\"
Name=group_normal
Name[zh_CN]=group_normal
Comment=group_normal
Comment[zh_CN]=group_normal
Categories=Tool
" > ~/.local/share/applications/group_normal.desktop

# zsh 
sudo pacman -S zsh zsh-completions fzf fd bat exa
chsh -s "$(which zsh)"
export ZSH=~/.config/zsh
git clone git@github.com:67906980725/conf_zsh.git -l $ZSH
cd $ZSH
git submodule add https://github.com/yaocccc/omz
echo '
export ZSH=~/.config/zsh
. $ZSH/init.zsh
' >> ~/.zshenv
source ~/.zshenv

# neovim
pip install pynvim
git clone git@github.com:67906980725/yaocccc_nvim.git -l ~/.config/nvim
nvim -c PackerSync # 命令报错的话行sync完再来一次

# qt
sudo pacman -S qt5ct
paru -S qt5-styleplugins
echo "
# 指定qt主题控制程序
export QT_QPA_PLATFORMTHEME=\"qt5ct\"
" >> ~/.profile

# daily
sudo pacman -S mpv goldendict-git obs-studio kdenlive calibre 

# syncthing
sudo pacman -S syncthing
sudo systemctl enable --now syncthing@${USER}.service
firefox 127.0.0.1:8384 # 刷新一下
    
# cornie 
sudo pacman -S cronie
sudo systemctl enable --now cronie.service

# nodejs (需要分开执行)
curl https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh -o- | bash
nvm install 16.18.1 # 需要新开终端
nvm use 16.18.1
npm install -g nrm
npm install -g pnpm

# icalingua++
sudo pacman -S fuse2
#  启动器
echo "
[Desktop Entry]
Encoding=UTF-8
Type=Application
Terminal=false
#Icon=
Exec=sh -c \"exec $HOME/.local/bin/Icalingua++.AppImage\"
Name=icalingua++
Name[zh_CN]=icalingua++
Comment=icalingua++
Comment[zh_CN]=icalingua++
Categories=Net;Tool
" > ~/.local/share/applications/icalingua++.desktop
#  本地表情
mkdir -p $HOME/.config/icalingua
ln -s $HOME/asset/tel/digital/sticker $HOME/.config/icalingua/stickers
#  安装
mkdir -p ~/.local/project/i ～/.local/bin
cd ~/.local/project/i
git clone git@github.com:67906980725/Icalingua-plus-plus.git
cd Icalingua-plus-plus/icalingua
pnpm install # 有错误就再执行一次
pnpm ibuild

# wechat
#  原生版
paru -S wechat-uos 
#paru -S com.qq.weixin.spark 
paru -S com.qq.weixin.work.deepin

# aria2
docker run -d \
    --name aria2-pro \
    --net=host \
    -u $UID:$GID \
    --restart unless-stopped \
    --log-opt max-size=1m \
    -e PUID=$UID \
    -e PGID=$GID \
    -e UMASK_SET=022 \
    -e RPC_SECRET=123456 \
    -e RPC_PORT=6800 \
    -p 6800:6800 \
    -e LISTEN_PORT=6888 \
    -p 6888:6888 \
    -p 6888:6888/udp \
    -v $CONF_PATH/aria2_pro:/config \
    -v $HOME/downloads:/downloads \
    p3terx/aria2-pro
#  ariang
echo "
[Desktop Entry]
Encoding=UTF-8
Type=Application
Terminal=false
#Icon=
Exec=sh -c \"firefox http://p3terx.gitee.io/ariang/#\!/settings/rpc/set/ws/localhost/6800/jsonrpc/$(echo -n 123456 | base64)\"
Name=AriaNg
Name[zh_CN]=AriaNg
Comment=AriaNg
Comment[zh_CN]=AriaNg
Categories=Net;Network,Tool
" > ~/.local/share/applications/aria_ng.desktop
# 如果下载秒失败查看 ~/downloads 所属用户和组是 root 的话 sudo chown $USER:$USER ~/downloads

# other
firefox https://code.visualstudio.com/Download
firefox https://github.com/liupan1890/aliyunpan/releases
paru -S baidunetdisk-bin
#  wps-office
paru -S wps-office
#paru -S ttf-wps-fonts ttf-ms-fonts wps-office-fonts wps-office-mime wps-office-mui-zh-cn

# sunlogin or rustdesk
paru -S sunlogin
sudo systemctl enable --now runsunloginclient.service
paru -S rustdesk-bin
sudo systemctl enable --now rustdesk
#sudo pacman -S tigervnc

# grub theme (如果bootloader是grub的话)
mkdir ~/.local/src
cd ~/.local/src
git clone https://github.com/67906980725/darkmatter-grub-theme-arch-patcher.git
cd darkmatter-grub-theme-arch-patcher
./run.sh # 2-1-1(大概

# virtualbox
uname -srm # 查看内核版本
sudo pacman -S linux-headers
sudo pacman -S virtualbox  # 选择 1 virtualbox-host-dkms
sudo pacman -S  virtualbox-guest-iso
sudo pacman -S  virtualbox-ext-oracle
sudo modprobe vboxdrv
sudo modprobe vboxnetadp
sudo modprobe vboxnetflt
sudo usermod -a -G vboxusers $USER 
#  u盘
sudo groupadd usbfs
sudo usermod -a -G usbfs $USER
sudo usermod -a -G vboxusers $USER
#sudo reboot now
```

## 装完app后

    firefox:
        点击右上角菜单"三"-Settings-Language-Choose the language...-Search for more languages...-Select a language to add...-最底部简体中文-Add-OK
        搜索-默认搜索引擎-Bing
        隐藏扩展：右击要隐藏的扩展后按p
    
    gnome:
    打开终端选择 `配置文件`, 
    点 `命令-运行自定义命令而不是 shell` 
    在 `自定义命令` 中输入 `zsh`
    terminal 最大化打开
    在快键盘快捷键设置中搜索 `ctrl+alt+t`, 命令后加 --maximize
    如果开了luks分区加密可以开启开机自动登录:
    设置-用户-解锁-自动登录
    
    截图:
    把命令 `flameshot gui` 添加到系统快捷键
    
    亮度:
    增加亮度 `brightnessctl set 2.5%+`
    降低亮度 `brightnessctl set 2.5%-`
    增加到系统自定义快捷键
    音量的话鼠标移到状态栏音量图标上就能用滚轮调了
    
    virtualbox 共享文件夹:
    在虚拟机系统中挂载 `VirtualBox 增强工具` 并安装
    添加共享文件夹时勾上自动挂载或
    使用 `net use x:\\vboxsvr\shareFolderName`

## 重启后

``` shell 
# 触摸板
libinput-gestures-setup autostart start

# gnome 扩展中开启插件
```

## 装完插件后

``` shell
# 上传网速前置
cp ~/.local/share/gnome-shell/extensions/simplenetspeed@biji.extension/extension.js ~/.local/share/gnome-shell/extensions/simplenetspeed@biji.extension/extension.js.bak
sed -i '/speedToString(speedUp)/c \ \ \ \ \ \ \ \ \ \ \ \ ioSpeed.set_text(" ↑" + speedToString(speedUp) + "↓" + speedToString(speed - speedUp));
' ~/.local/share/gnome-shell/extensions/simplenetspeed@biji.extension/extension.js
```

## README

    fcitx5: 
    "经典用户界面" 组件不要禁用, 会变得不幸
    idea 中 fcitx5 光标问题: 
    https://blog.csdn.net/weixin_43840399/article/details/112205858

    crontab: `-l` 查看 `-r` 移除 `-u` 用户 `-e` 编辑 `-` 覆盖

    obs必要设置(放yadm): 
    `右击`-`变换`-`拉伸到全屏`; 
    `文件-设置-输出-输出模式-高级`; 
    `输出-录像-格式` 设为 `mp4`

    vscode 加速:
    https://code.visualstudio.com/Download 
    下载链接域名换成 https://vscode.cdn.azure.cn 

    grub:
    如果要自己更换 grub 主题背景, grub不支持自动缩放和剪裁, 
    图片后缀名也需要符合图片格式


## 有用的

### 热点

```
sudo pacman -S create_ap
# 查看网络接口
ip a

# 临时开启热点
sudo create_ap wlan0 enp3s0f3u2c2 MyAccessPoint MyPassPhrase

# 开机自启
sudo nvim /etc/create_ap.conf

# 至少修改这四项，其他的可以不用更改
# WIFI_IFACE=wlan0 #网卡名称
# INTERNET_IFACE=enp3s0f3u2c2 #网卡名称, 使用 usb 接口的更换 usb 接口后会发生改变
# SSID=AP_Ali #热点名称
# PASSPHRASE=apap_1234 #热点密码

sudo systemctl enable --now create_ap

# 查看服务日志
sudo journalctl -u create_ap
```

create_ap 依赖的 iw 与自带的 nmcli 所依赖的 NetworkManager 冲突, 会导致 gnome 的 wifi 设置搜索不到可用wifi. 暂时只好先用 iw 管理 wifi了

``` shell
sudo iwctl

station wlan0 scan
station wlan0 get-networks
station wlan0 connect <network_name>
station wlan0 show
exit
```

### mpv 常用快捷键

- 音量

> 音量减: `9` 音量加: `0`

- 播放

      开始 / 暂停: `空格`
      全屏 / 窗口: `f`
      退出: `q`
      快进 5 秒: `→`
      快进 60 秒: `↑`
      切换下一个: `回车`

      调节字幕: `z`(→), `x`(←)

      调节画面延迟: 画面延迟: `ctrl-`  画面提前: `ctrl + shift + =`

### calibre 添加词典

    1. 调出词典界面: 在 `E-book viewer` 中
    随便选个单词鼠标右击，在上方找到第二个选项:
    `查阅或搜索选中的文字`

    2. 在界面右侧最下方点击 `+添加资源`

    3. 点击添加, 然后输入名称和地址。名称自己随便取。
    地址的话就是官方地址首页，然后在尾部加上:
    `{word}` 或者 `?q{word}`，
    注意地址一定都是英文字符，word要小写，大写无用。
    至于是加带问号的还是不带问号的，只能自己挨个试试啦

``` txt

谷歌翻译 https://translate.google.cn/?q={word}

柯林斯(简洁但没发音) https://m.youdao.com/singledict?q={word}&dict=collins

必应词典(缩放不太好) https://cn.bing.com/dict/search?q={word}

海词英文(缩放不太好) http://dict.cn/{word}

有道词典(缩放不太好) http://dict.youdao.com/search?q={word}

金山词霸(缩放不太好) http://www.iciba.com/{word}

韦伯词典(失效?) https://www.merriam-webster.com/?q={word}

大辞海(缩放不太好, 失效?) http://dacihai.com.cn/?q{word}

海词汉语词典 https://hanyu.dict.cn/{word}

汉典 https://www.zdic.net/hans/{word}

泰伯医学词典 https://www.tabers.com/tabersonline?q{word}
```

来源 [calibre中如何使用词典？](https://www.zhihu.com/question/56063180)

### golden-dict

#### 必应

新建文件 `bing.html`

``` html
<!DOCTYPE html>
<html>
<head>
    <meta charset=utf-8 />
    <style>
        iframe{ width: 706px; height: 650px; margin-top:-190px; margin-left:-120px;
        }
    </style>
</head>

<body>
    <iframe id="a" frameborder="0">
    </iframe>
    <script>
    var word = location.href.slice(location.href.indexOf('?a')+3);
    document.getElementById('a').setAttribute(
        'src', 
        'https://www.bing.com/dict/search?q=' + word);
    </script>
</body>
</html>
```
    
    菜单栏选择 【编辑】>【词典】>【词典来源】>【网站】> 添加，
    在新添加条目前勾选 【已启用】 和 【作为链接】，
    条目 【名称】 可自定义，【地址】填：`file://FILE_PATH` 
    如 file:///home/v/dict/bing.html

#### google-translate
    
    需要 python 3.7+, 如果没有 pip 先安装: 
    `sudo pacman -S python-pip`
    `pip3 install google-translate-for-goldendict`
    
    `编辑 - 字典 - 字典来源 - 程式`
    类型: `Html`
    名称: `Google Translate`
    命令行: `python -m googletranslate zh-CN %GDWORD%`
    类型可以设为 `Html` 或 `纯文本`
    其中: `Html` 对应 `-r "html"`. `纯文本` 对应 `-r "plain"`

[github](https://github.com/xinebf/google-translate-for-goldendict)

#### 构词法规则

要想能直接用复数时态等查词需要添加对应语言的构词法规则 [下载地址](http://sourceforge.net/projects/goldendict/files/better%20morphologies/1.0/)

#### 发声音频播放器调用失败:

`编辑 - 首选项 - 音频 - 播放 - 使用外部程序播放: mpv`

参考:

- [GoldenDict 中设置在线词典](https://zhuanlan.zhihu.com/p/151810213)
- [快速上手 Goldendict](https://zhuanlan.zhihu.com/p/344770839)
- [安装使用 GoldenDict 查词神器 (Windows/Mac/Linux)](https://www.cnblogs.com/keatonlao/p/12702571.html)

:: 查词慢的问题, 在词典中把维基百科禁用了就好了


### datagrip sqlite3 新增表找不到的问题

[sqlite3 新增表找不到的问题](https://www.cnblogs.com/tuobei/p/12616465.html?ivk_sa=1024320u " 关于pycharm 使用sqlite创建数据库表，创建模型后，表找不到或者不显示"):

在数据库上点开小扳手 在 file 栏输入当前项目文件夹里的 db.sqlite3 文件地址 ok

如果之前己经开过 console 了(一般肯定都开过了), 在打开的默认查询 console 标签下的运行工具栏最后有一个会话标签, 从这里切换到
default 会话就可以查到了, 然后从底部的服务标签中删除刚刚那个无效的 console 就可以了

*查看 `StartupWMClass` 的方式: 命令行输入 `xprop WM_CLASS` 然后点击目标窗口, 修复其它固定到 dock 栏启动后窗口图标和启动图标分离的应用都可以这样先找到 StartupWMClass 后填入 /usr/share/applications 下对应的启动快捷方式中

### 游戏

#### wine dx 库

`sudo pacman -S dxvk-bin` 没试

#### steam

`sudo pacman -S steam`

高分屏显示问题

``` shell
cp /usr/share/applications/steam.desktop ~/.local/share/applications/steam.desktop
nvim ~/.local/share/applications/steam.desktop
```

修改 `Desktop Entry` 下的 `Exec=/usr/bin/steam-runtime %U`
一行为 `Exec=sh -c "export GDK_SCALE=2 && /usr/bin/steam-runtime %U"`

#### psp

`sudo pacman -S ppsspp`

运行 `PPSSPP(Qt)`  这个启动图标

窗口太小时 `game settings` 中 `window size` 调到 `5x` (启动失效, 默认设成全屏比较好)

全局声音默认是0, 需要手动修改

如果碰到软件设置在下一次启动会失效的情况, 先更改好所有配置不要关闭 ppsspp, 开终端执行

``` shell
cd ~/.config/ppsspp/PSP/SYSTEM
sudo chmod 555 ppsspp.ini controls.ini
```

要修改的话

``` shell
cd ~/.config/ppsspp/PSP/SYSTEM
sudo chmod 755 ppsspp.ini controls.ini
```

也可以用 flatpak 安装

``` shell
# 国内源
sudo flatpak remote-modify flathub --url=https://mirror.sjtu.edu.cn/flathub

flatpak install flathub org.ppsspp.PPSSPP

# 如果出现错误可尝试
#wget https://mirror.sjtu.edu.cn/flathub/flathub.gpg
#sudo flatpak remote-modify --gpg-import=flathub.gpg flathub

```

[游戏下载](http://www.k73.com/down/psp/list-33-hot-1.html)

[PSP模拟器大全](https://www.bilibili.com/video/av851400458?vd_source=e7270e191e69d8e7b281edda1d105ff9)

#### 安卓

参考 [在 arch linux 上玩 android 游戏](https://www.cx03.space/2021/08/13/%E5%9C%A8-arch-linux-%E4%B8%8A%E7%8E%A9-android-%E6%B8%B8%E6%88%8F/), [下载 android studio](https://developer.android.google.cn/studio)


``` shell
# 创建启动快捷方式
echo "
[Desktop Entry]
Encoding=UTF-8
Type=Application
Terminal=false
Icon=$APP_PATH/android-studio/bin/studio.png
Exec=sh -c \"exec $APP_PATH/android-studio/bin/studio.sh\"
Name=AndroidStudio
Name[zh_CN]=AndroidStudio
Comment=AndroidStudio
Comment[zh_CN]=AndroidStudio
Categories=Develop;Tool;
" > ~/.local/share/applications/android-studio.desktop

# 创建虚拟机的启动快捷方式
# 下面的 `Pixel_4_API_30` 改成实际虚拟机的名字
echo "
[Desktop Entry]
Encoding=UTF-8
Type=Application
Terminal=false
Icon=$APP_PATH/android-studio/bin/studio.png
Exec=sh -c \"exec ~/Android/Sdk/emulator/emulator -avd Pixel_4_API_30\"
Name=Avd
Name[zh_CN]=Avd
Comment=Avd
Comment[zh_CN]=Avd
Categories=Develop;Tool;Game
" > ~/.local/share/applications/avd.desktop
```

#### switch

`sudo pacman -S yuzu-mainline-git`

下载密钥文件 [prod.keys](https://pan.baidu.com/s/1_uk6cMrDKP47U7yLSYydqQ) 提取码: b2ie, 运行模拟器，点击『文件』-> 『Open yuzu folder』，将 sysdata 放到该目录下 [参考](https://zhuanlan.zhihu.com/p/406048136)

[游戏下载](https://fourpetal.com/switchyouxi?order=hot)  [合辑](https://fourpetal.com/27637.html)

nsz 转换: `paru -S nsz-git` 把 `prod.keys` 复制一份放到 `~/.switch/prod.keys`

`nsz -D <file>`

### 修复引导

启动 live cd, 打开终端

``` shell
# passwd # sudo 要密码时执行
# 列出磁盘和分区, 我的系统所在硬盘是 `nvme0n1`, 
# 根分区是 `nvme0n1p2`, efi(esp) 分区是 `nvme0n1p1`
sudo lsblk
# 挂载根分区 
sudo mount /dev/nvme0n1p2 /mnt
# 挂载 efi 分区 
sudo mount /dev/nvme0n1p1 /mnt/boot/efi
# 切换根目录 
sudo arch-chroot /mnt
# 安装 grub 
sudo grub-install --recheck /dev/nvme0n1
# 生成配置 
sudo grub-mkconfig -o /boot/grub/grub.cfg

# 退出 chroot 环境并关机
exit
shutdown now
```

### 包管理常用命令

#### pacman

参考 [Arch Linux 软件包的查询及清理](https://www.cnblogs.com/sztom/p/10652624.html)

查询 lib 被谁依赖

`pacman -Qi libname | grep "Required By"`

查询包依赖谁

`pacman -Qi packagename | grep "Depends On"`

列出孤立的包（-t不再被依赖的"作为依赖项安装的包"）

`pacman -Qqdt` 通常这些是可以妥妥的删除的

`sudo pacman -Qqdt | sudo pacman -Rs -`

列出包所拥有的文件

`sudo pacman -Ql packagename`


## sway 桌面 (wayland 环境)

参考:

- [探索linux桌面全面wayland化（基于swaywm）](https://zhuanlan.zhihu.com/p/462322143 "探索linux桌面全面wayland化（基于swaywm）")

- [[译] Arch Linux 上的完整 Wayland 设置](https://a-wing.top/linux/2022/01/03/translate_wayland.html#gaps "[译] Arch Linux 上的完整 Wayland 设置")

``` shell
sudo pacman -S alacritty sway waybar otf-font-awesome swayidle swaylock wofi clipman brightnessctl
sudo pacman -S lxappearance # 配置gtk主题
# sudo pacman -S grimshot(代替flamshot) swaybg(设壁纸) imv（图片查看）

# 修复idea黑屏
echo "
# 修复idea黑屏
export _JAVA_AWT_WM_NONREPARENTING=1
" >> ~/.profile
```


## xorg 下纯 wm 环境的一些东西

### 触控板轻触

```plaintext
/etc/X11/xorg.conf.d/30-touchpad.conf

Section "InputClass"
    Identifier "touchpad"
    Driver "libinput"
    MatchIsTouchpad "on"
    Option "Tapping" "on"
    Option "TappingButtonMap" "lrm"
EndSection
```

### 音频和蓝牙

```shell
sudo pacman -S easyeffects pipewire-pulse
sudo pacman -S pavucontrol
sudo pacman -S bluez bluez-utils bluedevil
pactl load-module module-bluetooth-discover
sudo vim /etc/bluetooth/main.conf

FastConnectable=true
AutoEnable=true
```

### 日常软件

```shell
yay -S xorg-xsetroot
yay -S xf86-input-synaptics # 触摸板
yay -S network-manager-applet
yay -S rofi
yay -S libxft-bgra # 彩色emoji
yay -S lxappearance # gui设置
sudo pacman -S mpc mpd ncmpcpp # 音乐
yay -S byzanz # 命令行截动图
```

## 一些常用知识

- 变量相关文件 / 目录

`/etc/environment`, `~/.pam_environment`, `~/.zshenv` 环境变量配置项, 在任何场景下都能被读取, 通常把 `$PATH` 等变量写在这里,
这样无论是在交互 shell 或者运行程序都会读取此文件

`~/.zlogin` 在 login shell 的时候读取, 比如系统启动的时候会读取此文件

`/etc/profile`, `~/.profile`, `~/.zprofile` 当用户第一次登录时, 该文件被执行, 当被修改时, 必须重启才会生效. .zprofile 是
.zlogin 的替代品，如果使用了 .zlogin 就不必再关心此文件

`/etc/profile.d/` 可以简单的理解为是 /etc/profile 的一部分, 只不过按类别或功能拆分成若干个文件进行配置了 (
方便维护和理解)

`/etc/bashrc`, `~/.bashrc`, `~/.zshrc` 为每个运行对应 shell 的用户执行该文件, 当 shell 打开时, 该文件被执行, 其配置对所有使用
shell 的用户打开的每个 shell 都有效. 当被修改后, 不用重启只需要打开一个新的 shell 即可生效.

`~/.bash_logout`, `~/.zlogout` 每次退出对应 shell 时执行该文件, 可以把一些清理工作的命令放进这个文件

以上需要重启才能生效的文件, 其实可以通过source xxx暂时生效.

执行顺序:

当登录 Linux 时, 首先启动 /etc/environment 和 /etc/profile,
然后启动当前用户目录下的 /.bash_profile
执行此文件时一般会调用 /.bashrc 文件,
而执行 /.bashrc 时一般会调用 /etc/bashrc,
最后退出 shell 时, 执行 /.bash_logout.

简单来说顺序为:

登录时

`/etc/environment` → `/etc/profile`, `/etc/profile.d/*` → `~/.bash_profile`

打开shell时

`~/.bashrc` → `/etc/bashrc`

退出shell时

`~/.bash_logout`

zsh

`.zshenv` → `[.zprofile if login]` → `[.zshrc if interactive]` → `[.zlogin if login]` → `[.zlogout sometimes]`

- 文件与文件夹

大小

    `B` 是字节 Byte

    `KB` 是千字节 Kilobyte

    `MB` 是兆字节 MegaByte

    `GB` 是千兆字节 Gigabyte

    `TB` 是太兆字节 Terabyte

File, Folder 与 Directory 的区别

> `directory` 也是一个 `folder`, 但是我们在说一个 `directory` 的时候, 通常暗示了它的“路径”因素, 如下示例

> Please go to C:\News\test\ directory, double click and open folder "My Pics" . you will see the file aerchi.txt ...

- 用户权限

文件权限

    r=4 w=2 x=1

    一般用户文件权限为 `(d/-)rwxr-xr-x` 也就是 `755`

    第一位表示文件类型。d是目录文件，l是链接文件，-是普通文件，p是管道

    第2-4位表示这个文件的属主拥有的权限，r是读，w是写，x是执行

    第5-7位表示和这个文件属主所在同一个组的用户所具有的权限

    第8-10位表示其他用户所具有的权限

    `chmod 4755` 与 `chmod 755` 的区别在于开头多了一位，这个4表示其他用户执行文件时，具有与所有者相当的权限

更改所属用户

`sudo chown -R user[:group] <dir_name...>`

将用户添加到组

`sudo usermod -aG groupname username `

- 查看内存硬件信息

``` shell
sudo dmidecode -t memory
```
