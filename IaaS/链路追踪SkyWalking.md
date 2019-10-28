

链路追踪是为了解决应用内部系统调用； 帮助理解系统行为、用于分析性能问题的工具，以便发生故障的时候，能够快速定位和解决问题，这就是所谓的 APM（应用性能管理）

> 提供分布式追踪、服务网格遥测分析、度量聚合和可视化一体化解决方案。

- **Skywalking Agent：** 使用 JavaAgent 做字节码植入，无侵入式的收集，并通过 HTTP 或者 gRPC 方式发送数据到 SkyWalking Collector。
- **SkyWalking Collector：** 链路数据收集器，对 agent 传过来的数据进行整合分析处理并落入相关的数据存储中。
- **Storage：** SkyWalking 的存储，时间更迭，SW 已经开发迭代到了 6.x 版本，在 6.x 版本中支持以 **ElasticSearch**(支持 6.x)、Mysql、TiDB、H2、作为存储介质进行数据存储(最好用检索数据库)
- **UI：** Web 可视化平台，用来展示落地的数据。

## SkyWalking 功能特性

- 多种监控手段，语言探针和服务网格(Service Mesh)
- 多语言自动探针，Java，.NET Core 和 Node.JS
- 轻量高效，不需要大数据
- 模块化，UI、存储、集群管理多种机制可选
- 支持告警
- 优秀的可视化方案

### Docker安装 ES

`docker-compose.yml`内容

```yaml
version: '3.3'
services:
  elasticsearch:
    image: wutang/elasticsearch-shanghai-zone:6.3.2
    container_name: elasticsearch
    restart: always
    ports:
      - 9200:9200
      - 9300:9300
    environment:
      cluster.name: elasticsearch
```

执行

```shell
docker-compose up -d
```

访问: <http://192.168.18.44:9200/>

### 安装 SkyWalking

下载地址: <http://skywalking.apache.org/downloads/>

下载window版本 *6.4*

修改  `config/application.yml` 文件

用什么存储，就修改什么，默认H2，我们用ES

```yaml
storage:
  elasticsearch:
#    nameSpace: ${SW_NAMESPACE:""}
    clusterNodes: ${SW_STORAGE_ES_CLUSTER_NODES:192.168.18.44:9200}
#    protocol: ${SW_STORAGE_ES_HTTP_PROTOCOL:"http"}
#    trustStorePath: ${SW_SW_STORAGE_ES_SSL_JKS_PATH:"../es_keystore.jks"}
#    trustStorePass: ${SW_SW_STORAGE_ES_SSL_JKS_PASS:""}
#    user: ${SW_ES_USER:""}
#    password: ${SW_ES_PASSWORD:""}
    indexShardsNumber: ${SW_STORAGE_ES_INDEX_SHARDS_NUMBER:2}
    indexReplicasNumber: ${SW_STORAGE_ES_INDEX_REPLICAS_NUMBER:0}
#    # Those data TTL settings will override the same settings in core module.
#    recordDataTTL: ${SW_STORAGE_ES_RECORD_DATA_TTL:7} # Unit is day
#    otherMetricsDataTTL: ${SW_STORAGE_ES_OTHER_METRIC_DATA_TTL:45} # Unit is day
#    monthMetricsDataTTL: ${SW_STORAGE_ES_MONTH_METRIC_DATA_TTL:18} # Unit is month
#    # Batch process setting, refer to https://www.elastic.co/guide/en/elasticsearch/client/java-api/5.5/java-docs-bulk-processor.html
    bulkActions: ${SW_STORAGE_ES_BULK_ACTIONS:1000} # Execute the bulk every 1000 requests
    flushInterval: ${SW_STORAGE_ES_FLUSH_INTERVAL:10} # flush the bulk every 10 seconds whatever the number of requests
    concurrentRequests: ${SW_STORAGE_ES_CONCURRENT_REQUESTS:2} # the number of concurrent requests
#    metadataQueryMaxSize: ${SW_STORAGE_ES_QUERY_MAX_SIZE:5000}
#    segmentQueryMaxSize: ${SW_STORAGE_ES_QUERY_SEGMENT_SIZE:200}
#  h2:
#    driver: ${SW_STORAGE_H2_DRIVER:org.h2.jdbcx.JdbcDataSource}
#    url: ${SW_STORAGE_H2_URL:jdbc:h2:mem:skywalking-oap-db}
#    user: ${SW_STORAGE_H2_USER:sa}
#    metadataQueryMaxSize: ${SW_STORAGE_H2_QUERY_MAX_SIZE:5000}
#  mysql:
#    metadataQueryMaxSize: ${SW_STORAGE_H2_QUERY_MAX_SIZE:5000}
```

修改好后，启动 SkyWalking 在 bin/startup.bat 

访问地址: http://localhost:8081  默认是8080，但我的8080被jenkins占用了，所以修改了port

到这里 服务端配置完成

### 客户端使用

#### Java Agent探针

> 什么是探针 
>
> 互联网中探针早起是指 PHP探针
>
> 探针可以实时查看服务器硬盘资源、内存占用、网卡流量、系统负载、服务器时间等信息。

有三种方式部署 Java Agent

+ Java jar
+ idea 
+ Docker

新建一个项目 *spring-cloud-external-skywalking*

将agent包粘贴到该项目下

![](https://user-gold-cdn.xitu.io/2019/10/11/16db8adec573cb3a?w=360&h=323&f=png&s=17786)

IDEA中添加启动配置

> Run --> Edit Configurations 

修改每个参与调用链的项目的 Environment 的 VM options配置

```text
-javaagent:G:\Eclipse\spring\spring-cloud-alibaba\spring-cloud-external-skywalking\agent\skywalking-agent.jar   ## 探针项目的地址
-Dskywalking.agent.service_name=nacos-consumer ## 你的项目名
-Dskywalking.collector.backend_service=localhost:11800 ## 端口
```

启动多个项目，通过网关层服务进行请求



这种拓扑图看着就很舒服

![](https://user-gold-cdn.xitu.io/2019/10/11/16db8be20d62531a?w=1061&h=578&f=png&s=38549)



请求情况， 总耗时2s

![](https://user-gold-cdn.xitu.io/2019/10/11/16db8bd24fbd3cf8?w=1056&h=327&f=png&s=31311)



### Maven Assembly 插件

解决 工程依赖元素，模块，网站文档等其他文件存放到单个归档文件里

agent属于其他文件

#### 使用步骤

将 SkyWalking探针打包为tar.gz 为例，为后期持续集成构建Docker镜像做准备

##### POM

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>github.lucas</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
    </parent>

    <artifactId>spring-cloud-skywalking</artifactId>
    <packaging>jar</packaging>
    <name>spring-cloud-skywalking</name>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <executions>
                    <!-- 配置执行器 -->
                    <execution>
                        <id>make-assembly</id>
                        <!-- 绑定到 package 生命周期阶段上 -->
                        <phase>package</phase>
                        <goals>
                            <!-- 只运行一次 -->
                            <goal>single</goal>
                        </goals>
                        <configuration>
                            <finalName>skywalking</finalName>
                            <descriptors>
                                <!-- 配置描述文件路径 -->         <descriptor>src/main/resources/assembly.xml</descriptor>
                            </descriptors>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</project>
```



