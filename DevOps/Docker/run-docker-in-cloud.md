## 1、安装[docker](https://docs.docker.com/install/linux/docker-ce/centos/)

* ### 卸载旧版本

```
$ sudo yum remove docker \
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

* ### 安装依赖包

```
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
```

* ### 添加稳定版源

```
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

* ### 安装docker

```
$ sudo yum install docker-ce
$ sudo systemctl start docker
```

* ### 测试docker

```
$ sudo docker run hello-world
```

## 2、安装[portainer](https://portainer.io/install.html)

* ### 获取portainer

```
$ sudo docker pull portainer/portainer
```

* ### 配置数据挂在目录

```
$ docker volume create portainer_data
```

* ### 启动portainer

```
$ docker run -d -p 9000:9000 \
                -v /var/run/docker.sock:/var/run/docker.sock \
                -v portainer_data:/data portainer/portainer
```

* ### 测试portainer

访问地址 [http://127.0.0.1:9000](http://127.0.0.1:9000)

> ps:第一次访问需要设置管理员密码。



