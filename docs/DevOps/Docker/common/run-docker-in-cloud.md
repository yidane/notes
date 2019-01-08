---
layout: default
title: Docker
nav_order: 4
parent: DevOps
permalink: /docs/DevOps/Docker/common
---

# 1、安装[docker](https://docs.docker.com/install/linux/docker-ce/centos/)

## 卸载旧版本

``` bash
sudo yum remove docker \
        docker-client \
        docker-client-latest \
        docker-common \
        docker-latest \
        docker-latest-logrotate \
        docker-logrotate \
        docker-selinux \
        docker-engine-selinux \
        docker-engine
```

## 安装依赖包

``` bash
sudo yum install -y yum-utils device-mapper-persistent-data
```

## 添加稳定版源

``` bash
sudo yum-config-manager \
    --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

## 安装docker

``` bash
sudo yum install docker-ce
sudo systemctl start docker
```

## 测试docker

``` bash
sudo docker run hello-world
```

## 2、安装[portainer](https://portainer.io/install.html)

## 获取portainer

``` bash
sudo docker pull portainer/portainer
```

## 配置数据挂在目录

``` bash
docker volume create portainer_data
```

* ### 启动portainer

``` bash
docker run -d -p 9000:9000 \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data portainer/portainer
```

* ### 测试portainer

访问地址 [http://127.0.0.1:9000](http://127.0.0.1:9000)

> ps: 第一次访问需要设置管理员密码。