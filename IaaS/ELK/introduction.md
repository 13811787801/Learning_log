## ELK是什么
Elasticsearch + Logstash + Kibana（ELK）是一套开源的日志管理方案
> Logstash：负责日志的采集，处理和储存  
Elasticsearch：负责日志检索和分析  
Kibana：负责日志的可视化(Web界面)


## ELK的整体操作流程
+ 首先是logstash收集各种日志并进行过滤
+ 然后将过滤后的内容发送到ES服务中
+ 最后开发/运维人员通过Kibana的页面查看ES中的日志数据


## Docker 部署

```bash
# 拉取镜像
docker pull sebp/elk  
# 部署
docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -itd -e ES_HEAP_SIZE="2g" -e LS_HEAP_SIZE="1g" --name elk sebp/elk 
## 资源运行的话不要修改默认的大小
docker run -p 5601:5601 -p 9200:9200 -p 5044:5044 -itd  --name elk sebp/elk 
```
> #  5601 代表kibana端口 5044代表Logstash 9200代表ES

 