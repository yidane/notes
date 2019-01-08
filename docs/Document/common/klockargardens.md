---
layout: default
title: klockargardens
nav_order: 4
parent: Document
---

# klockargardens

## build docker

``` docker
FROM ubuntu
MAINTAINER yidane

VOLUME [ "/upload" ]
WORKDIR /admin
COPY conf/ conf/
COPY views/ views/
COPY static/ static/
COPY admin klockargardens.json favicon.ico ./

EXPOSE 8080
ENTRYPOINT [ "./admin" ]
```

* ## Local Build

``` bash
sudo docker build -t yidane/klockargardens:0.0.1 .      #admin站点
sudo docker build -t yidane/klockargardens:1.0.1 .      #web站点
sudo docker build -t yidane/klockargardens:2.0.1 .      #baidu tools
```

* ## Local Run

``` bash
sudo docker run -d -v /www/wwwroot/file/upload:/upload -p 8022:8022 --name=web yidane/klockargardens:1.0.1
sudo docker run -v /var/www/html/upload:/upload -p 8080:8080 yidane/klockargardens:0.0.2 --name=admin
```

* ## Local Push to hub.docker.com

``` bash
sudo docker login -u yidane
sudo docker push yidane/klockargardens:0.0.1
sudo docker push yidane/klockargardens:1.0.1
```

* ## Server Pull from hub.docker.com

``` bash
sudo docker login -u yidane
sudo docker pull yidane/klockargardens:0.0.1
sudo docker pull yidane/klockargardens:1.0.1
```

* ## Server Run

``` bash
sudo docker run -d -v /www/wwwroot/file/upload:/upload -p 8080:8080 --name=admin yidane/klockargardens:0.0.2
sudo docker run -d -p 8022:8022 --name=web yidane/klockargardens:1.0.1
```