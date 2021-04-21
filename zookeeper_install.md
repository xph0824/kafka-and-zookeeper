#### zookeepr安装和使用

下载及文档地址 https://zookeeper.apache.org/doc/current/zookeeperStarted.html


下载完成后使用解压命令解压，并且移动
```
tar -zxvf zookeeper-3.6.3.tar.gz 
mv zookeeper-3.6.3 /usr/local/zookeeper
cd /usr/local/zookeeper
```

在zookeeper的conf目录中，有一个官方提供的配置文件模板zoo_sample.cfg，我们将复制一份，重命名为zoo.cfg，并修改它：
```
#Client-Server通信心跳时间
#Zookeeper 服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个 tickTime 时间就会发送一个心跳。tickTime以毫秒为单位。
tickTime=2000

#Leader-Follower初始通信时限
#集群中的follower服务器(F)与leader服务器(L)之间初始连接时能容忍的最多心跳数（tickTime的数量）。
initLimit=10

#Leader-Follower同步通信时限
#集群中的follower服务器与leader服务器之间请求和应答之间能容忍的最多心跳数（tickTime的数量）。
syncLimit=5

#快照数据文件目录
dataDir=/data/zookeeper/zkdata
#日志文件所在目录
dataLogDir=/data/zookeeper/zkdatalog
  
#客户端连接 Zookeeper 服务器的端口，Zookeeper 会监听这个端口，接受客户端的访问请求。
clientPort=2181

#集群服务器列表，几台服务器集群就配几个，此处的1,2,3是服务器对应的id
#集群信息（服务器编号，服务器地址，LF通信端口(集群间机器互相通讯)，选举端口）
server.1=10.98.41.62:2888:13888
#server.2=10.98.41.xx:2888:13888
#server.3=10.98.41.xx:2888:13888
 
#对于一个客户端的连接数限制，默认是60
#maxClientCnxns=60

#3.4.0中的新增功能：启用后，ZooKeeper自动清除功能会将autopurge.snapRetainCount最新快照和相应的事务日志分别保留在dataDir和dataLogDir中，并删除其余部分。默认值为3。最小值为3。
autopurge.snapRetainCount=3

#3.4.0中的新增功能：必须触发清除任务的时间间隔（以小时为单位）。设置为正整数（1或更大）以启用自动清除。预设为0。
autopurge.purgeInterval=1

```

手动创建dataDir 和 dataLogDir ：
```
mkdir -p /data/zookeeper/zkdata
mkdir -p /data/zookeeper/zkdatalog
```


启动、停止
```
./bin/zkServer.sh start/stop
```

连接ZooKeeper
```
./bin/zkCli.sh -server 127.0.0.1:2181

输出一些相关信息
Connecting to 127.0.0.1:2181
...
...

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
[zk: 127.0.0.1:2181(CONNECTED) 0]

在shell,使用help获取可以从客户端执行的命令的列表
```

CRUD操作实例
```
list： ls /***命令，用来查看目录信息
[zkshell: 8] ls /
[zookeeper]

create:这将创建一个新的znode并将字符串“ my_data”与该节点关联
[zkshell: 9] create /zk_test my_data
Created /zk_test
查看现在的目录
[zkshell: 11] ls /
[zookeeper, zk_test]

get命令来验证数据是否与znode关联，如下所示：
[zkshell: 12] get /zk_test
my_data
cZxid = 5
ctime = Mon Apr 19 11:32:09 CST 2021
mZxid = 5
mtime = Mon Apr 19 11:32:09 CST 2021
pZxid = 5
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0
dataLength = 7
numChildren = 0

set命令，用来修改与目录相关的数据
set /zk_test junk
cZxid = 5
ctime = Mon Apr 19 11:32:09 CST 2021
mZxid = 6
mtime = Mon Apr 19 11:32:09 CST 2021
pZxid = 5
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0
dataLength = 4
numChildren = 0
[zkshell: 15] get /zk_test
junk
cZxid = 5
ctime = Mon Apr 19 11:32:09 CST 2021
mZxid = 6
mtime = Mon Apr 19 11:32:09 CST 2021
pZxid = 5
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0
dataLength = 4
numChildren = 0

信息说明：
cZxid：节点创建时的zxid
ctime：节点创建时间
mZxid：节点最近一次更新时的zxid
mtime：节点最近一次更新的时间
pZxid：子节点的id
cversion：子节点数据更新次数
dataVersion：本节点数据更新次数
aclVersion：节点ACL(授权信息)的更新次数
ephemeralOwner：如果该节点为临时节点,ephemeralOwner值表示与该节点绑定的session id. 如果该节点不是临时节点,ephemeralOwner值为0
dataLength：节点数据长度，本例中为hello world的长度
numChildren：子节点个数

delete删除节点
[zkshell: 16] delete /zk_test
[zkshell: 17] ls /
[zookeeper]
[zkshell: 18]

```
更过命令信息
https://zookeeper.apache.org/doc/current/zookeeperCLI.html



