## es

```
docker pull  docker.elastic.co/elasticsearch/elasticsearch:7.4.1


docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.4.1

# 校验
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

## logstash

```
docker pull docker.elastic.co/logstash/logstash:7.4.1

# 你设置的地址
docker run -d -p 5044:5044 -p 9600:9600 -it -v /usr/local/logstash/config/:/usr/share/logstash/config/  docker.elastic.co/logstash/logstash:7.4.1
```

## kibana

```text
docker pull docker.elastic.co/kibana/kibana:7.4.1

## 不需要 -p 打开对外开放的 不推荐使用这种(需要外网的情况下)
docker run -it -d -e ELASTICSEARCH_URL=http://127.0.0.1:9200  --name kibana --network=container:es docker.elastic.co/kibana/kibana:7.4.1


docker run --name kibana -e ELASTICSEARCH_URL=http://es:9200 -p 5601:5601 -d docker.elastic.co/kibana/kibana:7.4.1
##

```