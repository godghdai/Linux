## EndeavourOS

EndeavourOS是基于Arch Linux的滚动式Linux发行。该项目旨在继承Antergos的衣钵，基于Arch来提供方便的安装及预配置好的桌面环境。EndeavourOS通过Calamares图形系统安装器来安装，缺省使用Xfce桌面。

### 配置国内源

```
sudo sudo pacman -S gvim
sudo gvim /etc/pacman.conf
```

```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = http://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

```
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```

```
sudo gvim /etc/pacman.d/mirrorlist
```

 在文件的最顶端添加

```
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
```

更新软件包缓存

```
sudo pacman -Syy
```



### 常用软件

------

#### 搜狗输入法

```
sudo pacman -S fcitx-im
sudo pacman -S fcitx-configtool
yay S fcitx-sogoupinyin
```

开启输入法框架fcitx

```
gvim ~/.xprofile
```

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```


```
sudo pacman -S vlc
sudo pacman -S qbittorrent-enhanced-git 
yay S qbittorrent-enhanced
yay S google-chrome
yay S uget
yay S uget-integrator-chrome
# typora foxit tmux
```

#### WPS

```
yay S wps-office-cn
yay S ttf-wps-fonts
```

#### 录音+编辑

```
sudo pacman -S audacity
sudo pacman -S obs-studio
sudo pacman -S gimp
sudo pacman -S shotcut
sudo pacman -S joplin-desktop
```



#### 科学上网

------

#####  v2ray

https://github.com/v2fly/v2ray-core/releases

```
yay V2Ray
yay Qv2ray
```

#####  proxychains

直接安装

```
yay proxychains 
```
源码安装
```
git clone https://github.com/rofl0r/proxychains-ng.git
cd proxychains-ng
./configure
make && make install

cp ./src/proxychains.conf /etc/proxychains.conf
cd .. && rm -rf proxychains-ng
```

修改配置

```
gvim /etc/proxychains.conf
```

将socks4 127.0.0.1 9095改为socks5 127.0.0.1 1089 ，测试

```
proxychains4 curl www.google.com
```

优化增加命令别名,编辑.bashrc

```
gvim ~/.bashrc
```

在最后添加

```
alias pc='proxychains4'
```
可以简化输入
```
pc curl http://www.google.com
```

全局代理（仅本终端有效）

方法一
手动设置环境变量

```
export PROXYCHAINS_CONF_FILE=/usr/local/Cellar/proxychains-ng/4.11/etc/proxychains.conf
export DYLD_INSERT_LIBRARIES=/usr/local/Cellar/proxychains-ng/4.11/lib/libproxychains4.dylib
export DYLD_FORCE_FLAT_NAMESPACE=1
```

方法二

```
proxychains4  -q /bin/bash
```
这样在当前 shell 中运行的所有程序的网络请求都会走代理了。可以把上面的命令加入到用户目录的.bashrc或者.zshrc中,用户登录后自动代理一个shell,这就类似一个全局代理了。在这个SHELL下的所有命令都可以使用代理了。

##### shadowsocks

```
yay shadowsocks-qt5
yay -S shadowsocks-qt5
nohup sslocal -c config.json &
```



##### 免费结点

- https://v2rayse.com/free-v2ray/
- https://www.cfmem.com/search/label/free
- https://github.com/changfengoss/pub/tree/main/data
- https://www.youtube.com/watch?v=nVL9tEdxE7o
- https://github.com/Dreamacro/clash



#### 音乐软件

------
##### 网易云音乐
```
yay S netease-cloud-music
```

#####  listen1

https://github.com/listen1/listen1_desktop/releases

```
yay listen1
yay -S listen1-desktop-appimage
```

#####  yesplaymusic

https://github.com/qier222/YesPlayMusic/releases

```
proxychains4 wget  https://github.com/qier222/YesPlayMusic/releases/download/v0.4.1/yesplaymusic-0.4.1.pacman
sudo pacman -U yesplaymusic-0.4.1.pacman
# yay -S yesplaymusic
```

##### 洛雪音乐助手

https://github.com/lyswhut/lx-music-desktop



### 关闭自动更新

https://discovery.endeavouros.com/endeavouros-tools/endeavouros-update-notifier/2021/03/

```
./eos-update-notifier-configure
 /etc/eos-update-notifier.conf 
```



### 配置yay国内源

```
sudo pacman -S yay
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
yay -P -g
```

```
env | grep DESKTOP_SESSION
ps -A | egrep -i "gnome|kde|mate|cinnamon|lx|xfce|jwm"
修改的配置文件位于 ~/.config/yay/config.json
```



### 常用快捷键

```
ctr+alt+t   打开终端
shift+Print 截屏
ctr+alt+up down 切换工作区
ctrl+f1 workspace1 
ctrl+f2 workspace2
```



### 参考链接

- https://wiki.archlinux.org/title/SCP_and_SFTP#Secure_copy_protocol_(SCP)
- https://archlinuxstudio.github.io/ArchLinuxTutorial/#/play&office/office
- https://discovery.endeavouros.com/aur/yay-an-aur-helper-written-in-go/2021/03/
- https://archlinux.org/packages/
