# Docker原理


<!--more-->

# 持续集成实战：Docker原理

- 保证开发、测试、交付、部署的环境完全一致
- 保证资源的隔离
- 启动临时的、用完即弃的环境，例如测试
- 迅速超大规模部署和扩容

## Docker的基本概念
镜像（image）
- 一个预先定义好的模板文件，Docker引擎可以按照这个模板文件启动无数个一摸一样的，互不干扰
容器（container）
- 一台虚拟的计算机，拥有独立的：
    - 网络
    - 文件系统
    - 进程
- 默认和宿主机不发生任何交互
    - 意味着数据是没有持久化的

it参数会启动docker交互式命令行模式，会显示报错信息
```
docker run -it mysql
```
## docker pull/images

下载一个指定的镜像，方便随时启动

docker pull mysql:5.7.28下载指定镜像
- 如果不加tag（版本号）默认是latest（最新版本）
- registry.cn-beijing.aliyuncs.com/dr1/hcsp:0.0.16
- 如果不加镜像仓管地址，默认在docker中央仓库获取镜像

docker images 查看本地已有的镜像

## docker run/ps

docker run 装载镜像成为一个容器、

- 通过镜像模板创建出来一个镜像实例
- 在这个容器看来，自己就是一台独立的计算机
- 每个容器有一个ID。支持缩写

docker run it <镜像名> <镜像中要运行的命令和参数>
- 交互式命令行，当前shell中运行，Ctrl-C 退出
- 启动并进入容器

docker run -d <镜像名> <镜像中要运行的命令和参数>
- daemon模式，在后台运行

## docker run

--name 为容器指定一个名字

--restart=always 遇到错误自动重启

提供统一化软件化交互必不可少的功能

-v <本地容器>:<容器文件>
-p <本地端口>:<容器端口>
-e name=value
    - 向容器传递初始化参数，例如设置root密码

镜像名之前是docker参数，镜像名之后是镜像参数 

## docker start/stop

启动/停止一个容器

## docker rm

删除一个容器

# docker exec（execute执行）

指定目标容器，进入容器执行命令
- docker run -it <目标容器ID/名字><目标命令（通常为bash）>
- 可以想象成ssh
调试、解决问题必备命令

## docker logs
docker logs <容器ID或容器名>
- 查看目标容器的输出
docker logs -f <容器ID或容器名>
- 查看目标容器最后几行的输出

## docker inspect

查看容器的详细状态

## docker分层镜像
ubuntu等镜像的基础镜像相同，在不同版本可以在新层增加功能。
### Dockerfile

指定镜像如何生成

编写第一个Dockerfile

docker build .
- 执行当前目录的Dockerfile文件

每个镜像会有一个唯一的ID

### Docker的镜像仓库与tag
可以任意对镜像进行tag操作
- 决定了未来这个镜像会被push到哪里
- 决定了未来从哪里下载镜像
可以方便的藏剑镜像仓库的私服
- -- registry-mirror
- --insecure-registry
    - 指定成http方式
## docker与kubernetes(容器编排引擎)
自动化容器编排引擎

[练习run命令]https://github.com/hcsp/practise-docker-run
