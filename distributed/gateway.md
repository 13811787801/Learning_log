# Gateway / 网关服务

此处 Gateway 是指网关服务，不是Spring Cloud Gateway 网关框架

## Gateway 需要哪些功能

1. 请求路由  
这是网关层主要功能，没这个功能那还能算网关服务吗！

## Gateway 实战架构

![Gateway多层架构图](https://user-gold-cdn.xitu.io/2019/11/19/16e830f19c6ce65d?w=864&h=449&f=png&s=146646)

1. 多层Gateway架构：  
总的Gateway接收所有请求,分发到不同子系统,子系统也有二级Gateway 接收总Gateway的请求进行路由和分发 *好处是服务粒度可粗可细*相对的*系统架构也会变得复杂*
2. 粗犷架构  
一个Gateway负责所有服务的路由，分发
