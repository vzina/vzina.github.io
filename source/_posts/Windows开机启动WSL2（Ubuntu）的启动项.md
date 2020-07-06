---
title: Windows开机启动WSL2（Ubuntu）的启动项
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-06 22:39:36
password:
summary: 简单配置Windows开机启动WSL2（Ubuntu）的启动项。。。
tags:
  - wsl
  - ubuntu
categories:
  - 工具
---

## 1. 在Ubuntu里执行：

```
sudo vim /etc/init.wsl
```

## 2. 按 i 输入：

```
#!/bin/sh

/etc/init.d/redis-server start
/etc/init.d/mysql start
/etc/init.d/php-fpm start
/etc/init.d/nginx start
```

保存退出

## 3. 授权

```
sudo chmod +x /etc/init.wsl
```

## 4. 在Windows创建txt，输入：

```
Set ws = WScript.CreateObject("WScript.Shell")
ws.run "wsl -d ubuntu -u root /etc/init.wsl"
```

## 5. 给txt重命名成：

`linux-start.vbs` 记得给.txt也去掉

## 6. 按 `win+R` 输入 `shell:startup` 把刚刚那个vbs文件放进来

## 7. 其它

限制wsl内存使用大小
```
[wsl2]
memory=4GB
swap=0
localhostForwarding=true
```