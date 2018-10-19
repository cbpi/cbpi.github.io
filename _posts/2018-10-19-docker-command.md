---
layout: post
title: Docker常用命令
date: 2018-10-19
category: 工具
comments: true
excerpt: 列举docker里面常用的一些命令，镜像操作，容器操作等等。
tags: [tools]
---

> 根据搜索的文献，官网的文档，列举了使用docker时常用的一些命令


### 查看docker基本信息

```javascript
docker --version                #查看版本
docker-compose --version        #查看版本
docker-machine --version        #查看版本
docker version                  #查看client和server端版本，并可以查看是否开启体验功能
docker info                     #显示docker系统的信息
docker logs                     #日志信息
service docker status           #故障检查
sudo service docker start|stop  #启动关闭docker
```

### 检查

```javascript
docker ps                            #查看当前正在运行的image实例
docker ps -a                         #查看所有镜像实例
docker run hello-world               #验证docker是否在运行中
docker inspect <task or container>   #检查任务或容器
```

### 镜像(image)操作

```javascript
docker build -t <image-name> .                      #使用当前目录下的Dockerfile构建镜像
docker images                                       #查看镜像
docker image ls -a                                  #显示机器上所有的镜像
docker image rm <image id>                          #删除指定的镜像
docker image rm $(docker image ls -a -q)            #删除所有的镜像
docker rmi [image-id/image-name]                    #删除指定的镜像，如docker rmi nginx
docker tag <image> <username>/<repository>:<tag>    #为自定义的镜像打上tag。如：$docker tag hellopython followtry/demo:latest
docker push <username>/<repository>:<tag>           #将自定义的镜像发布到仓库。如：docker push followtry/demo:latest
    上传后访问地址：https://cloud.docker.com/swarm/followtry/repository/docker/followtry/demo/general
docker pull <username>/<repository>                 #pull自定义的上传上去的镜像。如：$docker pull followtry/demo
docker run username/repository:tag                  #运行仓库的镜像
```

### 容器(container)操作

```javascript
docker container ls                                #列出所有运行中的容器
docker container ls -a                             #列出所有容器，包括未运行的
docker container ls -q                              #只列出运行的容器的id集合
docker container stop <hash>                       #优雅停用指定的容器
docker container kill <hash>                       #强制关闭指定的容器
docker container rm <hash>                         #删除指定的容器
docker container rm $(docker container ls -a -q)   #删除所有的容器
docker run -d -p 8080:80 --name webserver nginx    #运行nginx镜像实例，-d：后台，-p:绑定端口8080到docker的80
docker stop <containerid/container-name>           #停止容器webserver
docker start <containerid/container-name>          #启动容器webserver
docker port <containerid/container-name>           #查看指定容器的端口映射
docker logs -f <containerid/container-name>        #查看指定容器的日志
docker top <containerid/container-name>            #查看容器的进程
docker inspect <containerid/container-name>        #检查容器的底层信息
docker rm <hash>                                   #从此机器中删除指定的容器
docker rm $(docker ps -a -q)                       #从此机器中删除所有容器
docker kill <hash>                                 #强制关闭指定的容器
```
参考文献 [https://docs.docker-cn.com/](https://docs.docker-cn.com/)   
参考文献 [Docker常用命令](https://segmentfault.com/a/1190000012063374)