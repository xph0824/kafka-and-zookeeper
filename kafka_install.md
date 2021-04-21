#### kafka安装及使用

下载链接
https://www.apache.org/dyn/closer.cgi?path=/kafka/2.7.0/kafka_2.13-2.7.0.tgz

使用解压命令解压
```
tar -xzf kafka_2.13-2.7.0.tgz
cd kafka_2.13-2.7.0
```

进入config目录修改配置文件 server.properties 
```
#给当前服务器指定一个id，你的多台服务器的这个值要设置成不一样的
broker.id=0

#日志文件所在目录
log.dirs=/data/kafka/logs/

#每个topic的分区数，可改可不改
num.partitions=1 

#zookeeper集群的地址和端口，ip地址要和上面zookeeper配置的server.1=10.168.168.xx一致，端口号和zookeeper配置文件中的clientPort一致
zookeeper.connect=10.98.41.62:2181,10.98.41.62:2182,10.98.41.62:2183
```

启动
```
cd /usr/local/kafka/bin
./kafka-server-start.sh -daemon ../config/server.properties
```

创建topic（主题）
```
./bin/kafka-topics.sh --create --topic test --bootstrap-server localhost:9092
```

显示主题信息
```
./bin/kafka-topics.sh --describe --topic test --bootstrap-server localhost:9092
Topic:quickstart-events  PartitionCount:1    ReplicationFactor:1 Configs:
    Topic: quickstart-events Partition: 0    Leader: 0   Replicas: 0 Isr: 0
```

往主题内写入事件
```
./bin/kafka-console-producer.sh --topic test --bootstrap-server localhost:9092
This is my no.1 event
This is my no.2 event
```

事件读取
```
$ ./bin/kafka-console-consumer.sh --topic test --from-beginning --bootstrap-server localhost:9092
This is my no.1 event
This is my no.2 event
```

查看主题列表
```
[root@openstack_dev kafka_2.7]# ./bin/kafka-topics.sh --list --zookeeper localhost:2181
__consumer_offsets
test
```

具体配置文件参数及说明，保存到本地html了












