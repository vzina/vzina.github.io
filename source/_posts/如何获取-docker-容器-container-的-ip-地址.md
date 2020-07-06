---
title: 如何获取 docker 容器(container)的 ip 地址
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-06 21:48:20
password:
summary: 简单的获取 docker 容器(container)的 ip 地址。。。
tags:
  - docker
categories:
  - 工具
---
#### 1. 进入容器内部后
```shell
cat /etc/hosts
```
会显示自己以及(– link)软连接的容器IP

#### 2.使用命令
```shell
docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container-ID>
# 或:
docker inspect <container id>
# 或:
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
```

#### 3.可以考虑在 ~/.bashrc 中写一个 bash 函数：

```shell
function docker_ip() {
    sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}' $1
}

source ~/.bashrc

# 然后：
$ docker_ip <container-ID>
```

#### 4.要获取所有容器名称及其IP地址只需一个命令。
```shell
docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress }}' $(docker ps -aq)
```

如果使用docker-compose命令将是：
```shell
docker inspect -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
```

#### 5.显示所有容器IP地址：
```shell
docker inspect --format='{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)

# 或：
docker inspect --format='{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}} {{end}}' $(docker ps -a|grep devdock|awk '{print $1}')
```


