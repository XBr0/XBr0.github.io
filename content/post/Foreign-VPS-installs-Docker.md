---
title: "国外VPS安装Docker"
date: 2025-11-18T20:44:03+08:00
categories: ["Docker"]
tags: ["Docker"]
---
本文介绍了在 Linux 系统上快速安装和配置 Docker 与 Docker Compose 的完整流程。通过官方脚本一键安装 Docker，从 GitHub 下载对应平台的最新版 docker-compose 二进制文件，并设置 Docker 服务开机自启。此外，还提供了免 sudo 使用 Docker 的方法：创建 docker 用户组，将当前用户加入该组，并通过 newgrp 命令即时生效权限变更。
#### 安装docker

```
sudo curl -fsSL https://get.docker.com | bash
```

#### 安装docker-compose

```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose
```

#### 自启动docker

```
sudo systemctl start docker
```

```
sudo systemctl enable docker
```

#### 一键完整配置

```
sudo curl -fsSL https://get.docker.com | bash && sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose && sudo systemctl start docker && sudo systemctl enable docker
```

#### 无需sudo使用docker

##### 创建docker用户组

```
sudo groupadd docker
```

##### 把当前用户加入到docker用户组

```
sudo gpasswd -a $USER docker
```

##### 更新当前用户组变动（就不用退出并重新登录了）

```
newgrp docker
```