## Install Arch Linux
####  检查网络是否可用

```
ip addr 或者 ip link
ping  www.baidu.com
```

#### 更新镜像源

```
reflector -–verbose -c “China” --sort rate > /etc/pacman.d/mirrorlist
curl -L -o /etc/pacman.d/mirrorlist "https://archlinux.org/mirrorlist/?country=CN“
pacman -Syy
```

#### 确保系统时间是准确

```
 timedatectl set-ntp true
```

#### 查看硬盘分区

```
lsblk 或者 fdisk -l
```

#### 创建root分区和swap分区

```
fdisk /dev/sda
```

|  挂载点  |             分区             | [分区类型](https://en.wikipedia.org/wiki/Partition_type) |   建议大小   |
| :------: | :--------------------------: | :------------------------------------------------------: | :----------: |
| `[SWAP]` | `/dev/sda1（交换空间分区）*` |                  Linux swap (交换空间)                   | 大于 512 MiB |
|  `/mnt`  |    `/dev/sda2（根分区）*`    |                          Linux                           |   剩余空间   |

#### 格式化分区

```
mkswap /dev/sda1
mkfs.ext4 /dev/sda2
```

#### 挂载分区

```
mount /dev/sda2 /mnt
swapon /dev/sda1
#检查是否正确
lsblk 
```

#### 安装必须软件包

```
pacstrap /mnt base linux linux-firmware sudo dhcpcd networkmanager vim openssh man net-tools
```

- https://gitlab.archlinux.org/archlinux/archiso/-/blob/master/configs/releng/packages.x86_64
- base: ArchLinux 运行所需的基础软件包集合
- linux: Linux 内核
- linux-firmware: Linux 设备驱动集合，包含了绝大多数设备的驱动（固件）。
- bash-completion: Bash 补全支持
- vim: 文本编辑器
- [networkmanager](https://wiki.archlinux.org/title/NetworkManager): 网络管理器
- pacman-contrib: pacman 相关实用脚本
- sudo: 用于普通用户获取 root 权限
- amd-ucode intel-ucode: AMD 和 Intel 的 CPU 微码更新，用于修补 CPU 漏洞。

#### 生成 fstab文件

```
genfstab -U /mnt >> /mnt/etc/fstab
#检查是否正确
vim /mnt/etc/fstab
```

#### Change root 到新安装的系统

```
arch-chroot /mnt
```

#### 时区

```
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

#### 设置主机名

```
echo ${hostname} > /etc/hostname
```

执行vim /etc/hosts 编辑 hosts 文件并添加以下行：

```
127.0.0.1 localhost
127.0.0.1 ${hostname}
::1 ip6-localhost ip6-loopback
::1 ${hostname}
```

#### 本地化

```text
vim /mnt/etc/locale.gen
```

取消注释，使用美式英语和中国简体

```
...
#en_SG ISO-8859-1  
en_US.UTF-8 UTF-8  
#en_US ISO-8859-1  
...
#zh_CN.GBK GBK  
zh_CN.UTF-8 UTF-8  
#zh_CN GB2312  
...
```

或者

```
 echo “en_US.UTF-8 UTF-8” >> /etc/locale.gen
 echo “zh_CN.UTF-8 UTF-8” >> /etc/locale.gen
```

编辑完成以后,通过下面的命令生成 Locale：

```
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
```


#### 设置开机启动项

```
systemctl enable dhcpcd
systemctl enable sshd
```

#### 安装引导程序

```
pacman -S grub
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

####  修改ssh配置文件，允许root用户ssh登录

发现使用root用户不能ssh远程登录系统，是因为配置文件里面没有允许root用户登录：

```
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
systemctl restart sshd
```

#### 让 wheel 用户组可以使用`sudo`获取 root 权

/etc/sudoers

执行`SUDO_EDITOR=vim visudo`

```
%wheel ALL=(ALL) ALL
```

#### 创建一个新[用户](https://wiki.archlinux.org/title/Users_and_groups)并加入 wheel 用户组：

```
useradd -m -G wheel,storage,power -s /bin/bash ${username}
```

useradd -m -G wheel -s /bin/bash ${username}

#### 重启

```
exit
umount -R /mnt
reboot
```

#### 图形界面相关

```
pacman -S xorg xorg-server
```

