# docker-stack知识

> [!NOTE]
> 先了解docker和docker-compose知识<br>
> 如果您在本地开发环境中进行尝试，您可以使用`docker swarm init`.<br>
> 如果你已经有了一个多节点群运行，请记住，所有 `docker stack`和`docker service`命令必须从管理节点运行。
######注意
> [!WARNING]
> The `docker stack deploy` command supports any Compose file of version “3.0” or above



## 创建一个stack服务

```bash
$ docker stack deploy --compose-file docker-compose.yml stackdemo

Ignoring unsupported options: build
Creating network stackdemo_default
Creating service stackdemo_web
Creating service stackdemo_redis
```



## 检查服务

```bash
$ docker stack services stackdemo

ID            NAME             MODE        REPLICAS  IMAGE
orvjk2263y1p  stackdemo_redis  replicated  1/1       redis:3.2-alpine@sha256:f1ed3708f538b537eb9c2a7dd50dc90a706f7debd7e1196c9264edeea521a86d
s1nf0xy8t1un  stackdemo_web    replicated  1/1       127.0.0.1:5000/stackdemo@sha256:adb070e0805d04ba2f92c724298370b7a4eb19860222120d43e0f6351ddbc26f
```



## 删除服务

```bash
$ docker stack rm stackdemo

Removing service stackdemo_web
Removing service stackdemo_redis
Removing network stackdemo_default
```

