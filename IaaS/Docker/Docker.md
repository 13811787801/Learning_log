管理 Docker

```bash
启动        systemctl start docker
守护进程重启   sudo systemctl daemon-reload
重启docker服务   systemctl restart  docker
重启docker服务  sudo service docker restart
关闭docker   service docker stop   
关闭docker  systemctl stop docker
```



### 静态镜像加速

在`/etc/docker/daemon.json`中配置（没有就创建该文件）

```json
 {
 	"registry-mirrors": ["https://registry.docker-cn.com"]
 }
```

重启服务

```bash
systemctl restart docker 
```



### Dockerfile定制镜像

dockerfile是脚本文件，给docker运行的

docker exec 交互的方式**进入**容器

run:交互的方式**启动**容器

> 下面以Tomcat为例、
>
> docker run -it -p 8080:8080 tomcat  // 启动
>
> docker exec -it jakfjklsalj(container) bash 进入容器
>
> cd webapps/ROOT
>
> echo "Hello Docker Tomcat" >> index.jsp
>
> exit //退出镜像

![](https://user-gold-cdn.xitu.io/2019/9/26/16d6cf26ba2d4359?w=1388&h=832&f=png&s=173083)

这样就简单定制了我们的docker容器

### 构建镜像

`tomcat` 镜像为例，这次我们使用 Dockerfile 来定制

在 `usr/local/`下创建mytomcat，创建文件 **Dockerfile**

```bash
FROM tomcat
RUN echo "hello world" > /usr/local/tomcat/webapps/ROOT/index.jsp
```

+ FROM 指定基础镜像
  所谓定制镜像，那一定是**以一个镜像为基础**

  `FROM` 就是指定**基础镜像**，因此一个 `Dockerfile`中 `FROM` 是必备的指令，并且必须是第一条指令

  除了选择现有镜像为基础镜像外，Docker 还存在一个特殊的镜像，名为 `scratch`。这个镜像是虚拟的概念，并不实际存在，它表示一个空白的镜像

+ RUN 

  `RUN` 指令是用来执行 Shell 命令行命令的。由于命令行的强大能力，`RUN` 指令在定制镜像时是最常用的指令之一，其格式有两种：

  *shell* 格式：

  ​	`RUN <命令>`，就像直接在命令行中输入的命令一样。上面的 Dockerfile 中的 `RUN` 指令就是这种格式。

Dockerfile **每一行**代码代表一层，下面我们在Dockerfile所在文件夹下执行`build`命令(后面那个点很重要，声明构建镜像上下文)，然后通过`images`命令查看创建的镜像情况

```bash
docker build -t tomcat8085 .
docker images
```

![](https://user-gold-cdn.xitu.io/2019/9/27/16d7186acc6fa6d6?w=616&h=81&f=png&s=11172)

可以看到我们的 *tomcat8085* 镜像,下面再修改`Dockerfile`

```bash
FROM tomcat

WORKDIR /usr/local/tomcat/webapps/ROOT // 制定工作目录，否则通过cd命令会出问题
RUN rm -rf * // 删除
RUN echo "hello world" > /usr/local/tomcat/webapps/ROOT/index.html  //创建index.html文件
```

构建镜像，并运行后

![](https://user-gold-cdn.xitu.io/2019/9/27/16d71990898a24f2?w=470&h=159&f=png&s=12356)

#### 镜像构建上下文

我们通过docker命令行，似乎是在本地执行这些操作，其实**一切都是使用远程调用在服务端 (Docker 引擎)完成**，所以`build`命令中最后一个 **.**  很重要，那是指定上下文目录