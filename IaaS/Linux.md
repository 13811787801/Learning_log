Linux  内核 具备 图灵完美标准，下面的是操作系统

​	Ubutu ， CentOS

CentOS 有强大公司作为背书 稳定

Ubuntu 社区产物，任何人和机构都可以自由使用Ubuntu底层源码

Windows操作的内核是 `Windows NT`



## Docker

> Docker EE  企业版 付费
>
> Docker CE 社区版 免费

要使用微服务，分布式必须掌握 Docker

Java 一次编译，到处运行 是基于JVM

真正实现一次编译，到处运行

CPU i7 3.0GHz

内存 16G

如果开两个虚拟机，独立开辟内存空间，将宿主机内存切分

VMware  Memory:2G 占用宿主机2G内存 此时宿主机14G Memory

VMware  Memory:2G 占用宿主机 2G内存 宿主机只有 12G Memory 如果只用1G内存，那么另外1G内存就会被浪费掉

Docker 

+ APP1-redis 2G
+ App2-influxdb 1g 

此时内存是共享的，不存在因开启应用而浪费资源 **高效利用系统资源**

**更快速的启动时间**

​	docker应用直接运行在宿主内核，无需启动完整的操作系统，可以做到s级，ms级启动时间

**持续交付和部署**

**更轻松的迁移**

支持 linux，window，mac，支持物理机，虚拟机(虚拟机上是不能运行虚拟机的)

> VMware -> windows --> x VMware 不能的
>
> Docker -> windows -> dockek 可以的 

**更轻松的维护和扩展**

Docker使用**分层存储以及镜像的技术**，使得应用重复部分复用更为容易

高质量官方镜像，可以在生产环境中直接使用

### Docker 引擎

C/S 结构的应用程序，包含以下组件

+ 一种服务器，以守护进程长时间运行的程序
+ REST API用于指定程序与守护进程通信的接口
+ 有命令行界面(CLI)工具的客户端

​	

## Docker 架构

客户端-服务器（C/S）架构模式，使用远程API管理本地和创建Docker容器

通过镜像(image)创建容器(container)

> 通过面向对象思想来解释镜像和容器的关系
>
> 镜像就是类 ，容器是对象
>
> public class image{	
>
> ​	private  Long id;
>
> }
>
> image container = new image()；   

#### 主机

运行Docker引擎的主机(虚拟机，物理机都可以作为主机）

#### 容器

容器是**一个独立或一组**应用

运行多个，实现负载均衡，docker也被称为负载均衡入口

按照Docker的最佳实践，容器存储要保持无状态化，所有文件的写入操作，都该使用**数据卷**

> VMware重复写，性能弱于宿主机；
>
> 数据卷可以实现文件和主机共享，性能更接近宿主机



#### 仓库

Docker 通过仓库保存镜像 ，类似maven中的中央仓库

+ 公有仓库

+ 私有仓库 镜像管理平台

  类似Maven的Nexus

#### 分层存储

使用Union FS(Union File System)技术，镜像并不是iso那样的打包文件

构建镜像时，一层层构建，前一层是后一层的基础，每一层构建完后不会再发生改变

> Docker -> builder Linux-> builder Java

