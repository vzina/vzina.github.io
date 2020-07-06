---
title: ubuntu 18.04开发环境配置
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-06 22:41:42
password:
summary: 一个较完整的、支持多版本的php ubuntu 18.04开发环境。。。
tags:
  - linux
  - ubuntu
  - php
categories:
  - 工具
---
# ubuntu 18.04开发环境配置


### 环境配置

```shell
# 切换阿里云的源地址
sudo mv /etc/apt/sources.list /etc/apt/sources.list.bak

# 编辑源文件
sudo vi /etc/apt/sources.list

# 复制下面内容到文件中

# deb-src http://cn.archive.ubuntu.com/ubuntu bionic-security multiverse
# deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable
# deb-src [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse

# 保存退出vi

# 更新ubuntu源
sudo apt-get update

# 安装编译工具
sudo apt-get install -y autoconf cmake automake gcc g++ make tcl-dev expect git

# 修改sudo后不需要密码
1. 执行 sudo visudo
2. 然后末尾添加，yeweijian为当前用户名
    yeweijian   ALL=(ALL:ALL) NOPASSWD: ALL
3. 保存退出，重新连接
```

1. zsh配置（可选）
```
# 安装zsh
sudo apt-get install -y zsh
zsh --version

# 将 ZSH 设置为当前登录用户的默认 Shell
sudo chsh -s /usr/bin/zsh


# 安装「Oh My ZSH」
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

# 手动安装

# 从git上把oh-my-zsh clone下来到根目录下
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
# 再在根目录下copy一份.zshrc配置
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc


# 语法高亮显示插件
sudo apt-get install zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
echo "source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ~/.zshrc

# 命令提示
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions

# 开启插件
vi ~/.zshrc

# 修改：plugins=(git zsh-syntax-highlighting zsh-autosuggestions sudo z)

# 注意：
# git卡顿现象，全局解决办法:
#    在 ~/.oh-my-zsh/custom/ 文件夹中加入一个自定义脚本，以 .zsh 作为后缀名，比如：~/.oh-my-zsh/custom/disable_git_info.zsh.
vi ~/.oh-my-zsh/custom/disable_git_info.zsh

function git_prompt_info() {
    ref=$(git symbolic-ref HEAD 2> /dev/null) || return
    echo "$ZSH_THEME_GIT_PROMPT_PREFIX${ref#refs/heads/}${ZSH_THEME_GIT_PROMPT_CLEAN}${ZSH_THEME_GIT_PROMPT_SUFFIX}"
}

```
2. 配置rc-local.service

```shell
# 创建启动脚本
sudo touch /etc/rc.local && sudo chmod +x /etc/rc.local

sudo ln -s /lib/systemd/system/rc-local.service /etc/systemd/system
sudo systemctl enable rc-local.service
sudo systemctl start rc-local.service
sudo systemctl status rc-local.service

```

3. 磁盘共享（虚拟机可以直接使用共享）

```shell
方式一

# windows 创建用户
win键+R，然后输入：control userpasswords2
添加用户 --> 本地用户
username=yewj
password=123456

# windows 设置密码不过期
高级-->高级用户管理-->高级，即可看到本地用户和组

# windows 共享目录
1). 确保文件和打印机共享功能已经被打开。 1). 如果你想要共享特定的文件夹，那么就要将该功能设置为开启状态。 ...
2). 找到你想要共享的文件夹。 ...
3). 选择“共享”选项。 ...
4). 点击“特定用户”选项来选择你想要分享给的特定用户。 ...
5). 对列表中的用户的权限进行设定。 ...
6). 点击共享按钮。

# ubuntu 挂载共享目录
sudo mkdir -p /mnt/data
sudo mount -o username=yewj,password=123456,uid=1000,gid=1000,dir_mode=0777,file_mode=0777 //192.168.216.1/data /mnt/data

# ubuntu 开机自动挂载
sudo -S mount -o username=yewj,password=123456,uid=1000,gid=1000,dir_mode=0777,file_mode=0777 //192.168.216.1/data /mnt/data <<EOF
password
EOF


方式二

第一步.通过VMware安装VMware Tools
第二步.设置共享文件夹
第三步.前两步就不说了，这一步发现/mnt/hgfs下没有设置的共享文件夹。之前在ubuntu16，需要安装open-vm-dkms。但在ubuntu18.04，apt搜不到，结果换了各种软件源也没有。后来发现不需要装。以下是ubuntu18.04解决步骤。
    1.执行，看是否能找到共享文件夹，如果有继续。
        vmware-hgfsclient
    2.继续执行 （注意中间的空格）
        vmhgfs-fuse .host:/ /mnt/hgfs
    3.步骤2如果出现chown: changing ownership of 'hgfs/': Operation not permitted，是因为/mnt/hgfs权限为dr-xr-xr-x。此时
        sudo chmod 777 /mnt/hgfs
    ，重新执行步骤2。
    4.以上配置，重启后失效，若要开机自动挂载。需配置/etc/fstab文件
        sudo vi /etc/fstab
        在文件最后加上
        .host:/ /mnt/hgfs   fuse.vmhgfs-fuse    allow_other 0   0
        重启前最好先sudo mount -a测试下是否可以，避免出错开不了机
```

4. 配置SSH服务(正常情况下不需要手动配置)

修改配置文件 ` sudo vi /etc/ssh/sshd_config`

```
Port 22222 # 端口号，防冲突可修改
PasswordAuthentication yes # 使用密码登录
```

启动服务 `sudo service ssh restart`


### php安装

1. 添加安装源

```shell
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
```

2. 安装多版本

```shell
# 安装5.6版本
sudo apt-get -y install php5.6 php5.6-common php5.6-dev php5.6-fpm

# 安装7.3版本
sudo apt-get -y install php7.3 php7.3-common php7.3-dev php7.3-xml php7.3-fpm

# 安装7.4版本
sudo apt-get -y install php7.4 php7.4-common php7.4-dev php7.4-xml php7.4-curl php7.4-gd php7.4-mbstring php7.4-mysql php7.4-fpm

# 扩展安装
sudo apt-get -y install php5.6-mysql php7.3-mysql php7.3-xml php7.3-curl php7.3-mbstring
# 或者
sudo pecl install libevent-0.1.0
```

3. 版本切换

```shell
# 列表选择php版本
sudo update-alternatives --config php

# 设置默认php-config版本
sudo update-alternatives --set php-config /usr/bin/php-config7.3
```

### 多网卡配置


```
sudo vi /etc/netplan/50-cloud-init.yaml

# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    ens33: #配置的网卡名称,使用ifconfig -a查看得到,且必须是空格缩进，netplan只认空格
      dhcp4: no #no-dhcp4开启 true-dhcp4开启
      dhcp6: true #true-dhcp6开启 no-dhcp6关闭
      addresses: [192.168.223.129/24, ] #设置本机IP及掩码,这个逗号和空格好像不能少，少了就不生效，后面的空格之后可以写入IPv6的地址，从而变成这样[192.168.2.110/24, "2001:1::1/64"]
      gateway4: 192.168.223.1 #设置ipv4的默认网关
      gateway6: 2001:1::1 #设置ipv6的默认网关

      nameservers:  #设置DNS服务器
       addresses: [8.8.8.8,8.8.8.4]  #多个DNS服务器之间用逗号隔开
```

参考链接：[@see](https://blog.csdn.net/lengye7/article/details/88889807)

### 其它应用

```shell
# 推荐安装方式
sudo apt-get install -y nginx mysql-client mysql-server redis memcached
```

### 便捷脚本

- 以下脚本仅适用于本文档设置的开发环境。

##### php扩展安装脚本


```shell
#!/bin/bash
#
# 用法：bash 脚本名 扩展名，如：bash /path/to/phpextutil swoole
#

set -x

PHP_BIN="/usr/bin/env php"

PHP_VERSION=$($PHP_BIN -r "echo substr(PHP_VERSION, 0, 3);")

if [ ! -n "$1" ]; then
  echo "extension name is empty!"
  exit 1
fi
EXTENSION_NAME=$1 # full name
REAL_EXTENSION_NAME=${EXTENSION_NAME%-*} # real name
PHP_EXTENSION_DIR=$($PHP_BIN -r "echo PHP_EXTENSION_DIR;")

# install extension
if [ ! -f "$PHP_EXTENSION_DIR/$REAL_EXTENSION_NAME.so" ]; then
  sudo pecl channel-update pecl.php.net
  sudo pecl -q install -o -f $EXTENSION_NAME
  if [ ! $? = 0 ]; then
    exit 1
  fi
fi

PHP_CONFIG_FILE_SCAN_DIR_CLI=$($PHP_BIN -r "echo PHP_CONFIG_FILE_SCAN_DIR;")
PHP_CONFIG_FILE_SCAN_DIR_FPM="/etc/php/$PHP_VERSION/fpm/conf.d"
PHP_CONFIG_FILE_SCAN_DIR_SRC="/etc/php/$PHP_VERSION/mods-available"
EXTENSION_FILE="20-$REAL_EXTENSION_NAME.ini"

if [[ -f "$PHP_CONFIG_FILE_SCAN_DIR_SRC/$EXTENSION_FILE" && -d "$PHP_CONFIG_FILE_SCAN_DIR_FPM" && ! -f "$PHP_CONFIG_FILE_SCAN_DIR_FPM/$EXTENSION_FILE" ]]; then
  sudo ln -s "$PHP_CONFIG_FILE_SCAN_DIR_SRC/$EXTENSION_FILE" "$PHP_CONFIG_FILE_SCAN_DIR_FPM/."
  echo "$PHP_CONFIG_FILE_SCAN_DIR_FPM/$EXTENSION_FILE"
  exit 0
fi

if [ -f "$PHP_CONFIG_FILE_SCAN_DIR_CLI/$EXTENSION_FILE" ]; then
  echo "extension [$REAL_EXTENSION_NAME] is exist!"
  echo "Modify the content, please edit this file: $PHP_CONFIG_FILE_SCAN_DIR_SRC/$EXTENSION_FILE"
  exit 2
fi

TMP_FILE="/tmp/$EXTENSION_FILE"
touch $TMP_FILE
echo "; configuration for php common module" > $TMP_FILE
echo "; priority=20" >> $TMP_FILE
echo "extension=$REAL_EXTENSION_NAME.so" >> $TMP_FILE
sudo mv $TMP_FILE $PHP_CONFIG_FILE_SCAN_DIR_SRC
sudo ln -s "$PHP_CONFIG_FILE_SCAN_DIR_SRC/$EXTENSION_FILE" "$PHP_CONFIG_FILE_SCAN_DIR_CLI/."

echo "$PHP_CONFIG_FILE_SCAN_DIR_SRC/$EXTENSION_FILE"
echo "$PHP_CONFIG_FILE_SCAN_DIR_CLI/$EXTENSION_FILE"

if [[ -d "$PHP_CONFIG_FILE_SCAN_DIR_FPM" ]]; then
  sudo ln -s "$PHP_CONFIG_FILE_SCAN_DIR_SRC/$EXTENSION_FILE" "$PHP_CONFIG_FILE_SCAN_DIR_FPM/."
  echo "$PHP_CONFIG_FILE_SCAN_DIR_FPM/$EXTENSION_FILE"
fi

exit 0
```

##### 快速切换版本

```shell
#!/usr/bin/env bash
#
# 用法：bash 脚本名 扩展名，如：bash /path/to/phpevcutil
#

sudo update-alternatives --config php

PHP_BIN="/usr/bin/env php"
PHP_VERSION=$($PHP_BIN -r "echo substr(PHP_VERSION, 0, 3);")
sudo update-alternatives --set php-config "/usr/bin/php-config$PHP_VERSION"
sudo update-alternatives --set phpize "/usr/bin/phpize$PHP_VERSION"


```
