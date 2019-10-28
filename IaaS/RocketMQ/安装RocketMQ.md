## 入门

[阿里中间件：十分钟入门](RocketMQ]<http://jm.taobao.org/2017/01/12/rocketmq-quick-start-in-10-minutes/>)

## 下载

[官网地址](<http://rocketmq.apache.org/docs/quick-start/>)
```bash
# 下载二进制包
wget https://www.apache.org/dyn/closer.cgi?path=rocketmq/4.5.1/rocketmq-all-4.5.1-bin-release.zip
# 解压
unzip rocketmq-all-4.5.1-bin-release.zip

# 备份到/usr/backup中
cp -r ./rocketmq-all-4.5.1 /usr/backup/myrocketmq
```
## 配置
### NAMESRV_ADDR环境 

NAMESRV_ADDR可以不在这声明，但必须在启动broker时加上“-n 服务器IP:端口”以绑定nameserver，这其实简化了操作简化操作，9876是nameserver的默认端口。

```
vim /etc/profile
# 增加内容
export ROCKETMQ_HOME=/usr/local/myrocketmq
export PATH=$ROCKETMQ_HOME/bin:$PATH
export NAMESRV_ADDR=localhost:9876
# 刷新profile
source /etc/profile
```


### Name Server

默认 RocketMQ Server 内存需要很大的

```bash
vim bin/runserver.sh
JAVA_OPT="${JAVA_OPT} -server -Xms4g -Xmx4g -Xmn2g -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
```

我修改成

```bash
JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx1g -Xmn512m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
```

启动nameserver : `./bin/mqnamesrv &`

如果看到  The Name Server boot success. serializeType=JSON 则成功

关闭nameserver在bin路径下执行以下命令：sh mqshutdown namesrv

### Broker

默认 RocketMQ Broker 内存需要很大的

```
vim bin/runbroker.sh
JAVA_OPT="${JAVA_OPT} -server -Xms8g -Xmx8g -Xmn4g"
```

我修改成

```
JAVA_OPT="${JAVA_OPT} -server -Xms1g -Xmx1g -Xmn512m"
```

启动Broker `nohup sh bin/mqbroker &`

执行命令`tail -f ~/logs/rocketmqlogs/broker.log`查看是否启动成功

如果有 boot success. serializeType=JSON and name server is localhost:9876，则成功

关闭broker在bin路径下执行以下命令：`sh mqshutdown broker`

### 模拟

bin 路径下执行以下命令模拟Producer发消息：

sh tools.sh org.apache.rocketmq.example.quickstart.Producer



执行后会发出很多条下面的命令，说明发送成功：(请主动关闭：ctrl+c)



在 bin路径下执行以下命令模拟Consumer发消息：

sh tools.sh org.apache.rocketmq.example.quickstart.Consumer

会收到和上面一样多的命令，说明接收成功，证明RocketMQ部署成功

### 修改Broker配置文件

```
vim conf/broker.conf

# 修改内容
 brokerClusterName = DefaultCluster
 brokerName = broker-a
 brokerId = 0
 deleteWhen = 04
 fileReservedTime = 48
 brokerRole = ASYNC_MASTER
 flushDiskType = ASYNC_FLUSH
 brokerIP1 = 47.***.86.*
 namesrvAddr = 47.***.86.*:9876
```

先杀掉进程，再重启nameserver和broker

启动Brock的命令要修改：

 `nohup sh mqbroker -c ../conf/broker.conf &`

使用命令创建Topic

sh mqadmin updateTopic -n localhost:9876 -b localhost:10911 -t sunfounder



