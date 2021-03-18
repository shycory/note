# docker笔记

### 前置

#### centos7安装docker

##### 官方网站

https://docs.docker.com/engine/install/centos/

##### 主要命令

> 安装`yum-utils`软件包（提供`yum-config-manager` 实用程序）并设置**稳定的**存储库

```sh
yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

> 安装*最新版本*的Docker Engine和容器，或转到下一步以安装特定版本：

```shell
yum install docker-ce docker-ce-cli containerd.io
```





#### 配置阿里云 镜像加速器



##### 1.登录阿里云,获取加速器地址

​	https://6dn4tqcz.mirror.aliyuncs.com

##### 2.执行命令

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://6dn4tqcz.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



##### 查看docker链接的仓库()

> centos6.8

```sh
ps -ef|grep docker
```

> centos7

```sh
docker info
```



#### 启动Docker

##### centos 7 以上 启动命令

``` sh
systemctl start docker
```

##### centos 6.5-7 之间 启动命令

```sh
system start docker
```

##### 测试命令

```sh
docker run hello-world
```

##### run 命令 流程图

![1615363352](picture\1615363352.jpg)



##### 开启关闭-开机启动

> 开启

``` sh
 sudo systemctl enable docker.service
 sudo systemctl enable containerd.service
```

> 关闭

``` sh
 sudo systemctl disable docker.service
 sudo systemctl disable containerd.service
```



### 常用命令

#### 1.帮助命令

##### docker version

Client: Docker Engine - Community
 Version:           20.10.5
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        55c4c88
 Built:             Tue Mar  2 20:33:55 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.5
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       363e9a8
  Built:            Tue Mar  2 20:32:17 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.4
  GitCommit:        05f951a3781f4f2c1911b05e61c160e9c30eaa8e
 runc:
  Version:          1.0.0-rc93
  GitCommit:        12644e614e25b05da6fd08a38ffa0cfe1903fdec
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

##### docker info

Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Build with BuildKit (Docker Inc., v0.5.1-docker)

Server:
 **Containers: **2
  Running: 0
  Paused: 0
  Stopped: 2
 **Images:** 1
 Server Version: 20.10.5
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 05f951a3781f4f2c1911b05e61c160e9c30eaa8e
 runc version: 12644e614e25b05da6fd08a38ffa0cfe1903fdec
 init version: de40ad0
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-1127.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux
 Architecture: x86_64
 CPUs: 1
 Total Memory: 972.3MiB
 Name: localhost.localdomain
 ID: 4GSX:32EJ:YX4I:LS7D:3Y5N:NGRX:2YIX:EURR:5332:GTVU:HCFW:NA7M
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Registry Mirrors:
  https://6dn4tqcz.mirror.aliyuncs.com/
 Live Restore Enabled: false

##### docker --help



#### 2.镜像命令

##### 2.1 列出本地所有镜像

---> docker images [options]

options:

- -a :列出本地所有镜像(含中间映像层)
- -q : 只显示镜像id
- --digests : 显示镜像的摘要信息
- --no-trunc : 显示完整的镜像信息 

> 无参数例子:docker images

REPOSITORY    TAG       IMAGE ID       CREATED      SIZE
hello-world   latest    d1165f221234   4 days ago   13.3kB

**同一repository可有多个tag,代表不同版本,可用 repository:tag 定义不同镜像**

默认 latest



##### 2.2 拉取镜像

---> docker pull [镜像名称]

##### 2.3 创建镜像

- 通过容器创建

  ​	---> docker commit -m="message" -a="作者" [容器id] [镜像名称]:[标签]

- 通过Dockerfile创建

  ​	---> docker build -t [镜像名] .

  ​	(-t :指定要创建的目标镜像名

  ​	  .  :  Dockerfile文件所在目录,也可指定绝对路径)

##### 2.4 上传镜像,如阿里云

- 具体命令在阿里云上有

#### 3.容器命令

##### 3.1 运行

---> docker run [options] 镜像名/id

> -it 表示交互式.例如运行docker run -it centos 则直接进入容器的centos



> -d 守护进程的形式运行(没有前台交互会自杀)
>
> 防止自杀:docker run -d centos /bin/sh -c "while true; do echo hello world; sleep 120; done"
>
> 表示运行后每2分钟输出一下hello world;不会停了



> docker run -it -p 8080:8080 tomcat 
>
> -p :以固定端口运行容器
>
> -P:随机分配端口
>
> docker run -it tomcat



##### 3.2 列出运行的所有容器 

---> docker ps

##### 3.3 退出

> 停止容器并退出 exit
>
> 不停止容器 仅推出 :ctrl+p+q

##### 3.4 停止容器

> docker stop 容器id/名称
>
> docker kill 容器id/名称(强制)

##### 3.5 删除容器

> docker rm [-f] 容器id
>
> -f 未停止强制删除

> 一次删除多个容器
>
> docker rm -f $(docker ps -a -q)
>
> docker ps -a -q | xargs docker rm

##### 3.6 查看日志

> docker  logs [options] --tail 容器id
>
> - -t 是加入时间戳
> - -f 跟随最新的日志打印
> - --tail 数字  显示最后多少条

##### 3.7 查看容器细节

---> docker inspect 容器id

> 以json串的形式,显示容器信息

##### 3.8 进入容器

---> docker attach 容器id

##### 3.9 容器外让容器执行命令

> docker exec -t 容器id  ls -l /tmp

##### 3.10 拷贝容器内的数据

> docker cp 容器id: 容器内路径 目的主机路径

##### 3.11 根据容器创建镜像

> docker commit -m="提交的描述信息" -a="作者" 容器id 要创建的目标镜像名:[标签名]



#### 容器卷

##### 作用: 保存容器的数据

> docker run -it -v /myData: /dataContainer centos
>
> 本机的myData文件和容器中的dataContainer 数据共享



#### DockerFile 

+ 本质就是构建镜像的脚本
+ 使用方式

> 手写dockerfile 文件,
>
> 使用 docker build 命令执行,获得一个自定义的镜像
>
> run

##### 保留字

- FROM --- 类似java继承 (FROM scratch 类似与继承基类)
- MAINTAINER --- 作者和邮箱
- RUN --- 容器构建时需要运行的命令
- EXPOSE --- 当前容器暴露接口
- WORKDIR --- 指定在创建容器后,终端默认进来的路径
- ENV ---  用来在构建镜像过程中设置环境变量
- ADD --- 将宿主机目录下的文件拷贝进镜像且ADD 命令会自动处理URL和解压tar压缩包
- COPY --- 类似ADD ,拷贝文件和目录到镜像中.将从构建上下文目录中<源路径>的文件/目录复制到新的一层的镜像内<目标路径>位置
- VOLUME --- 容器数据卷,用于容器运行时的数据保存和持久化
- CMD --- 指定一个容器启动时要运行的命令,(dockerfille中可有多个cmd命令,但只有最后一个生效,cmd会被docker run之后的参数替换,默认命令)
- ENTRYPOINT --- 同上,但这个不是默认命令   .**docker run 会追加**

- ONBUILD --- 类似与父类构造器,有子镜像继承后必被触发;

