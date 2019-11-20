# ELK

## ES

```bash
# 拉取环境
docker pull elasticsearch:6.5.4

# 启动容器 飞机群 
docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.5.4

# 启动容器 集群
docker run -d --name es1 -p 9200:9200 -p 9300:9300 --restart=always elasticsearch:6.5.4

docker run -d --name es2 -p 9201:9201 -p 9301:9301  --restart=always elasticsearch:6.5.4

# 校验
## 局域网ip也可以
> curl http://localhost:9200
{
  "name" : "a6c43709b3ff",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "rAlMCqv0Q262DU5-pZtwDA",
  "version" : {
    "number" : "7.4.1",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "fc0eeb6e2c25915d63d871d344e3d0b45ea0ea1e",
    "build_date" : "2019-10-22T17:16:35.176724Z",
    "build_snapshot" : false,
    "lucene_version" : "8.2.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

## kibana
```bash
# 拉取镜像
docker pull kibana:6.5.4

# 启动kibana
docker run -d --name kibana -p 5601:5601 --restart=always --link es:elasticsearch kibana:6.5.4

```

## logstash

部署 以及配置服务远程打日志

```bash

docker pull docker.elastic.co/logstash/logstash:6.5.4

# 你设置的地址
docker run -d -p 5044:5044 -p 9600:9600 --restart=always --name logstash logstash:6.5.4

# 修改docker内部配置
docker exec -it logstash bash

cd pipeline

vi logstash.conf
## 下面是文件内容
input{
        tcp {

                port => 5044
                codec => json_lines
        }
}
output{
        elasticsearch{
                hosts=>["http://192.168.92.130:9200"]
                index => "user-%{+YYYY.MM.dd}"
                }
        stdout{codec => rubydebug}
}



```
