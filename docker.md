# Docker 概述

官网地址: www.docker.com/

仓库地址:www.hub.docker.com/

## docker的作用

  > 在项目开发的过程中,生产环境,开发环境,配置起来就很麻烦。

  > 公司里面的开发人员,运维人员，常常面临一个问题,在我的电脑上面可以正常运行。然后涉及到版本跟新,node版本，sass版本等等插件不适合,导致项目运行不起来。 甚至部署的电脑多,每一台电脑都要配置，比如(redis,ES,hadoop)等集群配置，费时费力。

  > docker的出现就是为了保证环境的统一性。方便管理部署的多个项目。项目打包的时候带上需要的环境镜像安装。这样在任何电脑上环境都是一致的

  > 例子 传统java开发,打包项目,把jar包丢给运维部署。 现在开发即运维， 打包的时候带上环境配置， 用docker部署。 高级开发人员必须掌握。

## Docker和传统的虚拟机技术对比

传统的VMware技术 如图

![image-20231105121455861](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105121455861.png)

传统的虚拟机技术，是完全模拟一整套的电脑设备，(Kernel 内核， lib运行环境，硬件)。 一个虚拟机基本在几个G.

缺点: 

1 体积大，占用资源多，一台电脑上如果有多个虚拟机，带不动

2.启动慢,冗余步骤多。

3.容灾能力差，多个应用都是共用一个lib环境



docker也是一种虚拟技术 如图

![image-20231105122047207](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105122047207.png)

优点：

1 不需要完整虚拟电脑lib环境，或者硬件设备。应用直接运行在宿主机的Kernel上。

2 体积非常小，秒级启动。 

3 每一个应用都是带着自己需要的运行环境，互相隔离。 容灾能力强。

## docker 的基本组成

docker架构图

![image-20231105123458094](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105123458094.png)

镜像(image)    

 镜像好比一个类模板， 容器则是一个个实例。

 容器(container)

可以启动，停止，删除容器。

容器可以简单理解成一个简易的linux系统

仓库(repository)

可以拉取，或者发布自己的镜像。



## docker安装

1.检查centos 服务的内核，和系统版本

```
uname -r 查看服务器内核
3.10.0-1160.95.1.el7.x86_64

cat /etc/os-release  查看服务器信息

NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/
```

2.安装之前先卸载旧版本

```
1 # yum install -y yum-utils

2  配置docker 源
yum-config-manager --add-repo  https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

3 yum makecache fast 更新包索引

4.yum install docker-ce docker-ce-cli containerd.io -y

5.systemctl start docker 启动docker

6.docker version   启动成功会显示以下信息，包括是什么版本，go语言的版本等等都会有
Client: Docker Engine - Community
 Version:           24.0.7
 API version:       1.43
 Go version:        go1.20.10
 Git commit:        afdd53b
 Built:             Thu Oct 26 09:11:35 2023
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          24.0.7
  API version:      1.43 (minimum version 1.12)
  Go version:       go1.20.10
  Git commit:       311b9ff
  Built:            Thu Oct 26 09:10:36 2023
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.24
  GitCommit:        61f9fd88f79f081d64d6fa3bb1a0dc71ec870523
 runc:
  Version:          1.1.9
  GitCommit:        v1.1.9-0-gccaecfc
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
  
7.测试docker   docker run hello-world
```

![image-20231105143243360](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105143243360.png)

```
1.如果没有这个镜像，先find 去查找这个镜像
2 拉取镜像
3 显示status download 成功
```

**docker images** 可以查看到镜像 hello-world

![image-20231105143523783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105143523783.png)

```
8 卸载docker
 yum remove docker-ce docker-ce-cli containerd.io -y 卸载安装包
 rm -rf /var/lib/docker  删除目录
 

```

```
9.配置镜像加速
找到自己的阿里云服务器 
```

![image-20231105150338080](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105150338080.png)

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://sleo5lac.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## docker run的运行原理图

![image-20231105151048643](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105151048643.png)

## docker 的工作原理

docker 是一个cs架构， client -server

客户端发送指令  服务端通过指令操作镜像和容器

![image-20231105151651413](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105151651413.png)

宿主机和容器是互相隔离的，所以要做端口映射。

# docker常用命令

```
docker version #查看docker版本信息
docker info  #查看docker系统信息，包括镜像和容器的数量
docker --help # 帮助mingl
```

## 镜像命令

**docker images**

```
# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    9c7a54a9a43c   6 months ago   13.3kB
#REPOSITORY 仓库的名称
#TAG  版本号
#IMAGE ID 镜像id
#CREATE 创建时间
#SIZE  包大小

可选项  如 docker images --help 
-a  所以镜像
-f  过滤条件
-p  只显示id
```

**docker search --help**

```
docker serach mysql
NAME                            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                           MySQL is a widely used, open-source relation…   14577     [OK]       
mariadb                         MariaDB Server is a high performing open sou…   5561      [OK]       
percona                         Percona Server is a fork of the MySQL relati…   620       [OK]       
phpmyadmin                      phpMyAdmin - A web interface for MySQL and M…   887       [OK]       
bitnami/mysql                   Bitnami MySQL Docker Image                      101                  [OK]
circleci/mysql                  MySQL is a widely used, open-source relation…   29                   
bitnami/mysqld-exporter                                                         5                    

Name  名称
DESCRIPTION  描述
STARS  星星数量

docker search mysql -f=STARS=4000   筛选查询

NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   14577     [OK]       
mariadb   MariaDB Server is a high performing open sou…   5561      [OK]       

```

docker pull mysql  下载镜像  

```
doker pull 镜像名 [:tag]
docker pull mysql
Using default tag: latest      #默认使用最新版本
latest: Pulling from library/mysql
72a69066d2fe: Pull complete    #分层下载，docker images 的核心  联合文件系统
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709  #签名防伪
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest  #真实地址

docker pull mysql   等价于下面的命令
docker pull .io/library/mysql:latest

指定版本下载 
docker pull mysql:5.7

5.7: Pulling from library/mysql
72a69066d2fe: Already exists  # Already exists之前下载的，可以共用的就不会下载，极大的节省了内存
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
0ceb82207cd7: Pull complete 
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7


```

**docker rmi**  删除镜像

```
docker rmi -f 容器id  删除指定
docker rmi -f 容器id  容器id   删除多个容器，
docker rmi -f $(docker images -aq)  删除所有容器
```

## 容器命令

**说明:有了镜像，才能产生容器，提供服务**

**新建容器并启动**

```
docker run [可选参数] image

#参数说明
--name="name"  容器名称, mysql1 mysql2 用于区分容器
-d  			后台方式运行
-it  			使用交互模式运行
-p  			指定容器的端口 -p 8080:8080
	-p ip:主机ip地址
	-p  主机端口:容器端口 常用     8080：8080
	-p  
-P  			随机指定端口
 
测试 docker run -it centos /bin/bash    6a3c55e8f7fc 容器id
[root@iZ2vc7kou0oyopcn0y14fnZ /]# docker run -it centos /bin/bash
[root@6a3c55e8f7fc /]# 

退出 exit
```



**查看正在运行的容器**

```
docker ps 
不带参数  #显示正在运行的容器
-a       #显示所有容器
-n=1     #显示最近创建的1个容器
-q       #只显示id
```

**退出容器**

```
exit   直接退出
ctrl +p+Q   容器不停止 退出
```

**删除容器**

```
docker rm 容器id  不能删除正在运行的容器
docker rm -f $(docker ps -aq)
```

**启动和停止容器命令**

```
docker start 容器id
docker restart 容器id
docker stop 容器id
docker kill 容器id  强制停止容器
```



##  常用命令

**后台启动容器**

```
docker run -d centos 
问题 docker ps 发现没有正在运行的容器
docker ps -aq  发现容器停止运行了

#常见的坑
docker容器后台运行，就必须要有一个前台应用提供服务，不然docker认为没有应用了，就会停掉容器。
```



**查看容器日志**

```
docker logs -tf --tail 10 容器id   查询指定容器的10条日志
```

**查看容器的源数据**

```
docker inspect 容器id(326eb1b4900f)  可以查看到容器的所有信息，id,创建时间，状态，数据卷等等
[
    {
        "Id": "326eb1b4900f22678df9c3f7f3a1191536f6b44416472ecfaf1cfc627e6db0a3",
        "Created": "2023-11-05T09:01:46.819808531Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 13979,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2023-11-05T09:01:47.031812942Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
```

**查看容器中的进程信息**

```
docker top 326eb1b4900f

UID                 PID                 PPID                C                   STIME               TTY      root                13979               13958               0                   17:01               pts/0   
```

### 进入正在运行的容器

```
docker exec -it 容器id /bin/bash
docker attach 容器id 
#区别   exec 会开启一个新的终端，让开发人员操作  attch则是进入之前正在执行的终端
```

**从容器中拷贝数据到主机**

```
docker cp 326eb1b4900f:/home/text.txt  /home
docker cp 容器id:文件路径   主机存放路径

[root@iZ2vc7kou0oyopcn0y14fnZ home]# docker exec -it 326eb1b4900f /bin/bash  #进入容器
[root@326eb1b4900f /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@326eb1b4900f /]# cd home   											 #进入容器中的home目录
[root@326eb1b4900f home]# touch text.txt									 #创建文件	
[root@326eb1b4900f home]# ls
text.txt
[root@326eb1b4900f home]# exit
exit
[root@iZ2vc7kou0oyopcn0y14fnZ home]# docker cp 326eb1b4900f:/home/text.txt /home  //拷贝到主机home目录下
Successfully copied 1.54kB to /home
[root@iZ2vc7kou0oyopcn0y14fnZ home]# ls
text.txt
[root@iZ2vc7kou0oyopcn0y14fnZ home]# 

```

## nginx

```
docker pull nginx  #下载镜像
docker run -d --name nginx01 -p 3344:80 nginx   启动一个nginx容器
#参数说明
-d 后台运行
--name nginx01 容器名称
-p  3344:80    服务器主机端口3344: 容器内部端口80
```

![image-20231105175424817](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105175424817.png)

```
docker exec -it 8fc5c8dac6cf /bin/bash  # 进入容器
root@8fc5c8dac6cf:/# whereis nginx		#查找nginx文件所在的目录
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx  

/etc/nginx    #nginx配置文件所存放目录
/usr/share/nginx    #前端项目文件所存放目录

```

## tomcat

```
docker pull tomcat  #下载镜像

docker run -d --name tomcat01 -p 3388:8080 tomcat  #启动容器

docker exec -it f4c83e00ec3b /bin/bash		#进入容器

网页地址栏直接访问是可以成功的，但是页面显示有问题是因为，阿里云镜像剔除了不必要的一些东西，导致webApps这个目录下没有任何东西

root@f4c83e00ec3b:/usr/local/tomcat# ls        #检查发现 webapps.dist
BUILDING.txt	 NOTICE		RUNNING.txt  lib	     temp	   work
CONTRIBUTING.md  README.md	bin	     logs	     webapps
LICENSE		 RELEASE-NOTES	conf	     native-jni-lib  webapps.dist

#把webapps.dist目录下面的所有文件复制到webapps目录中， 或者改变文件夹名称，在删除同名的webapps目录
root@f4c83e00ec3b:/usr/local/tomcat# cp -r webapps.dist/* webapps   


```



## commit 命令

比如tomcat容器 我不想每次都手动cp webapps.dist目录下的文件到webapps目录，每次部署都要做一次。

所以我可以把这个tomcat01容器 做成一个新的镜像。

```
docker commit -m="提交描述信息" -a="作者名称"  容器id (f4c83e00ec3b )   镜像名称 (myTomCat:1.0)

[root@iZ2vc7kou0oyopcn0y14fnZ ~]# docker commit -m="我的tomcat" -a="wzp" f4c83e00ec3b mytomcat:1.0  #提交镜像

[root@iZ2vc7kou0oyopcn0y14fnZ ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
mytomcat     1.0       57a72a2cd5e2   8 seconds ago   684MB    #新镜像，下次启tomcat服务就可以用自己的镜像
nginx        latest    605c77e624dd   22 months ago   141MB
tomcat       9.0       b8e65a4d736d   22 months ago   680MB
tomcat       latest    fb5657adc892   22 months ago   680MB
[root@iZ2vc7kou0oyopcn0y14fnZ ~]# 

```



## 容器数据卷

docker的理念是应用和环境都打包成一个镜像。

用户的操作所产生的数据，不可能放在容器内，不然删除了容器，数据也没了。

需求：数据持久化，容器删除了，数据还在。同时容器之间也能共享数据。这就需要一种数据同步，数据贡献的技术。

![image-20231105234402683](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105234402683.png)

把容器中的文件 和 宿主机中的文件进行映射。 

### 使用数据卷

```
方式一： 直接用命令进行挂载 -v
```

```
docker run -it -v 主机目录 : 容器目录   (-p 主机端口:容器端口)

docker run -it -v /home/ceshi:/home centos /bin/bash  

#多个目录映射 用空格隔开
docker run -it -v /home/ceshi:/home -v /home/config:/etc/nginx/config centos /bin/bash  
```

```
docker insepct 8ffcbc0c2974  #查看容器信息
```

![image-20231105235139914](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231105235139914.png)

这时候无论是操作主机home/ceshi 目录， 给目录中增，删，改 还是在容器中，做增，删，改 的操作，都会双向同步

![image-20231106000403310](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106000403310.png)

**删除容器，不会影响到主机中的数据卷目录**

###  匿名挂载和具名挂载

```
#匿名挂载
docker run -d -P -v/etc/nginx nginx  #后台运行一个nginx容器，  -v/etc/nginx  只有容器内的目录地址

docker volume ls  #查看docker中所有的挂载项

DRIVER    VOLUME NAME
local     e97481eeba1f2179f532c0c10d5f60a0256c35ecd6eea06  #匿名挂载的名称就是这串

#具名挂载

docker run -d -P -v juming-guazai:/etc/nginx --name nginx02 nginx 
[root@iZ2vc7kou0oyopcn0y14fnZ ceshi]# docker volume ls
DRIVER    VOLUME NAME
local     e97481eeba1f2179f532c0c10d5f60a0256c35ecd6eea0637345f9b3eb7ed5f7
local     juming-guazai   #具名挂载就是取了一个名字  注意千万不能加 /    /juming-guazai  代表的是一个目录地址


docker inspect e68c3cc42f7c  查看nginx02 上的数据卷关系 
```

![image-20231106004537822](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106004537822.png)

**匿名挂载的名字就是这个随机的字符串**

![image-20231106004819673](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106004819673.png)

```
docker 不管是具名还是匿名挂载， 只要不是指定目录名称，docker的数据卷都是存放在   /var/lib/docker/volumes  中的
```

```
拓展  docker run -d -P -v juming-guazai:/etc/nginx:ro --name nginx02 nginx 
	  docker run -d -P -v juming-guazai:/etc/nginx:rw --name nginx02 nginx 	
	 
	#ro 指readOnly 只读   rw 可读可写  默认就是rw
	
	一旦设置成了ro，就代表这个目录在容器内只读权限，只能通过外部宿主机对应的目录进行修改
```

### 数据容器卷

1.容器之间的数据同步

![image-20231107125331708](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231107125331708.png)

备注:

**数据容器卷是拷贝性质，删除父容器，子容器中的数据还在。**不是数据共享，删除了之后其他容器也没了数据

```
# 命令 --volumes-from  共享的前提是必须有一个数据卷

1 创建一个centos01 镜像， 匿名挂载 /home/data的目录
docker run -it --name centos01 -v /home/data centos 

2.centos2 共享centos1的数据
docker run -it --name centos2 --volumes-from centos01 centos
[root@a932f384694d /]# cd home/
[root@a932f384694d home]# ls
data
[root@a932f384694d home]# cd data
[root@a932f384694d data]# touch 1.txt   #在centos2 /home/data 中创建1.txt
[root@a932f384694d data]# ls
1.txt
[root@a932f384694d data]# 

3 查看centos01 /home/data 下是否具有1.txt
[root@iZ2vc7kou0oyopcn0y14fnZ data]# docker attach  centos01  #进入容器
[root@d08732c9f9a7 home]# ls
data
[root@d08732c9f9a7 home]# cd data/
[root@d08732c9f9a7 data]# ls
1.txt														 #1.txt存在， 数据容器生效						#
[root@d08732c9f9a7 data]# 


```



## 初识dockerFile

DockerFile就是通过一段脚本代码，生成镜像。

##  Docker 网络 重点

问题: 

我的项目部署在一个容器内，		 3345 ：3000   主机和容器的端口映射

我的mongoDB部署在另一个容器内	 27017：27017   主机和容器的端口映射

后台项目配置的数据库连接地址  mongodb://47.108.24.121:27017/manager   连接不上

3345端口怎么访问27017端口中的数据库

![image-20231106224742564](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106224742564.png)



```
查看容器地址 docker exec -it 容器id ip addr
#unable to start container process: exec: "ip"  报错原因为该容器的镜像时精简版，内部缺少iproute2导致无法使用ip命令
解决方案1
进入容器
docker exec -it 容器名 /bin/bash

进入后：

更新apt

apt-get update

安装iproute2

agt install -y iproute2

解决方案2  通过这个命令查看 省事  docker inspect tomcat05 | grep IPAddress
            "SecondaryIPAddresses": null,
            "IPAddress": "172.17.0.8",
            "IPAddress": "172.17.0.8",
	

安装完以后退出exit
docker exec -it tomcat05 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
80: eth0@if81: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:08 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.8/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

# eth0@if81  这种地址是dockers分配的地址

ping 172.17.0.8

PING 172.17.0.8 (172.17.0.8) 56(84) bytes of data.
64 bytes from 172.17.0.8: icmp_seq=1 ttl=64 time=0.068 ms
64 bytes from 172.17.0.8: icmp_seq=2 ttl=64 time=0.048 ms
64 bytes from 172.17.0.8: icmp_seq=3 ttl=64 time=0.041 ms
64 bytes from 172.17.0.8: icmp_seq=4 ttl=64 time=0.062 ms
64 bytes from 172.17.0.8: icmp_seq=5 ttl=64 time=0.042 ms

#主机是可以ping 通容器的
```



```
原理:每启动一个容器，docker就会给这个容器分配一个地址，同时自己也需要一个东西来标识这个地址。不然怎么区分，只要安装了docker,
就会有一个docker0的网卡，桥接模式， evth-pair技术。

以下通过容器查看网址，主机ip addr 发现，网址是11对应， 成对出现的

```

![image-20231106230252094](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106230252094.png)

![image-20231106230332130](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106230332130.png)

## 前端项目部署

以音乐app项目为例子

```
1. 在centos 服务器  
home目录下创建对应项目的nginx 目录结构  
mkdir -p /home/music_app/nginx/html
mkdir -p /home/music_app/nginx/conf
mkdir -p /home/music_app/nginx/logs

2. 随便启动一个nginx 容器 就是为了拷贝出容器中的默认配置文件到宿主机系统目录里
docker cp 容器ID：/etc/nginx/nginx.conf /home/docker/nginx/conf

3.数据卷的对应关系
宿主机                                              容器
/home/music_app/nginx/html                           /usr/share/nginx/html #网页文件
/home/music_app/nginx/conf/nginx.conf               /etc/nginx/nginx.conf#配置文件
/home/music_app/nginx/logs                          /var/log/nginx#日志文件

#执行以下命令 就可以启动一个nginx容器
docker run -d -p 3344:80 --name music-web-app  -v /home/music_app/nginx/html:/usr/share/nginx/html -v /home/music_app/nginx/conf/nginx.conf:/etc/nginx/nginx.conf  -v /home/music_app/nginx/logs:/var/log/nginx nginx

4. 把前端打包好的dist目录下的所有文件通过xftp 传到 /home/music_app/nginx/html 这个目录中

```

![image-20231106152309898](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106152309898.png)

## Dockersfile 构建前端项目镜像

1.创建Dockerfile文件  和default.conf 文件

```
Dockerfile

FROM nginx
 
RUN rm /etc/nginx/conf.d/default.conf
 
ADD default.conf /etc/nginx/conf.d/
 
COPY dist/ /usr/share/nginx/html/
```



```
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

   
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

}
```

![image-20231107020715586](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231107020715586.png)

2.把dist目录和 上面2个文件都上传到   /home/保存应用的目录

![image-20231107020554950](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231107020554950.png)

3. ```
   docker build -t 镜像名称 .        # . 不能忘记
   ```

   ```
   docker run -d -p 3346:80 镜像名称(或者id)   #启动容器服务
   ```

   

## docker部署mongoDB

```
1.docker pull mongo 安装镜像

2.docker run -it -d -p 27017:27017 -v /home/mongo/data:/data/db --name managermongo mongo 启动容器，数据卷映射

3.阿里安全组开放27017端口  本地cmd  mongo 47.108.24.121:27017  # 测试能否远程连接

4.把本地电脑中mongoDB数据同步到docker 服务器上的mongoDB数据库中

```

（1.） 先到导出数据脚本js文件，其他不选，有坑

![image-20231106185433728](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106185433728.png)

（2）. 连接上远程数据库，导入脚本文件

![image-20231106185725001](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106185725001.png)

（3.） docker monfoDB容器中查看数据 一切正常

![image-20231106185854393](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106185854393.png)

项目运行，查询数据正常

![image-20231106190037573](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231106190037573.png)

## docker部署node后台项目

1.在服务器 home目录下创建manager_node目录

```
[root@iZ2vc7kou0oyopcn0y14fnZ ~]# cd /home
[root@iZ2vc7kou0oyopcn0y14fnZ home]# ls
app_node  manager_node  mongo  music_app
```

2.创建Dockerfile

```
FROM node        #需要的低沉环境
COPY . /root/manager_node   #把当前目录下的所有文件赋值到容器 /root/manager_node 这个目录中
WORKDIR  /root/manager_node  #启动容器自动进入这个工作目录
RUN echo "registry=https://registry.yarnpkg.com" > /root/.yarnrc  #更爱yarn的下载源
RUN yarn install	 #下载依赖
EXPOSE 3000 		 #申明暴露了那些接口
ENTRYPOINT [ "node","app.js" ]    #默认启动执行node app.js
```

3.修改数据库连接地址

```
let config={
  dbUrl:"mongodb://47.108.24.121:27017/manager"    #这里改成自己的服务ip地址
}
module.exports=config;
```

4. 把除去node_modules 之外的所有文件通过xftp 上传到  /home/manager_node

5. ```
   docker build -t manager_node .   创建manager_node镜像
   ```

6. ```
    docker run -d -p 3345:3000 manager_node   #运行容器服务
   ```

7. 项目启动成功，数据正常

   ![image-20231107010208920](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231107010208920.png)

