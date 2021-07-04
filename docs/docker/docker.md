## 一、docker 镜像

### 创建镜像

```
创建一个DockFile文件
```

```
docker build -t omiga:0.0.1 -f DockerFile
注释：-t 打标记 名称:版本
	 -f 指定dockerFile文件 如果是当前文件家，可以 . 代替
```

### 拉取镜像

```
docker pull
```

### 推送镜像

```
docker push
```

### 运行镜像

```
docker run -d -p 80:80 docker/getting-started

注释： -p 0.0.0.0:8080:80 宿主机端口8080监听容器内80端口。
		-i -t   tty
		--name my-redis  指明容器名称
```

### 删除镜像

```
docker rmi omiga:0.0.1  或者  docker rmi 镜像id
```

### 查看镜像

```
docker images 
可结合grep过滤
```

## 二、docker 容器

### 查看运行的容器

```
docker ps
```

### 停止容器

```
docker stop 容器id
```

### 删除容器

```
docker rm 容器id 
注意：容器必须先停止
```

