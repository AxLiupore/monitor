# Linux 分布式性能分析监控系统

## 构建

我使用的 Linux 版本是：6.5.2-arch1-1，如是要构建本项目，至少需要 4.15，存储大小至少为 30G；因为项目需要用到 docker，所以内存至少需要 4G

在构建本项目的时候，项目代码不能在 Windows 上下载后拷贝到 Linux 环境中，Windows 会改变代码中的可执行文件的权限和格式，造成项目构建的失败

首先下载所需要的 qt 资源包：https://download.qt.io/archive/qt/5.12/5.12.9/qt-opensource-linux-x64-5.12.9.run，将这个包放到：`/home/axliu/monitor/docker/build/install/qt`  的目录下

### 构建项目镜像

通过 docker 中的 dockerfile 文件，构建项目镜像

首先进入到 docker 构建下的目录

```
cd /home/axliu/monitor/docker/build
```

之后执行下面代码，这里会等待一段时间，因为构建的镜像很大

```
docker build --network host -f base.dockerfile .
```

构建完毕后查看镜像 id

```
docker images
```

![images](https://github.com/AxLiupore/monitor/blob/master/images/images.png)

命名镜像 id 为：linux:monitor

```
docker tag IMAGEID linux:monitor
```

### 使用 docker 容器

进入 docker 容器

```
cd /home/axliu/monitor/docker/scripts
```

启动 docker 容器

```
./monitor_docker_run.sh
```

进入容器

```
./monitor_docker_into.sh
```

### 编译代码

```
cd work
```

![work](https://github.com/AxLiupore/monitor/blob/master/images/work.png)

```
cd cmake
cmake ..
make -j8
```

### 启动服务

#### 启动 grpc 服务

在当前终端启动 grpc 服务

```
cd rpc_manager/server
./server
```

#### 启动 monitor 服务

另开一个终端去进入 docker

```
cd /home/axliu/monitor/docker/scripts
./monitor_docker_into.sh
```

在 docker 容器里面进入到下面目录中

```
cd work/cmake/test_monitor/src
```

启动服务

```
./monitor
```

#### 启动 qt 展示

```
cd /home/axliu/monitor/docker/scripts
./monitor_docker_into.sh
```

从 docker 容器里面进入下面目录中

```
cd work/cmake/display_monitor
```

运行 qt 的展示页面

```
./display
```

## 模块概述

