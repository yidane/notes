# Install [RocketMQ](https://rocketmq.apache.org/docs/quick-start/)

## Prerequisite

>* 64bit OS, Linux/Unix/Mac is recommended
>* 64bit JDK 1.8+
>* Maven 3.2.x
>* Git
>* 4g+ free disk for Broker server

### Install 64bit JDK 1.8+

```bash
sudo yum install java-1.8.0-openjdk
sudo yum install java-1.8.0-openjdk-devel
```

### Install Maven 3.2.x

```bash
wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
yum -y install apache-maven
```

### Install Git

```
yum install git
```

## Download & Build from Release

### Download

```bash
cd /export/rocketmq
wget https://archive.apache.org/dist/rocketmq/4.7.0/rocketmq-all-4.7.0-source-release.zip
```

### Build

```
  unzip rocketmq-all-4.7.0-source-release.zip
  cd rocketmq-all-4.7.0/
  mvn -Prelease-all -DskipTests clean install -U
  cd distribution/target/rocketmq-4.7.0/rocketmq-4.7.0
```

## Start

### Start Name Server

```
nohup sh bin/mqnamesrv &
tail -f ~/logs/rocketmqlogs/namesrv.log
# The Name Server boot success...
```

### Start Broker

```
nohup sh bin/mqbroker -n localhost:9876 &
tail -f ~/logs/rocketmqlogs/broker.log
```
## Test Producer & Consumer
### Send & Receive Message

```
export NAMESRV_ADDR=localhost:9876
sh bin/tools.sh org.apache.rocketmq.example.quickstart.Producer
# SendResult [sendStatus=SEND_OK, msgId= ...

sh bin/tools.sh org.apache.rocketmq.example.quickstart.Consumer
# ConsumeMessageThread_%d Receive New Messages: [MessageExt...
```

### Shutdown Servers

```
sh bin/mqshutdown broker
# The mqbroker(36695) is running...
# Send shutdown request to mqbroker(36695) OK

sh bin/mqshutdown namesrv
# The mqnamesrv(36664) is running...
# Send shutdown request to mqnamesrv(36664) OK
```



