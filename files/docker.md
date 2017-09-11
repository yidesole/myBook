# Docker

### docker概念

- docker里有镜像和容器的概念

- 镜像概念:可以把镜像看做类(java类 JavaScript类)

- 容器概念:可以把容器看做实例(及new java对象, new JavaScript对象)

- 镜像和容器的关系: Image container = new Image();  容器是基于镜像创建的


### docker的应用

以ubuntu镜像为例： 这个镜像被称为基础镜像，及docker官方提供的（看做它就是个ubuntu的虚拟机）只是一个裸机

以上说了ubuntu只是个镜像，我们是不能直接用的 我们最终用到的是容器及"镜像new出来的东西"

### 我的第一个容器

- 我们如何"new"一个容器那？

#### 搜索镜像

- 首先查看docker的镜像仓库中是否有ubuntu这个镜像

```
➜  ~ docker search ubuntu
NAME                                                   DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
ubuntu                                                 Ubuntu is a Debian-based Linux operating s...   6509      [OK]
dorowu/ubuntu-desktop-lxde-vnc                         Ubuntu with openssh-server and NoVNC            128                  [OK]
rastasheep/ubuntu-sshd                                 Dockerized SSH service, built on top of of...   97                   [OK]
ansible/ubuntu14.04-ansible                            Ubuntu 14.04 LTS with ansible                   86                   [OK]
ubuntu-upstart                                         Upstart is an event-based replacement for ...   77        [OK]

```

上图中我们可以看到有我们需要的ubuntu镜像 接下来就把它拉去到本地吧！



#### 拉取镜像
要从docker的镜像仓库中拉去ubuntu这个镜像到本地

```
➜  ~ docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
d5c6f90da05d: Downloading [==========================>                        ]  25.18MB/47.26MB
1300883d87d5: Download complete
c220aa3cfc1b: Download complete
2e9398f099dc: Download complete
dc27a084064f: Download complete
```

#### 查看拉取的镜像

```
➜  ~ docker images
REPOSITORY                                       TAG                 IMAGE ID            CREATED             SIZE
ubuntu                                           latest              ccc7a11d65b1        3 weeks ago         120MB
mysql/mysql-server                               latest              3157d7f55f8d        5 weeks ago         241MB
registry.cn-hangzhou.aliyuncs.com/haomo/mdexam   zf                  e00964020355        2 months ago        1.64GB
busybox                                          latest              c30178c5239f        2 months ago        1.11MB
juu                                              latest              452596e4f289        2 months ago        118MB
yd/mysql                                         latest              4e84d647f316        3 months ago        586MB
nginx                                            latest              958a7ae9e569        3 months ago        109MB
ubuntu                                           <none>              ebcd9d4fca80        3 months ago        118MB
mysql
```
在刚装的情况下应该只显示一个ubuntu镜像 其他镜像是我装的可以忽略

#### 创建第一个容器

```
➜  ~ docker run -i -t ubuntu /bin/bash
root@c6d35db61f33:/# exit
```
上面用run命令创建一个给予ubuntu镜像的容器 -i -t参数是调出容器内的shell可以与容器进行交互 exit 退出容器（退出及关闭容器）

#### 查看已启动的容器

```
➜  ~ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
docker ps 是查看已启动的容器 加个-a参数查看全部的容器(包括没启动的)

#### 查看所有容器

```
➜  ~ docker ps -a
CONTAINER ID        IMAGE                                               COMMAND                  CREATED             STATUS                      PORTS                               NAMES
3b7409112eb8        ubuntu                                              "/bin/bash"              5 minutes ago       Exited (0) 5 minutes ago                                        sharp_williams
```

我们可以看到的信息有这个容器的id(3b7409112eb8) 给予哪个镜像创建的(ubuntu) 还有就是这个容器的名称(sharp_williams)名称是随机分配的

#### 启动容器

```
➜  ~ docker start c6d35db61f33
c6d35db61f33
➜  ~ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c6d35db61f33        ubuntu              "/bin/bash"         7 minutes ago       Up 4 seconds                            silly_aryabhata
```
- docker start c6d35db61f33(容器id) 启动容器 可以使用docker ps -a 查看容器id

- docker ps 查看我们刚刚启动的容器

#### 关闭容器
```
➜  ~ docker stop c6d35db61f33
c6d35db61f33
➜  ~ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
➜  ~
```
- docker stop c6d35db61f33(容器id)  关闭这个容器

- docker ps 查看已启动的容器这里显示没有,及已被关闭

#### 与容器进行交互

```
➜  ~ docker start c6d35db61f33
c6d35db61f33
➜  ~ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c6d35db61f33        ubuntu              "/bin/bash"         14 minutes ago      Up 9 seconds                            silly_aryabhata
➜  ~ docker exec -i -t c6d35db61f33 /bin/bash
root@c6d35db61f33:/#
```

- 要与容器进行交互首先要保证这个容器是启动的，docker start c6d35db61f33 运行id为c6d35db61f33的容器

- docker ps 查看已启动的容器

- docker exec -i -t c6d35db61f33 /bin/bash  调出容器的shell与容器进行交互 在容器内新开一个shell交互退出时exit不退出容器

#### 创建一个指定名称为testDocker的容器
```
➜  ~ docker run -i -t --name testDocker ubuntu /bin/bash
root@31fd4713f7da:/#
➜  ~ docker ps -a
CONTAINER ID        IMAGE                                               COMMAND                  CREATED             STATUS                      PORTS                               NAMES
31fd4713f7da        ubuntu                                              "/bin/bash"              31 seconds ago      Exited (0) 14 seconds ago                                       testDocker
```
- --name参数可以指定创建容器的名称

#### 创建一个后台进行的容器

```
➜  ~ docker run--name testDocker -d ubuntu /bin/bash
2b6be9c242276b3969d6b8e36f5d07337f770bfe6c64a43fb51054824e995c67
➜  ~ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
2b6be9c24227        ubuntu              "/bin/bash"         44 seconds ago      Up 43 seconds                           testDocker
```

- 上面用了-d参数，因此docker会将容器放到后台运行

#### attach 附着容器
```
➜  ~ docker attach 2b6be9c24227
root@2b6be9c24227:/#
root@2b6be9c24227:/# exit
exit
➜  ~
➜  ~ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
➜  ~
```
- docker attach 2b6be9c24227 附着到这个容器在创建时的shell交互  当exit退出时容器也跟着停止（一般用exec）

#### 查看容器日志
```
docker logs 2b6be9c24227
```
- 查看容器日志docker logs 2b6be9c24227(容器id)

#### 查看容器内部运行的进程

```
➜  ~ docker top testDocker
PID                 USER                TIME                COMMAND
3162                root                0:00                /bin/bash
➜  ~
```

- docker top testDocker(容器名/ID都可以)

```
➜  ~ docker top testDocker
PID                 USER                TIME                COMMAND
3162                root                0:00                /bin/bash
➜  ~
```

##### 自动重启容器

```
docker run --restart=on-failure:5 --name testR1 -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
```

- --restart 标志被设置为always,无论容器的退出代码是什么，Docker都会自动重启该容器,除了always ，我们还可以将这个标志设置为on-failure,这样，只有当容器的退出代码为非0值的时候，才会自动启动，另外，on-failure还接受一个可选的重启次数，如：--restart=on-failure:5


#### 深入容器

查看容器
```
➜  ~ docker inspect testR1
[
{
"Id": "553badc90f4cbb430791dad29c597c3ec46fb6d825c3951e79d73f884d404069",
"Created": "2017-09-04T07:07:10.607876379Z",
"Path": "/bin/sh",
"Args": [
"-c",
"while true; do echo hello world; sleep 1; done"
],
"State": {
"Status": "running",
"Running": true,
"Paused": false,
"Restarting": false,
"OOMKilled": false,
..........
```

docker inspect 容器名称 查看容器构造

#### 删除容器

```
➜  ~ docker rm 2b6be9c24227
```

- docker rm 容器id或者容器名都可以，注意：运行中的容器时删除不了的，必须停止之后进行删除


##### 删除镜像

```
docker rmi c90f4cbb43079
```

docker rmi 镜像名称/或者镜像id


##### 端口映射

```
docker run -it -p 80 --name test ubuntu /bin/bash
```

- -p参数是端口映射  指定容器内的80端口随机映射到宿主机的端口,随机映射的端口范围49153-65535之间

##### 映射到宿主机的指定端口

```
docker run -it -p 8080:80 --name test ubuntu /bin/bash
```

- -p 8080:80 将容器内的80端口映射到宿主机的8080端口上

##### 卷

```
docekr run -it -v /opt/tomcat8/:/opt/tomcat8/ --name test ubuntu /bin/bash
```

- -v 将宿主机的/opt/tomcat8目录挂在(共享)到容器内的/opt/tomcat8/下   (及改变容器该是改变宿主机上的tomcat8下的文件他们同时都会被改变)

##### 查看容器端口映射情况

```
docekr port test1 80
```

- 查看test1容器的80端口的映射情况(映射到宿主机的哪个端口)


###### 将文件拷贝到容器内

```
docker cp /pot/project/ c90f4cbb43079:/tomcat8/webapps/
```

- 将宿主机/pot/project/文件夹拷贝到id为c90f4cbb43079容器的/tomcat8/webapps/目录下

##### 将容器的文件拷贝到宿主机

```
docker cp c90f4cbb43079:/tomcat8/webapps/ /pot/project/
```
- 将id为c90f4cbb43079容器的/tomcat8/webapps/目录拷贝到宿主机/pot/project/文件夹下
