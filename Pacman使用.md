# Pacman使用

## 1. 简介

pacman 是ArchLinux的包管理工具,与debian，ubuntu发行版的apt-get包管理工具和RedHat系列的RPM包管理工具相似，为了能够轻松的学会使用ArchLinux，轻松安装所需要的各种软件，学会使用pacman 以及熟悉记忆几个常用的pacman参数是至关重要的。
 pacman采用C语言进行编写，其所用的软件包扩展名为 .pkg.tar.xz

- [官方链接 pacman wiki](https://wiki.archlinux.org/index.php/Pacman)
- 镜像源设置: /etc/pacman.d/mirrorlist
- 配置文件: /etc/pacman.conf

## 2.常用参数

- 安装软件包

| 命令   | 参数 | 包名                    | 说明                     | 备注             |
| ------ | ---- | ----------------------- | ------------------------ | ---------------- |
| pacman | -S   | package_name            | 安装软件包               | NA               |
| pacman | -Ss  | package_name            | 查询软件包               | NA               |
| pacman | -Scc | NA                      | 清理包缓存               | 目录在/var/cache |
| pacman | -Ss  | package_name            | 查询软件包               | NA               |
| pacman | -U   | package_name_PATH       | 安装一个本地包           | *.pkg.tar.xz     |
| pacman | -U   | http://.../X.pkg.tar.xz | 从远程安装一个软件包     | *.pkg.tar.xz     |
| pacman | -Sw  | package_name            | 下载一个软件包但是不安装 | NA               |

- 更新查询软件

| 命令   | 参数 | 包名               | 说明                               | 备注               |
| ------ | ---- | ------------------ | ---------------------------------- | ------------------ |
| pacman | -Syy | NA                 | 同步软件列表至本地                 | NA                 |
| pacman | -Syu | NA                 | 升级所有软件包-升级系统            | NA                 |
| pacman | -Syu | NA                 | 升级所有软件包-升级系统            | NA                 |
| pacman | -Sg  | package_group_name | 查询那些软件包在这个软件包组里面   | NA                 |
| pacman | -Q   | NA                 | 查询当前本地软件数据库             | 查看安装了那些软件 |
| pacman | -Qs  | package_name       | 查询已安装的软件包                 | NA                 |
| pacman | -Si  | package_name       | 查询某个软件包的详细信息           | NA                 |
| pacman | -Ql  | package_name       | 查询已安装的软件包所包含的文件列表 | NA                 |

- 删除软件包

| 命令   | 参数  | 包名         | 说明                                         | 备注                                                         |
| ------ | ----- | ------------ | -------------------------------------------- | ------------------------------------------------------------ |
| pacman | -R    | package_name | 卸载软件包                                   | NA                                                           |
| pacman | -Rs   | package_name | 卸载软件包                                   | 包含所有未被其他已安装软件包使用的依赖关系                   |
| pacman | -Rsc  | package_name | 卸载软件包                                   | 包含所有依赖这个软件包的程序                                 |
| pacman | -Rscn | package_name | 卸载软件包                                   | 由于pacman删除某些程序时会自动备份重要配置文件，在其后加上*.pacsave扩展名,-n选项可以删除这些文件 |
| pacman | -Rdd  | package_name | 删除软件包但是不删除依赖这个软件包的其他程序 | NA                                                           |
