# docker-compose知识

## docker-compose.yml

> [!COMMENT]
>
> Compose文件是一个定义服务`services`、网络`networks`和卷`volumes`的YAML文件，默认路径是`./docker-compose.yml`，可使用`.yml`或`.yaml`作为文件扩展名。
>
> 服务`services`定义包含应用于为该服务启动的每个容器的配置，类似传递命令行参数一样`docker container create`。同样，网络`networks`和卷`volumes`的定义类似于`docker network create`和`docker volume create`。正如`docker container create`在`Dockerfile`指定选项，如`CMD`、`EXPOSE`、`VOLUME`、`ENV`，在默认情况下，不需要在`docker-compose.yml`配置中再次指定。可以使用Bash类`${VARIABLE}`语法在配置值中使用环境变量。
>
> 标准配置文件应该包含`version`、`services`、`networks`三部分，其中最关键的是`services`和`networks`两个部分。



```
service:	定义服务
	web:	二级标签,服务名称,用户自己定义
		image:	指定服务镜像的名称或ID，如果本地镜像不存在，Compose将会尝试拉取这个镜像
		格式：	image: redis
				image: ubuntu:14.04
				image: tutum/influxdb
				image: example-registry.com:4000/postgresql
				image: a4bc65fd
		build:	服务除了可以基于指定的镜像，还可以基于一份 Dockerfile，
				在使用 up 启动之时执行构建任务，这个构建标签就是 build，
				它可以指定 Dockerfile 所在文件夹的路径。Compose 将会利用它自动构建这个镜像，
				然后使用这个镜像启动服务容器。
		格式：	build: /path/to/build/dir
				build: ./dir	相对路径
				build:			设定上下文根目录，然后以该目录为准指定 Dockerfile
					context: ../
					dockerfile: path/of/Dockerfile
    ###ARG:是在build的时候存在的,可以在Dockerfile中当做变量来使用,ARG是允许空值的,构建成功之后会消失
    ###ENV:是容器构建好之后的变量,不能再Dockerfile中当参数使用,不允许空值
				build:
					context: .		context:上下文
					args:
						buildno: 1
						password: secret
				build:
					context: .
					args:
						- buildno=1
						- demo 			空值
						- password=secret
				ports:		映射端口的标签
				格式：
					ports:
						- "3000"
						- "8000:8000"
    ###YAML 的布尔值（true, false, yes, no, on, off）
    ###必须要使用引号引起来（单引号、双引号均可），否则会当成字符串解析。
		command:	使用 command 可以覆盖容器启动后默认执行的命令。
		格式:
			command: bundle exec thin -p 3000
			command: [bundle, exec, thin, -p, 3000]		这是dockerfile中的文件格式
		container_name: 虽然可以自定义项目名称、服务名称，但是如果你想完全控制容器的命名，
						可以使用这个标签指定
		格式：
			container_name: app
		depends_on:		解决容器启动先后顺序问题，避免了容器依赖因启动顺序不同而造成退出错误
		格式：
			depends_on:
				- db		先启动数据库在启动redis
				- redis
```



## docker-compose命令

```bash
Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f <arg>...] [--profile <name>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE             Specify an alternate compose file
                              (default: docker-compose.yml)
  -p, --project-name NAME     Specify an alternate project name
                              (default: directory name)
  --profile NAME              Specify a profile to enable
  --verbose                   Show more output
  --log-level LEVEL           Set log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
  --no-ansi                   Do not print ANSI control characters
  -v, --version               Print version and exit
  -H, --host HOST             Daemon socket to connect to

  --tls                       Use TLS; implied by --tlsverify
  --tlscacert CA_PATH         Trust certs signed only by this CA
  --tlscert CLIENT_CERT_PATH  Path to TLS certificate file
  --tlskey TLS_KEY_PATH       Path to TLS key file
  --tlsverify                 Use TLS and verify the remote
  --skip-hostname-check       Don't check the daemon's hostname against the
                              name specified in the client certificate
  --project-directory PATH    Specify an alternate working directory
                              (default: the path of the Compose file)
  --compatibility             If set, Compose will attempt to convert deploy
                              keys in v3 files to their non-Swarm equivalent

Commands:
  build              Build or rebuild services
  bundle             Generate a Docker bundle from the Compose file
  config             Validate and view the Compose file
  create             Create services
  down               Stop and remove containers, networks, images, and volumes
  events             Receive real time events from containers
  exec               Execute a command in a running container
  help               Get help on a command
  images             List images
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pull service images
  push               Push service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  top                Display the running processes
  unpause            Unpause services
  up                 Create and start containers
  version            Show the Docker-Compose version information
```

> [!TIP]
>
> 必须指定-f 指定docker-compose.yml文件或者在同根目录下执行如下命令

### 启动一个docker-compose

```sh
docker-compose up -d 
```

### 停止一个docker-compose

```sh
docker-compose down
```

### 列出一个docker-compose服务信息

```sh
docker-compose ps
```

### docker-compose logs

查看服务容器的输出，默认情况下`docker-compose`将对不同的服务输出使用不同的颜色来区分。可以通过`--no-color`来关闭颜色。

```css
docker-compose logs [options] [SERVICE...]
```

例如：

```sh
root@default:/var/www/swoft# docker-compose logs
Attaching to swoft_swoft_1
swoft_1  | root@cd054651dfcb:/var/www/swoft# exit
```

### docker-compose restart

重启项目中的服务

```css
docker-compose restart [options] [SERVICE...]
```

命令选项`[options]`

- `-t, --timeout TIMEOUT`指定重启前停止容器的超时时长，默认为10秒。



### 其他命令

```
#删除所有（停止状态的）服务容器。推荐先执行 docker-compose stop 命令来停止容器。
docker-compose rm 

#在指定服务上执行一个命令。
docker-compose run ubuntu ping docker.com

#设置指定服务运行的容器个数。通过 service=num 的参数来设置数量
docker-compose scale web=3 db=2

#启动已经存在的服务容器。
docker-compose start

#停止已经处于运行状态的容器，但不删除它。通过 docker-compose start 可以再次启动这些容器。
docker-compose stop
```
