# 跟着小D学Dokcer
## 一、	Docker简介
### 1、	Docker与传统虚拟化（虚拟机）不同
Docker 和传统虚拟化方式的不同之处。传统虚拟机技术是虚拟
出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。
主流的Linux操作系统已经支持Docker。Redhat RHEL6.5、CentOS6.5以上的操作系统、Ubuntu14.04操作系统，都已经默认带有Docker软件包。
Docker的主要目标：Build，Ship And Run Any App，Anywhere。即通过对应组件封装（Packaging）、分发（Distribution）、部署（Deployment）、运行（Runtime）等生命周期管理，达到应用组件级别的：“一次封装，到处运行”。为应用的开发和部署提供了“一站式”的解决方案。
Git：https://github.com/docker/docker
Docker官网文档URL： https://docs.docker.com/engine/
### 2、	Docker核心概念
2.1、	Docker镜像
镜像是Docker容器创建的基础。Docker提供一套十分简单的机制来创建和更新现有的镜像，用户也可以从网上下载一个已经做好的应用镜像，并通过命令来运行。
2.2、	Docker容器
Docker容器Container类似于一个轻量级的沙箱，Docker利用容器来运行和隔离应用。
容器是从镜像创建的应用运行实例，可以将其启动、开始、停止、删除，而这些容器都是相互隔离的、互不可见的。
2.3、	Docker仓库
Docker仓库指的是：Docker集中存放镜像文件的地方。
3、	Docker知识
docker在1.13版本以后分了Docker社区版(ce)和Docker企业版(ee)两个版本，ee需要购买。较老版本的docker叫docker或者docker-engine
## 二、	Docker安装、卸载、配置
### 1、	Centos安装Docker
安装Docker的方法有两种：
①	设置Docker的存储库并从中安装（推荐） 。
②	某些用户通过下载RPM软件包并手动安装，并手动完成管理升级。
#### 1.1、	通过存储库安装
参考官网的docker-ce存储库设置：
安装所需的软件包 yum-utils提供了yum-config-manager 效用，并device-mapper-persistent-data和lvm2由需要 devicemapper存储驱动程序。
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2```
使用以下命令设置稳定版本库。您始终需要稳定的存储库，即使您也想安装边缘版本。
```
$ sudo yum-config-manager \
	--add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
可选：启用边缘存储库。该存储库包含在上述docker.repo文件中，但默认情况下禁用。您可以在稳定的存储库旁边启用它。
```
$ sudo yum-config-manager --enable docker-ce-edge```
您可以通过运行 具有该标志的命令来禁用边缘存储库。要重新启用它，请使用该 标志。以下命令禁用边缘存储库。
```
yum-config-manager--disable—enable```
Docker安装：
更新yum包索引
```
sudo yum makecache fast```
安装最新版本的Docker
```
sudo yum install docker-ce```
通过下面命令安装的是docker1.12版本
```
sudo yum install docker```
查询存储库中的docker版本
```
$ yum list docker-ce.x86_64  --showduplicates |sort -r```
安装指定版本docker
`sudo yum install docker-ce-<Version>`
编辑/etc/docker/daemon.json。如果还不存在，创建它。假设文件为空，请添加以下内容。
```
{
	"storage-driver": "devicemapper"
}```
启动Docker。
```
$ sudo systemctl start docker```
docker通过运行hello-world 映像验证是否正确安装
```
$ sudo docker run hello-world```


### 1.2、	通过RPM软件包安装
下载docker-ce 或者ee软件包：
Docker-ce URL：
https://download.docker.com/linux/centos/7/x86_64/stable/Packages/ 并下载.rpm要安装的Docker版本的文件。
安装Docker
```
$ sudo yum install /path/to/package.rpm
```
编辑/etc/docker/daemon.json。如果还不存在，创建它。假设文件为空，请添加以下内容。
```
{
	"storage-driver": "devicemapper"
}
```
启动Docker。
```
$ sudo systemctl start docker
```
docker通过运行hello-world 映像验证是否正确安装
```
$ sudo docker run hello-world
```

【注】对于centos6安装Docker，参考：
http://blog.csdn.net/tuzhutuzhu/article/details/48727597

### 1.3、	卸载Docker
1.3.1卸载旧版本的Docker
```
$ sudo yum remove docker \    
		 docker-common \
			container-selinux \
                  docker-selinux \
			docker-engine \docker-engine-selinux
```
1.3.2卸载docker-ce
```
$ sudo yum remove docker-ce
```
```
$ sudo rm -rf /var/lib/docker
```
### 1.4、	Docker加速器配置
#### 1.4.1配置使用加速器
```
$sudo mkdir -p /etc/docker
```
```
$sudo tee /etc/docker/daemon.json <<-'EOF'
```
```
{
  "registry-mirrors": ["https://n9ekbvjb.mirror.aliyuncs.com"]
}
EOF
```
```
$sudo systemctl daemon-reload
```
```
$sudo systemctl restart docker
```
参考资料：
http://blog.csdn.net/u010716706/article/details/60962061
#### 1.4.2启动参数设置加速器
```
echo "DOCKER_OPTS=\"--registry-mirror= https://n9ekbvjb.mirror.aliyuncs.com\"" | sudo tee -a /etc/default/docker sudo service docker restart
```

## 三、	Docker简单使用
### 1、	启动docker
```
systemctl start docker
```
### 2、	查看docker版本
```
Docker --version
```
### 3、	镜像
#### 3.1、镜像配置
#### 3.2、获取镜像
```
docker pull [选项] [Docker Registry地址]<仓库名>:<标签>
```
Docker Registry地址：地址的格式一般是  <域名/IP>[:端口号]  。默认地址是Docker Hub
仓库名：如之前所说，这里的仓库名是两段式名称，既  <用户名>/<软件名>  。对于 Docker Hub，如果不给出用户名，则默认为  library  ，也就是官方镜像
命令：
`docker images`//查看本地镜像
`docker search mysql`//搜索远程仓储包含mysql的镜像，默认仓储式Docker Hub
`docker tag `原镜像名 新命名标签（快捷方式而已）
`docker inspect dockerid `//查看docker详细信息
`docker inspect -f `{{.属性名}}//查看docker的指定信息
`docker rmi `镜像名称、镜像id//删除docker镜像	
注：当有docker镜像创建的容器存在时，docker镜像不能被删除。
`docker save -o `文件.tar docker镜像名
`docker load` < 文件.tar
`docker load –input` 文件.tar
#### 4、	容器
容器的创建
`docker create -it dockername`//使用docker create创建的容器处于停止状态，启动容器需用docker start命令来启动它
查看容器
`docker ps -a`//显示所有容器，不加-a默认情况只是显示运行容器
`docker run dockername`//等价于docker create + docker start
Docker run讲解：
当利用docker run 来创建并启动容器时，docker在后台运行的标准操作包括：
（1）	检查本地是否存在指定的镜像，不存在就从公有仓库下载
（2）	利用镜像创建并启动一个容器
（3）	分配一个文件系统，并在只读的镜像曾外面挂载一层可读写层
（4）	从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
（5）	从地址池配置了一个ip地址给容器
（6）	执行用户指定的应用程序
（7）	执行完毕后容器被终止
启动一个bash终端，允许用户进行交互：
```
Docker run -t -i ubuntu:14.04 /bin/bash```
其中，-t选项让docker分配一个伪终端并绑定到容器的标准输入上，-t则是让容器的标准输入保持打开。
在交互模式下，用户可以通过所创建的终端来输入命令
退出伪终端可以使用ctrl+d或者输入exit来退出,对于所创建的bash容器，当使用exit命令退出之后，该容器就自动处于终止状态。
守护状态运行容器：
Docker容器更多的是需要以守护状态形式运行，用户可以通过添加-d 参数来实现。
```
```
docker run -d ubuntu /bin/sh -c “while true; do echo hello word; sleep 1; done”
```
容器启动之后会返回一个唯一的id。
获取容器的输出信息，可以通过docker logs命令
```
docker logs  containerId
```
终止容器
```
docker stop containerId
```
查看所有处于终止状态的容器
```
docker ps -a -q
```
启动处于终止状态的容器/重启容器
```
docker start containerId
```
```
docker restart containerId
```
进入容器
（1）	Attach命令
```
docker attach containerId /containerName
```
多个窗口同事attach到同一个容器的时候，所有的窗口都会同步显示，当某个窗口因命令阻塞时，其他窗口也无法执行操作了
（2）	Exec命令
Docker从1.3版本后，提供exec命令，可以直接在容器内运行命令
使用ctrl+D，或者使用exit（会停止容器）
使用Ctrl+P+Q退出不会停止容器
```
docker exec -ti containerId /bin/bash
```
（3）	Nsenter工具
删除容器
`docker rm containerId`命令删除处于终止状态的容器
-f 强行终止并删除一个运行中的容器
-l 删除容器的连接，保留容器
-v 删除容器挂载的
导入/导出容器
`docker export containerId `>文件名.tar
导出的文件可以使用docker import命令导入，成为镜像
`cat test.tar | sudo docker import – test/ubuntu:v1.0`
既可以使用docker load命令导入镜像存储文件到本地的镜像库，又可以使用docker import命令导入一个容器快照到本地镜像库。两者的区别在于容器快照文件将丢弃所有的历史记录和元数据信息（只是保存容器当时的快照状态），而镜像存储文件将保存完整记录状态，体积也比较大。此外，从容器快照文件导入时可以重新制定标签等元数据信息。
#### 5、	仓库
仓库是集中存放镜像的地方，Docker官方维护的公共仓库docker hub
基本操作：
用户可以通过docker search命令来查找官方仓库的镜像
可以通过docker pull命令将仓库镜像下载到本地
在docker hub仓库搜索的镜像有两类：（1）基础/根镜像，这些镜像是由docker公司创建、验证、支持、提供，这些镜像的名称往往使用单个单词作为名称。（2）个人镜像，例如tianon/centos镜像，这类镜像是由docker hub用户tianon创建并维护的
用户也可以登录之后通过docker push命令将自己的镜像推送到docker hub上。
自动创建
自动创建功能是用户通过docker hub指定跟踪一个目标网站（例如github）上的项目，一旦项目发现新的提交，则会自动执行创建。
配置自动创建：
（1）	创建并登陆docker hub，以及目标网站，*在目标网站中连接账户到docker hub
（2）	在docker hub中配置一个自动创建
（3）	选取一个目标网站中的项目（需要含dockerfile）和分支
（4）	指定dockerfile的位置，并提交创建
#### 6、	数据卷
数据卷是一个可供容器使用的特殊目录，它绕过文件系统，可以提供很多有用的特性：
数据卷可以在容器之间共享和重用。
对数据卷的修改会立马生效
对数据卷的更新，不会影响镜像
卷会一直存在，直到没有容器使用
在容器内创建数据卷
将数据卷改在到容器的webapp目录下
```
Docker run -d -P –name web -v /webapp training/webapp pathon app.py
```
挂载一个主机目录作为数据卷
`sudo docker run -d -P –name web -v /src/webaapp:/opt/webapp training/webapp python app.py `//加载主机的/src/webapp目录到容器的/opt/webapp目录，注：本地目录的路径必须是绝对路径，如果不存在，docker会自动创建。
docker挂载数据卷的默认全是是rw读写权限，用户也可以自己指定权限or是只读权限
docker run -d -P --name web -v /src/webapp:/opt/webapp:ro training/webapp python app.gy
-d 后台
-P 允许外部访问容器所需要暴露的端口
格式：`ip：hostPort：containerPort | ip :: containerPort | hostPort：containerPort | containerPort`也可以将hostPort和containerPort指定为一系列端口，例如docker run -p 1234-1236：1222-1224
-- name 指定启动后容器的名称
#### 7、	数据卷容器
数据卷容器
```
docker run -it –name dbdata -v /dbdata centos```
在其他容器中使用—volumes-from来挂载dbdata容器中的数据卷
```
docker run -it –name dbdata01 –volumes-from dbdata centos```
```
docker run -it –name dbdata02 -volumes-from dbdata centos
```
利用数据卷容器进行数据迁移
```
docker run –volumes-from /dbdata -v /root/backdata:/backup –name work ubuntu tar cvf /backup/dbdata.tar /dbdata
```
运行创建数据卷容器，将宿主数据与新创建的容器数据目录绑定，解压客户容器到数据卷容器中
#### 8、	网络基础配置
当使用 -P 标记时，Docker 会随机映射一个  49000~49900  的端口到内部容器开放的网络端口
```
$ sudo docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py
```
```
$ sudo docker run -d -p 127.0.0.1::5000 training/webapp python app.py
```
//绑定 localhost 的任意端口到容器的 5000 端口，本地主机会自动分配一个端口。

## 四、	Dockerfile
### 1、	Dockerfile简介
Dockerfile由一行行命令语句组成，并且支持以#开头的注释行。
Dockerfile一般来说分成四个部分：
1. 基础镜像信息
2. 维护者信息
3. 镜像操作指令
4. 操作系统数据
5. 对外开放的接口
6. 容器启动时执行命令

例子：
```
#第一行必须制定基于的基础镜像
FROM ubuntu
#维护者信息
MAINTAINER docker_user docker_user@email.com
#镜像的操作指令
RUN echo “deb http://archive.ubuntu.com/ubuntu raring main universe”>> /etc/apt/sources.list
RUN
#容器启动时执行的命令
CMD /usr/sbin/nginx
```
### 2、	Dockerfile命令
#### （1）	FROM
指定基础的image，构建指令，必须指定且需要在Dockerfile其他指令的前面。后续的指令都依赖于该指令指定的image。FROM指令指定的基础image可以是官方远程仓库中的，也可以位于本地仓库。
```
FROM <image>
FROM <image>:<tag>
```
#### （2）	MAINTAINNER
用来指定镜像创建者信息, 构建指令，用于将image的制作者相关的信息写入到image中。当我们对该image执行docker inspect命令时，输出中有相应的字段记录该信息。
```
MAINTAINER <name>
```
#### （3）	RUN
安装软件用的，构建指令，RUN可以运行任何被基础image支持的命令。如基础image选择了ubuntu，那么软件管理部分只能使用ubuntu的命令。
```
RUN yum install -y applydeltarpm pcre-devel wget net-tools gcc gcc-c++ zlib zlib-devel make openssl-devel
```
#### （4）	CMD
设置container启动时执行的操作，设置指令，用于container启动时指定的操作。该操作可以是执行自定义脚本，也可以是执行系统命令。该指令只能在文件中存在一次，如果有多个，则只执行最后一条。
```
CMD /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
CMD ["/usr/java/tomcat/bin/catalina.sh", "run"]
CMD "echo" "Hello docker!"
CMD /bin/bash
```
#### （5）	ENTRYPOINT
设置container启动时执行的命令，该指令的使用分为两种情况，一种是独自使用，另一种和CMD指令配合使用。
当独自使用时，如果你还使用了CMD命令且CMD是一个完整的可执行的命令，那么CMD指令和ENTRYPOINT会互相覆盖只有最后一个CMD或者ENTRYPOINT有效。
```
ENTRYPOINT /usr/java/tomcat/bin/startup.sh
```
另一种用法和CMD指令配合使用来指定ENTRYPOINT的默认参数，这时CMD指令不是一个完整的可执行命令，仅仅是参数部分；ENTRYPOINT指令只能使用JSON方式指定执行命令，而不能指定参数。
#### （6）	USER
设置container容器的用户，设置指令，设置启动容器的用户，默认是root用户。
USER [UID] 设置运行容器的UID
#### （7）	EXPOSE
指定容器需要映射到宿主机的端口
设置指令，该指令会将容器中的端口映射成宿主机器中的某个端口。当你需要访问容器的时候，可以不是用容器的IP地址而是使用宿主机器的IP地址和映射后的端口。要完成整个操作需要两个步骤，首先在Dockerfile使用EXPOSE设置需要映射的容器端口，然后在运行容器的时候指定-p选项加上EXPOSE设置的端口，这样EXPOSE设置的端口号会被随机映射成宿主机器中的一个端口号。也可以指定需要映射到宿主机器的那个端口，这时要确保宿主机器上的端口号没有被使用。EXPOSE指令可以一次设置多个端口号，相应的运行容器的时候，可以配套的多次使用-p选项。
```
EXPOSE: EXPOSE <port> <port>
```
```
# 映射一个端口
EXPOSE port1
# 相应的运行容器使用的命令
docker run -p port1 image
# 映射多个端口
EXPOSE port1 port2 port3
# 相应的运行容器使用的命令
docker run -p port1 -p port2 -p port3 image
# 还可以指定需要映射到宿主机器上的某个端口号
docker run -p host_port1:port1 -p host_port2:port2 -p host_port3:port3 image
```
#### （8）	ENV
用户环境变量设置，构建指令，在image中设置一个环境变量。
```
ENV <key> <value>
```
设置了后，后续的RUN命令都可以使用，container启动后，可以通过docker inspect查看这个环境变量，也可以通过在`docker run --env key=value`时设置或修改环境变量。
假如你安装了JAVA程序，需要设置JAVA_HOME，那么可以在Dockerfile中这样写：
```
ENV JAVA_HOME /path/to/java/dirent
```
#### （9）	ADD
从src复制文件到container的dest路径，构建指令，所有拷贝到container中的文件和文件夹权限为0755，uid和gid为0；如果是一个目录，那么会将该目录下的所有文件添加到container中，不包括目录；如果文件是可识别的压缩格式，则docker会帮忙解压缩（注意压缩格式）；如果<src>是文件且<dest>中不使用斜杠结束，则会将<dest>视为文件，<src>的内容会写入<dest>；如果<src>是文件且<dest>中使用斜杠结束，则会<src>文件拷贝到<dest>目录下。
```
ADD <src> <dest>
```
<src> 是相对被构建的源目录的相对路径，可以是文件或目录的路径，也可以是一个远程的文件url;
<dest> 是container中的绝对路径
```
ADD http://nginx.org/download/nginx-1.8.0.tar.gz
```

#### （10）	COPY
复制，和ADD使用差不多一致
#### （11）	VOLUMN
指定挂载点，设置指令，使容器中的一个目录具有持久化存储数据的功能，该目录可以被容器本身使用，也可以共享给其他容器使用。我们知道容器使用的是AUFS，这种文件系统不能持久化数据，当容器关闭后，所有的更改都会丢失。当容器中的应用有持久化数据的需求时可以在Dockerfile中使用该指令。
```
VOLUME [“/data”]
```
#### （12）	WORKDIR
切换目录，设置指令，可以多次切换(相当于cd命令)，对RUN,CMD,ENTRYPOINT生效。
```
WORKDIR: WORKDIR /path/to/workdir
```
#### （13）	ONBUILD
在子镜像中执行，ONBUILD 指定的命令在构建镜像时并不执行，而是在它的子镜像中执行。
### 3、	参考资料
http://blog.csdn.net/qinyushuang/article/details/43342553




