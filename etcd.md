### Etcd的基本使用

* 介绍
```
1. etcd是一个高可用的键值存储系统,场景主要是：

1）主要用于共享配置
2）服务注册与发现
3）分布式锁

2. etcd是由CoreOS开发并维护的,灵感来自于 ZooKeeper，它使用Go语言编写

3. 下载地址
  https://github.com/etcd-io/etcd/releases（下载比较慢）
  
  http://www.jtthink.com/download/detail?did=227（备用，下载速度比较快）
```
* docker启用服务
```
1. docker下载镜像
  docker pull golang:1.12-alpine

2. 创建存放配置文件和数据的文件夹
  mkdir -p etcd/conf etcd/data

3. 启动容器
  docker run -it --name etcd \
  -p 2379:2379 \
  -v /home/yang/etcd:/etcd \
  golangd:1.12-alpine sh （注意 -d 不能启动服务）
  
4. 拷贝etcd的服务端和客户端到容器（进入到etcd解压后的文件夹）
  docker cp etcd etcd:/usr/bin && docker cp etcdctl etcd:/usr/bin 

5. 创建etcd.yml配置文件
  name: $(hostname -s)
  data_dir: /etcd/data
  listen-client-urls: http://0.0.0.0:2379

6. 启动etcd
  1、docker exec -it etcd sh (进入容器)
  2、chmod +x /usr/bin/etcd  (增加可执行权限)
  3、etcd --version  查看版本
  4、 etcd --config-file  /etcd/conf/etcd.yml
```
* etcd的基本使用一
```
1. 介绍
  Etcdctl是客户端工具，由于历史原因，存在v2和v3版本。

2. 设置环境变量（版本3）
  export ETCDCTL_API=3

3. 查看版本
  etcdctl version

4. 增删改查
   1）查
   etcdctl get /user/101/name  
   etcdctl get /user/101/age
   etcdctl get /user/101 --prefix  //查询前缀为/user/101的所有键的值
  
   2）增
   etcdctl put /user/101/name shenyi
   etcdctl put /user/101/age  19
   
   3）删
   etcdctl del /user/101 --prefix （删除这个前缀的键）
   etcdctl del /user/101/age
```