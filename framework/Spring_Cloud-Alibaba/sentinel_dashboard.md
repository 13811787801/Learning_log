sentinel-dashboard是Sentinel仪表盘监控

> 提供机器发现，单机资源实时监控，集群资源汇总，规则管理的功能
>
> 单个dashboard 汇总资源只支持500个服务



## 构建Dockerfile

```dockerfile
FROM openjdk:8-jdk-alpine

ADD https://github.com/alibaba/Sentinel/releases/download/1.6.0/sentinel-dashboard-1.6.0.jar /sentinel-dashboard-1.6.0.jar
ENTRYPOINT ["java", "-Dserver.port=8080", "-Dcsp.sentinel.dashboard.server=localhost:8080", "-Dproject.name=sentinel-dashboard", "-jar", "/sentinel-dashboard-1.6.0.jar", "-Dfile.encoding=utf-8"]
```



```shell
// 构建镜像
docker build -t sentinel-dashboard .

// 运行镜像
docker run --name sentinel-dashboard \
    -it --rm -p 8719:8719 -p 8780:8080 sentinel-dashboard
```

8719 是sentinel应用端和控制台通信端口，8080是web控制端口(很多服务端口都是8080，我们修改到8780)

账户密码都是 `sentinel`



![](https://user-gold-cdn.xitu.io/2019/10/9/16dae3b4c951e5fa?w=988&h=525&f=png&s=25956)



Spring Cloud项目配置

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
    <version>2.1.0.RELEASE</version>
</dependency>
```



```yaml
spring:
  application:
    name: nacos-consumer
  cloud:
    nacos:
      discovery:
        server-addr: 192.168.18.44:8848
        weight: 1
    sentinel:
     transport:
        dashboard: 192.168.18.44:8780
        port: 8719
```

启动我们的消费者，多请求几次监控的服务，不然sentinel-dashboard不会刷新出nacos-consumer的页面

![](https://user-gold-cdn.xitu.io/2019/10/9/16daea1c0ea5aa16?w=1404&h=535&f=png&s=66812)

![](https://user-gold-cdn.xitu.io/2019/10/9/16daea1529391d44?w=1111&h=305&f=png&s=31203)

dashboard记录的是**实时记录**，每台机器都需要有熔断