

# Docker

## Docker加速
在/etc/docker/daemon.json加入
{"registry-mirrors":["https://reg-mirror.qiniu.com/"]}
## Docker服务
sudo service docker restart/start/stop

## Docker镜像

### 获取镜像

```
docker pull 镜像名
```

## 查找镜像

```
docker search 镜像名
```

### 查看本地镜像

```
docker images
```

### 删除镜像

```
docker rmi 镜像名
```

### 更新镜像

```
docker commit -m="提交的描述信息" -a="镜像作者" 容器ID 用户名/更新后镜像名:版本
```

### 构建镜像

dockerfile：

FROM：基础镜像，当前新镜像是基于哪个镜像的

MAINTAINER：镜像维护者的姓名和邮箱地址

RUN：容器构建时需要运行的命令

EXPOSE：当前容器对外暴露出的端口

WORKDIR：指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

ENV：用来在构建镜像过程中设置环境变量

ADD：将宿主机目录下的文件拷贝进镜像且 ADD 命令会自动处理 URL 和解压 tar 压缩包

COPY：类似 ADD，拷贝文件和目录到镜像中。（COPY src dest 或 COPY ["src","dest"]）

VOLUME：容器数据卷，用于数据保存和持久化工作

CMD：指定一个容器启动时要运行的命令，Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数替换

ENTRYPOINT：指定一个容器启动时要运行的命令，ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数

ONBUILD：当构建一个被继承的 Dockerfile 时运行命令，父镜像在被子继承后父镜像的 onbuild 被触发

eg：

```
FROM centos
MAINTAINER jhxxb

# 把宿主机当前目录下的 jdk1.8.0_221 拷贝到容器 /usr/local/ 路径下
COPY jdk1.8.0_221/ /usr/local/jdk1.8.0_221/

# 把宿主机当前目录下的 tomcat 添加到容器 /usr/local/ 路径下
ADD apache-tomcat-9.0.24.tar.gz /usr/local/

# 安装vim编辑器
RUN yum -y install vim

# 设置工作访问时候的 WORKDIR 路径，登录落脚点
ENV MYPATH /usr/local
WORKDIR $MYPATH

# 配置 java 与 tomcat 环境变量
ENV JAVA_HOME /usr/local/jdk1.8.0_221
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.24
ENV CATALINA_BASE /usr/local/apache-tomcat-9.0.24
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

# 容器运行时监听的端口
EXPOSE  8080

# 启动时运行 tomcat
# ENTRYPOINT ["/usr/local/apache-tomcat-9.0.24/bin/startup.sh" ]
CMD ["/usr/local/apache-tomcat-9.0.24/bin/catalina.sh","run"]
# CMD /usr/local/apache-tomcat-9.0.24/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.24/bin/logs/catalina.out
```

```
docker build -f mydockerfile -t mytomcat:0.1 .
```

### 设置镜像标签

```
docker tag 镜像ID 用户名/镜像名:新的标签名
```



## 容器

### 启动容器

```
docker run -itd -P 127.0.0.1:5001:5000 --name tom training/webapp python app.py
-i: 交互式操作
-t: 终端
-d：后台运行
-p：绑定地址：端口 默认tcp通信，如需要udp需加/udp
--name：容器命名
training/webapp: 镜像
python app.py：运行容器中程序，也可以只启动容器不运行程序则不用加
/bin/bash：交互式 Shell，/bin/bash。
```

### 查看运行容器


```
docker ps 
```

### 查看所有的容器

```
docker ps -a
```

### 容器重启与停止

```
docker restart/stop <容器 ID>
```

### 进入容器

```
docker attach 容器id
docker exec -it 容器id /bin/bash
```

### 导出和导入容器

```
docker export 容器id >压缩包.tar
docker import http://example.com/exampleimage.tgz或者example/imagerepo
```

### 删除容器

```
docker rm -f 容器id 
```

### Docker 容器互联

#### docker网络

```
docker network create -d bridge/overlay（集群） 网络名
```

#### 容器链接

```
运行一个容器并连接到新建的网络
docker run -itd --name 容器名1 --network 网络名 镜像名 /bin/bash 
打开新的终端，再运行一个容器并加入网络
docker run -itd --name 容器名2 --network 网络名 镜像名 /bin/bash
```

#### **手动指定容器的配置**

```
docker run -it --rm host_ubuntu  --dns=114.114.114.114 --dns-search=test.com ubuntu
```

**-h HOSTNAME 或者 --hostname=HOSTNAME**： 设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts。

**--dns=IP_ADDRESS**： 添加 DNS 服务器到容器的 /etc/resolv.conf 中，让容器用这个服务器来解析所有不在 /etc/hosts 中的主机名。

**--dns-search=DOMAIN**： 设定容器的搜索域，当设定搜索域为 .example.com 时，在搜索一个名为 host 的主机时，DNS 不仅搜索 host，还会搜索 host.example.com。

#### 配置主机DNS

我们可以在宿主机的 /etc/docker/daemon.json 文件中增加以下内容来设置全部容器的 DNS：

```
{
  "dns" : [
    "114.114.114.114",
    "8.8.8.8"
  ]
}
```

## Docker仓库

### 登录退出

```
docker login/logout
```
### 推送镜像
```
docker push username/ubuntu:18.04
```