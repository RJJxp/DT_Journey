# Docker

## Docker安装
1. 按照 [Docker Engine链接](https://docs.docker.com/engine/install/ubuntu/) 安装即可, 不用参考Docker Desktop 的安装

2. 安装完成后, 按照 [Docker安装之后的操作](https://docs.docker.com/engine/install/linux-postinstall/) 进行权限设置

## image
通过`docker run`命令, 运行镜像, 生成容器
- -it: 这是 -i 和 -t 两个选项的组合
-  -i 保持容器的标准输入开启, -t 表示以/bin/bash进入容器, 结束终端后, 容器stop, docker ps看不到
- -d 后台运行容器, docker ps命令可以看到
- --name 启动后容器的名称
- --mount type=bind, source=, dst= 设置映射目录
- -w 设置容器工作目录, 通过/bin/bash进入容器时的路径
- map_editor:latest 为镜像名称

```bash
docker run --env--it --name map_dataflow --mount type=bind,source=$(pwd),dst=/workspace/app -w /workspace/app map_editor:latest /bin/bash
```

- 镜像创建容器时, 通过命令行设置容器的环境变量
```bash
docker run -e "VAR1=value1" -e "VAR2=value2" IMAGE_NAME
```

- 镜像创建容器时, 通过读取文件设置容器环境变量
```bash
docker run --env-file <env.list> IMAGE_NAME
```
env.list一般目录结构如下, 没有双引号
```txt
VAR1=value1
VAR2=value2
```

## container
CONTAINER 可以是ID也可以是NAME
```bash
# 获取活动container的信息
docker ps
# 获取所有container信息, 包括停止的
docker ps -a
# 启动容器
docker start CONTAINER
# 停止容器
docker stop CONTAINER
# 删除容器, 一般先停止, 再删除
docker rm CONTAINER
```