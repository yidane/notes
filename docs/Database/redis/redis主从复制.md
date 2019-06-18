# redis集群搭建

## 主从复制

* 搭建redis节点

> ```bash
> docker run --name master -d redis
> docker run --name slave1 -d redis
> docker run --name slave2 -d redis
> docker run --name slave3 -d redis
> ```

* 查看节点IP

> ```bash
> docker inspect master slave1 slave2 slave3 | grep IPAddress
> ```
>
> 假设获取到的IP为 172.17.0.3

* 设置主从复制

依次登录各从节点

> ```bash
> redis-cli -h 172.17.0.3 -p 6379
> slaveof 172.17.0.3 6379
> ```

* 测试

> ```bash
> # 登录master
> redis-cli -h 172.17.0.3 -p 6379
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
