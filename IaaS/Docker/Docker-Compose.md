# Docker Compose

Docker Compose 实现快速部署分布式项目，定义和运行多个容器的**应用**

> Compose是Docker开源项目，负责实现对Docker 容器集群的快速编排
>
> 源码地址: https://github.com/docker/compose
>
> Compose由Python实现,实现调用Docker提供的API对容器进行管理

Compost 有两个重要的概念

+ Server/服务 一个应用容器(例如tomcat)
+ Project/项目 由一组容器组成的完整业务单元 在`docker-compose.yml`中定义
