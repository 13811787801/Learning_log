

Spring Cloud Alibaba体系中 Nacos 提供了服务配置的功能，代替了 Spring Cloud Config 服务



下图服务配置模块

![](https://user-gold-cdn.xitu.io/2019/10/9/16daf82f3edc417f?w=1439&h=246&f=png&s=28955)

下面是我们nacos-register项目的配置内容 内容是该项目的`application.yml`

![](https://user-gold-cdn.xitu.io/2019/10/9/16dafc4face16af1?w=1416&h=316&f=png&s=42484)

>  Spring Boot 配置文件加载优先级
>
> bootstrap.properties -> bootstrap.yml -> application.properties -> application.yml 

新增maven依赖

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

创建 *bootstrap.properties* 文件(删除application.yml文件)

```properties
# 这里的应用名对应 Nacos Config 中的 Data ID，实际应用名称以配置中心的配置为准 
# 服务的注册会是nacos-config中配置的 spring.application.name而不是这里，如果有配置的话
spring.application.name=nacos-register-provider
# 指定 nacos-register-provider 项目的配置文件
spring.cloud.nacos.config.file-extension=yaml
# Nacos 地址
spring.cloud.nacos.config.server-addr=192.168.18.44:8848
```

## 测试配置动态更新

在我们的配置中新增

```yaml
dev:
 name: lucas
```

新增一个对外接口, 注意 @Value 是不会根据配置文件刷新的

```java
@Autowired
ConfigurableApplicationContext applicationContext;

@GetMapping("/echo/dev")
public String returnDevName() throws InterruptedException {
        String userName = applicationContext.getEnvironment().getProperty("dev.name");
        return userName + " Progromming this Project";
}
```

启动项目，访问接口后，修改nacos的配置，在访问接口，看看结果

`spring.cloud.nacos.config.refresh.enabled = false` 关掉动态刷新