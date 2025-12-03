---
title: "Docker版 Gitea 部署解决 ROOT_URL 警告及配置详解"
date: 2025-12-03T21:06:13+08:00
categories: ["Docker"]
tags: ["gitea"]
---

## 1. 问题描述

在使用 Docker 部署 Gitea 后，进入管理页面时可能会在顶部看到如下红色警告提示：

> **Your ROOT_URL in app.ini is "https://x.x.x/", it's unlikely matching the site you are visiting.**
> Mismatched ROOT_URL config causes wrong URL links for web UI/mail content/webhook notification/OAuth2 sign-in.

**影响：**
如果忽略此错误，会导致 Gitea 生成的仓库克隆链接、邮件通知链接、Webhook 回调地址以及 OAuth2 登录跳转地址不正确。

## 2. 解决方法

我们需要修改 Gitea 挂载卷中的配置文件 `app.ini`。

### 步骤一：找到配置文件

根据您的 Docker 挂载路径，找到 `app.ini` 文件。
通常路径为（请根据实际情况调整）：

```bash
vi /root/docker/gitea/data/gitea/conf/app.ini
```

### 步骤二：修改 [server] 配置

找到 `[server]` 区域，根据您的实际部署场景（域名反代 或 IP直接访问）选择下方对应的配置进行修改。

#### 场景 A：使用域名 + 反向代理（推荐）

适用于使用 Nginx/Traefik 等反代工具，并通过 `https://git.example.com` 访问的情况。

```ini
[server]
# 这里的 DOMAIN 填写单纯的域名（不带 http/https）
DOMAIN = git.example.com
# SSH 域名通常与访问域名一致
SSH_DOMAIN = git.example.com

# 【核心配置】必须填写完整的外部访问 URL，以此消除警告
ROOT_URL = https://git.example.com/

# 内部监听配置（Docker 内部通常保持 http 即可，SSL 由反代处理）
PROTOCOL = http
HTTP_ADDR = 0.0.0.0
HTTP_PORT = 3000
```

#### 场景 B：群晖 NAS 或 IP 直连访问

适用于没有域名，直接通过局域网 `IP:端口` 访问的情况。

```ini
[server]
# 填写群晖或服务器的局域网 IP
DOMAIN = 172.28.6.5
SSH_DOMAIN = 172.28.6.5

# 【核心配置】必须包含协议 + IP + 映射出去的端口
# 注意：这里的端口号是你在 Docker 映射时宿主机的端口（例如 58816）
ROOT_URL = http://172.28.6.5:58816/

PROTOCOL = http
HTTP_ADDR = 0.0.0.0
# 容器内部端口通常保持默认 3000
HTTP_PORT = 3000
```

### 步骤三：重启容器

修改配置文件后，必须重启 Gitea 容器才能生效：

```bash
docker restart gitea
```

*(注：请将 `gitea` 替换为您实际的容器名称)*

---

## 3. 为什么需要修改 ROOT_URL？

*   **ROOT_URL** 是 Gitea 认为“外界”访问它的唯一真实地址。
*   当您通过浏览器访问的地址（例如 `http://192.168.1.5:5000`）与配置文件中的 `ROOT_URL` 不一致时，Gitea 为了安全和链接生成的准确性，会发出警告。
*   正确配置此项可以修复仓库的 Clone 地址、头像显示以及第三方登录回调问题。