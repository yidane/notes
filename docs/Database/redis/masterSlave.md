# redis集群搭建

## 主从复制

* 创建redis网络

> ```bash
> docker network create --subnet=172.20.0.0/16 redis-network
> ```

* 搭建redis节点

> ```bash
> docker run --name master --network redis-network --ip 172.20.0.20 -d redis
> docker run --name slave1 --network redis-network --ip 172.20.0.21 -d redis
> docker run --name slave2 --network redis-network --ip 172.20.0.22 -d redis
> docker run --name slave3 --network redis-network --ip 172.20.0.23 -d redis
> ```

* 设置主从复制

依次登录各从节点172.20.0.21,172.20.0.22,172.20.0.23

> ```bash
> redis-cli -h 172.20.0.21 -p 6379
> slaveof 172.20.0.20 6379
> ```

* 测试

> ```bash
> # 登录master
> redis-cli -h 172.20.0.20 -p 6379
> # 执行命令
> set key1 value1
>
> # 登录从节点，查看数据已经复制
> ```

* 缺点

> **当从节点断开后，需要重新复制数据，全量复制**

* 删除所有测试节点

> ```bash
> docker stop master slave1 slave2 slave3 && docker rm master slave1 slave2 slave3
> ```

## Sentinel

> 重复主从复制中1-3步骤

* 设置Sentinel

> ```bash
> # 登录
> redis-cli -h 172.17.0.3 -p 6379
> ```
