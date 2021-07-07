# 1、创建swarm

```
docker swarm init --advertise-addr <MANAGER-IP>
```

例如：docker swarm init --advertise-addr 192.168.3.12

# 2、node操作

## 查看node

```
$ docker node ls


ID                           HOSTNAME  STATUS  AVAILABILITY  MANAGER STATUS
dxn1zf6l61qsb1josjja83ngz *  manager1  Ready   Active        Leader

```

## node-inpect

```
$ docker node inspect --pretty manager1
```

## 添加node到swarm

```
$ docker swarm join \
  --token  SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
  192.168.99.100:2377

This node joined a swarm as a worker.
```

### **如果忘记了token**

```
$ docker swarm join-token worker

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377
```

## 

## 停止一个node

```
docker node update --availability drain worker1
```

## 再加入该node

```
docker node update --availability active worker1
```





# 3、服务操作

## 发布一个服务

```
docker service create --replicas 1 --name helloworld alpine ping docker.com
```

## 查看服务列表

```
$ docker service ls

ID            NAME        SCALE  IMAGE   COMMAND
9uk4639qpg7n  helloworld  1/1    alpine  ping docker.com
```

## 查看服务的细节

```
docker service inspect --pretty helloworld
```

## 查看服务运行的节点

```
[manager1]$ docker service ps helloworld

NAME                                    IMAGE   NODE     DESIRED STATE  CURRENT STATE           ERROR               PORTS
helloworld.1.8p1vev3fq5zm0mi8g0as41w35  alpine  worker2  Running        Running 3 minutes
```

**注意：**

​	服务运行节点可以使用docker ps 查看该容器详情。



## 改变服务运行的容器数量

```
[manager1]$  docker service scale <SERVICE-ID>=<NUMBER-OF-TASKS>

例如 docker service scale helloworld=5

helloworld scaled to 5
```

## 删除一个服务

```
docker service rm helloworld
```

查看

```
$ docker service inspect helloworld
[]
Error: no such service: helloworld
```

## 循环更新服务

```
docker service update --image redis:3.0.7 服务名称
update后面可以接各种不同的修改内容
```



## 开放服务的内部端口

### 创建时给出：

```
$ docker service create \
  --name <SERVICE-NAME> \
  --publish published=<PUBLISHED-PORT>,target=<CONTAINER-PORT> \
  <IMAGE>
```

可以使用简写  -p 8080:80

### 或者之后更新：

```
$ docker service update \
  --publish-add published=<PUBLISHED-PORT>,target=<CONTAINER-PORT> \
  <SERVICE>
```



### 开放端口指明TCP/UDP

```
$ docker service create --name dns-cache \
  --publish published=53,target=53 \
  --publish published=53,target=53,protocol=udp \
  dns-cache
  
OR
$ docker service create --name dns-cache \
  -p 53:53 \
  -p 53:53/udp \
  dns-cache
  
 注：上述2种方式表明开通了53端口的tcp和udp访问权限。

```

