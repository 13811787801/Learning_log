# ELK

## ELK是什么

Elasticsearch + Logstash + Kibana（ELK）是一套开源的日志管理方案
> Logstash：负责日志的采集，处理和储存  
Elasticsearch：负责日志检索和分析  
Kibana：负责日志的可视化(Web界面)

## ELK的整体操作流程

+ 首先是logstash收集各种日志并进行过滤
+ 然后将过滤后的内容发送到ES服务中
+ 最后开发/运维人员通过Kibana的页面查看ES中的日志数据

## References

[博客园:Angel挤一挤](cnblogs.com/sxdcgaq8080/p/10442696.html)
