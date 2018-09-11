# 在ubuntu 18.04安装单机版kubernetes 1.3.0

## 1、下载kubernetes 1.3.0

[下载地址](https://github.com/kubernetes/kubernetes/releases?after=v1.3.3 "下载地址")  [百度网盘](https://pan.baidu.com/s/1PvJe1IzY31plsytKK3oOaA "百度网盘")

* 由于github提供的下载链接只有5分钟有效时间，国内网速无法完整下载kubernetes 1.3.0。因此解决办法是在服务器（美国服务器）上先下载，然后从服务器上下载。

## 2、解压

解压后，server子目录中的kubernetes-server-linux-amd64.tar-gz文件包含了kubernetes需要的全部运行服务程序文件。

| 文件名 | 说明 |
| :--- | :--- |
| hyperkube | 总控程序，用于运行其他kubernetes程序 |
| huke-apiserver | apiserver主程序 |
| kube-apiserver,docker\_tag | apiserver docker镜像的tag |
| kube-apiserver.tar | apiserver docker镜像文件 |
| kube-controller-manager | controller-manager主程序 |
| kube-controller-manager.docker\_tag | controller-manager docker镜像的tag |
| kube-controller-manager.tar | controller-manager docker镜像文件 |
| kubectl | 客户端命令行工具 |
| kubelet | kubelet主程序 |
| kube-proxy | proxy主程序 |
| kube-scheduler | scheduler主程序 |
| kube-scheduler.docker\_tag | scheduler docker镜像的tag |
| kube-sceduler.tar | scheduler docker镜像文件 |

## 3、配置和启动kubernetes服务

kubernetes的服务都可以通过直接运行二进制文件加上启动采纳数完成，为了便于管理，常见做法是将kubernetes服务程序配置为系统开机自启服务。

### 1、etcd服务

```
sudo apt install etcd-server
sudo apt install etcd-client
```

设置为自动自动项

```
#vi /lib/systemd/system/etcd.service
[Unit]
Description=Etcd Server
After=network.service

[Service]
Type=simple
WorkingDirectory=/var/lib/etcd/
EnvironmentFile=/etc/etcd/etcd.conf
ExecStart=/etc/etcd/etcd.conf

[Install]
WanteBy=multi-user.target
```

验证etcd安装

```
etcdctl cluster-health
member 8e9e05c52164694d is healthy: got healthy result from http://localhost:2379
cluster is healthy
```

### 2、kube-apiserver服务

将kube-apiserver可执行文件复制到/usr/bin目录

编辑systemd服务文件 /lib/systemd/system/kube-apiserver.service，内容如下

```
#vi /lib/systemd/system/kube-apiserver.service
[Unit]
Description=Kubernetes API Server
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=etcd.service
Wants=etcd.service

[Service]
EnvironmentFile=/etc/kubernetes/apiserver
ExecStart=/usr/bin/kube-apiserver $KUBE_API_ARGS
Restart=on-failure
LimitNOFILE=65535

[Install]
WanteBy=multi-user.target
```

配置文件 /etc/kubernetes/apiserver的内容包括了kube-apiserver的全部启动参数，主要的配置参数变量在KUBE\_\_API\_\_ARGS中指定

```
KUBE_API_ARGS="--etcd_servers=http://127.0.0.1:2379
--insecure-bind-address=0.0.0.0 --insecure-port=8080
--service-cluster-ip-range=169.168.0.0/16 --service-node-port-range=1-65535
--admission_control=NamespaceLifecyce,LimitRange,SecurityContextDeny,
ServiceAcount,ResourceQuota --log-dir=/var/og/kubernetes --v=2"
```

对启动参数的说明如下

* --etcd\_services：指定etcd服务的URL
* --insecure-bind-address：apiserver绑定主机的非安全IP地址，设置0.0.0.0b表示绑定所有IP地址
* --insecure-port：apiserver绑定主机的非安全端口号，默认为8080
* --service-cluster-ip-range：kubernetes集群中Service可映射的屋里机端口号范围，默认为3000~32767
* --admission\_control：Kubernetes集群的准入控制设置，各公职模块以插件的形式依次生效
* --logtostderr：设置为false表示将日志写入文件，不写入stderr
* --log-dir：日志目录
* --v：日志级别

### 3、kube-controller-manager服务

kube-controller-manager服务依赖于kube-apiserver服务

```
#cat /lib/systemd/system/kube-controller-manager.service
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=kube-apiserver.service
Requires=kube-apiserver.service

[Service]
EnvironmentFile=/etc/kubernetes/controller-manager
ExecStart=/usr/bin/kube-controller-manager $KUBE_CONTROLLER_MANAGER_ARGS
Restart=on_failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

配置文件/etc/kubernetes/controller-managerc的内容包含了kube-controller-manager的全部启动参数，主要的配置参数在变量KUBE\__CONTROLLER\_MANAGER\_ARGS 中指定_

```
#cat /etc/kubernetes/controller-manager
KUBE_CONTROLLER_MANAGER_ARGS="--master=https://192.168.18.3:8080
--logostderr=false --log-dir/var/log/kubernetes --v=2"
```

启动参数说明如下

* --master：指定apiserver的URL地址
* --logtostderr：设置为false表示将日志写入文件，不写入stderr
* --log-dir：日志目录
* --v：日志级别

### 4、kube-scheduler服务

kube-scheduler服务也依赖于kube-apiserver服务

```
#cat /lib/systemd/system/kube-controller-manager.service
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=kube-apiserver.service
Requires=kube-apiserver.service

[Service]
EnvironmentFile=/etc/kubernetes/scheduler
ExecStart=/usr/bin/kube-scheduler $KUBE_SCHEDULER_ARGS
Restart=on_failure
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

配置文件 /etc/kubernetes/scheduler的内容包括了kube-scheduler的全部启动参数，主要的配置参数在变量 KUBE\_\_SCHEDULER\_\_ARGS中指定

```
#cat /etc/kubernetes/scheduler
KUBE_SCHEDULER_ARGS="--master=http://192.168.18.3:8080 --logtostderr=false
--log-dir=/var/log/kubernetes --v=2"
```

启动参数说明如下

* --master：指定apiserver的URL地址
* --logtostderr：设置为false表示将日志写入文件，不写入stderr
* --log-dir：日志目录
* --v：日志级别

### 5、启动服务

配置完成之后，执行systemctl start命令依次启动上述服务。使用systemctl enable命令jian个服务加入开机启动列表中

```
#systemctl daemon-reload
#systemctl enable kube-apiserver.service
#systemctl start kube-apiserver.service
#systemctl enable kube-controller-manager.service
#systemctl start kube-controller-manager.service
#systemctl enable kube-scheduler.service
#systemctl start kube-scheduler.service
```

执行 systemctl ststus &lt;service\_name&gt; 来验证服务状态

6、kubelet服务

kubelet服务依赖于Docker服务

```
#cat /lib/systemd/system/kubelet.service
[Unil]
Description=kubernetes Kubelet Server
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=docker.service
Requires=docker.service

[Service]
WorkingDirectory=/var/lib/kubelet
EnvironmentFile=/etc/kubernetes/kubelet
ExecStart=/usr/bin/kubelet $KUBELET_ARGS
Restart=on_failure

[Install]
WantedBy=nulti-use.target
```

其中，WorkingDirectory表示kubelet保存数据的目录，需要在启动kubelet服务之前进行创建

