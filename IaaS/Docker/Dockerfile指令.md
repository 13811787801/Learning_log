## COPY

已在 tomcat 中部署 Jenkins 为例

 ```dockerfile
FROM tomcat


WORKDIR /usr/local/tomcat/webapps # 指定上下文路径，非系统路径
COPY jenkins.war ./ # jenkins在当前路径
 ```

![](https://user-gold-cdn.xitu.io/2019/9/29/16d7b1618a28509f?w=664&h=70&f=png&s=9928)

`build`构建成功后 很明显第一个镜像比tomcat的大，就是多了jenkins文件，下面运行

![](https://user-gold-cdn.xitu.io/2019/9/29/16d7b17bae045d37?w=787&h=62&f=png&s=7662)

访问我们的Jenkins

![](https://user-gold-cdn.xitu.io/2019/9/29/16d7b1853cd708e6?w=1019&h=594&f=png&s=73447)

这样我们就定制好了 通过Tomcat镜像运行Jenkins的镜像



## ADD 命令

```dockerfile
...
FROM scratch
ADD ubuntu-xenial-core-cloudimg-amd64-root.tar.gz /
...
```

ADD` 指令和 `COPY` 的格式和性质基本一致。但是在 `COPY` 基础上增加了一些功能。

比如 `<源路径>` 可以是一个 `URL`，这种情况下，Docker 引擎会试图去下载这个链接的文件放到 `<目标路径>` 去。下载后的文件权限自动设置为 `600`，如果这并不是想要的权限，那么还需要增加额外的一层 `RUN` 进行权限调整，另外，如果下载的是个压缩包，需要解压缩，也一样还需要额外的一层 `RUN` 指令进行解压缩。所以不如直接使用 `RUN` 指令，然后使用 `wget` 或者 `curl` 工具下载，处理权限、解压缩、然后清理无用文件更合理。因此，这个功能其实并不实用，而且不推荐使用



如果 `<源路径>` 为一个 `tar` 压缩文件的话，压缩格式为 `gzip`, `bzip2` 以及 `xz` 的情况下，`ADD` 指令将会**自动解压缩**这个压缩文件到 `<目标路径>` 去

`Dockerfile 最佳实践文档` 中要求，尽可能的使用 `COPY`，因为 `COPY` 的语义很明确，就是复制文件而已，而 `ADD` 则包含了更复杂的功能，其行为也不一定很清晰。最适合使用 `ADD` 的场合，就是所提及的需要自动解压缩的场合。