---
title: Ubuntu 服务器管理员指南
date: 2026-04-23 22:00:00 +0800
categories: [服务器]
tags: [服务器, 乌班图, ubuntu, amax]
---

组内目前新到了几台 GPU 服务器，被分配去当其中一台的管理员。因为从来没当过管理员（之前只是在一些服务器上拥有 sudo 权限辅助管理），所以记录一下管理服务器的探索过程。

## 1 安装基础软件

需要管理员负责安装的软件分为两种：

- `tmux`、`htop` 这样较为基础的
- `docker`、`toolkit` 这种较为困难的

### 1.1 基础软件

基础软件目前安装的有 `tmux`、`git`、`curl`、`wget`、`vim`、`htop`、`nvtop`、`rsync`、`ca-certificates`、`net-tools`。

这些基础软件的安装直接使用 `sudo apt install xxx` 安装即可。不过对应的 xxx 可能与命令不相同，只需要运行一下命令，查看提示即可：

### 1.2 CUDA Toolkit

下载网址 <https://developer.nvidia.com/cuda-downloads>，依次根据选项选择即可，运行 nvidia-smi 查看当前 GPU 驱动最高支持的 CUDA 版本（若不支持最新版历史版本在 <https://developer.nvidia.com/cuda-toolkit-archive> 中查找），选择合适的下载。

`Installer Type` 建议选择 `deb (network)`，会自动拉取最新的小版本。按照如下命令安装即可：

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda-toolkit-13-2
```

安装后的工具目录为 `/usr/local/cuda-xx.x`，其中 `xx.x` 是安装的版本。同时 `/usr/local` 目录下还会出现两个软链接，其中一个是 `/usr/local/cuda`，这个软链接会始终指向最新版本的 Toolkit，即便你的环境中存在多个版本的 Toolkit，可以利用软链接配置环境，使其自动指向最新的版本。

环境配置如下：

```bash
export PATH=/usr/local/cuda/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

### 1.3 Docker

Docker 官方安装指南：<https://docs.docker.com/engine/install/ubuntu>

装完之后试验 `sudo docker run hello-world`，若失败的话，建议配置 Docker 镜像站 ：

```bash
sudo vim /etc/docker/daemon.json
```

```json
{
  "debug": true,
  "experimental": false,
  "registry-mirrors": [
    "https://docker.1ms.run",
    "https://docker.xuanyuan.me"
  ]
}
```

其中，`https://docker.1ms.run` 是阿里镜像站， `https://docker.xuanyuan.me` 是轩辕镜像站。

当然也可以使用代理配置，不过在我手下这一台服务器上效果很差，全局代理需要先创建配置文件：

```shell
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf
```

``` c
# /etc/systemd/system/docker.service.d/http-proxy.conf
[Service] 
Environment="HTTP_PROXY=http://proxy.example.com:8080/" 
Environment="HTTPS_PROXY=http://proxy.example.com:8080/" 
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

``` json
// 容器运行阶段内部代理 ~/.docker/config.json
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://proxy.example.com:8080",
     "httpsProxy": "http://proxy.example.com:8080",
     "noProxy": "localhost,127.0.0.1,.example.com"
   }
 }
}
```

完成后需要重启服务

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 1.4 Mihomo

尚在探索整理，未完成
