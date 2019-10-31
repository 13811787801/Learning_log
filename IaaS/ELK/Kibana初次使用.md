第一次登录，我们配下 ES 日志
![](https://user-gold-cdn.xitu.io/2019/10/30/16e1b7f312b9b6b0?w=1370&h=624&f=png&s=115834)

选择RPM  
![](https://user-gold-cdn.xitu.io/2019/10/30/16e1b7fdaf0dc6bf?w=405&h=134&f=png&s=9996)

下面有指引,我们该如何做ES 日志


![](https://user-gold-cdn.xitu.io/2019/10/31/16e1fdc614d4cdd0?w=772&h=80&f=png&s=10112)

第二部:修改配置
`vi /etc/filebeat/filebeat.yml` 下面只是部分修改的
```bash
# 默认是关闭的
enable:true
# 第一个是默认的配置 第二个是docker的日志
paths:
    - /var/log/*.log
    - /var/log/messages

# Kibana 配置 
setup.kibana:
  host: "localhost:5601"

#------------- Elasticsearch output --------------
output.elasticsearch:
  hosts: ["localhost:9200"]
```
其他的根据页面上提示就好

现在我们来输出一下，等等查看日志
```bash
# 这句话让我们在控制台循环打印
docker run busybox sh -c 'while true;do echo "This test Docker Log by kibana";sleep 10;done;'
```
查询
![](https://user-gold-cdn.xitu.io/2019/10/31/16e2088adf6ada6a?w=1377&h=590&f=png&s=158409)
