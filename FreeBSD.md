https://docs.freebsd.org/en/articles/linux-users/

### 新增用户

```
pw useradd yzd
passwd yzd
pw groupmod wheel -m yzd
```

### SSH

修改/etc/ssh/sshd_config

```
sed -i ‘s/PermitRootLogin no/PermitRootLogin yes/g’ /etc/ssh/sshd_config
sed -i ‘s/PasswordAuthentication no/PasswordAuthentication yes/g’ /etc/ssh/sshd_config
```

重载sshd服务，即可生效

```
service sshd reload
```

### 禁用默认源

```
vi /etc/pkg/FreeBSD.conf  
```

enabled的值由yes改为no

```
FreeBSD: {
  url: "pkg+http://pkg.FreeBSD.org/${ABI}/quarterly",
  mirror_type: "srv",
  signature_type: "fingerprints",
  fingerprints: "/usr/share/keys/pkg",
  enabled:no
}
```

```
vi /usr/local/etc/pkg/repos/FreeBSD.conf
```

增加科大源

```
FreeBSD: {
    url: "pkg+http://mirrors.ustc.edu.cn/freebsd-pkg/${ABI}/latest",
    mirror_type: "srv",  signature_type: "fingerprints",
    fingerprints: "/usr/share/keys/pkg",
    enabled: yes
}
```

 ```
pkg update
pkg search vim
pkg install vim-8.2.3458
 ```

### 安装bash

```
pkg install -y bash bash-completion
chsh -s /usr/local/bin/bash
```


### 安装vsftpd

```
portsnap fetch extract
find /usr/ports -name '*ftp*'
cd /usr/ports/ftp/vsftpd
make install clean
```

```
vim /etc/rc.conf
```

最后一行增加

```
sftpd_enable="YES"
```

### 安装xray

```
cp /usr/home/ftp/go1.17.2.src.tar.gz /usr/ports/distfiles/ 
```

```
cd /usr/ports/security/xray-core
make install clean
xray run -config ./config2.json
```

