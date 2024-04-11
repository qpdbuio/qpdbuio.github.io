+++
title = "安装rocketmq"
weight = 8
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: "3"
services:
  namesrv:
    image: apache/rocketmq:4.9.4
    container_name: rocketmq-namesrv
    ports:
      - "9876:9876"
   volumes:
      - ./data/namesrv:/home/rocketmq/namesrv/
    command: sh mqnamesrv
    networks:
      - test-rocketmq
  broker:
    image: apache/rocketmq:4.9.4
    container_name: rocketmq-broker
    ports:
      - "10909:10909"
      - "10911:10911"
      - "10912:10912"
    volumes:
      - ./data/broker/logs:/home/rocketmq/logs/rocketmqlogs
      - ./data/broker/store:/home/rocketmq/store
      - ./data/broker/conf/broker.conf:/home/rocketmq/rocketmq-4.9.4/conf/broker.conf
    environment:
      NAMESRV_ADDR: "namesrv:/9876"
      JAVA_OPTS: "-Xms1024m -Xmx1024m -Xmn200m -Drocketmq.namesrv.addr=namesrv:9876"
    command: sh mqbroker -n namesrv:9876  -c ../conf/broker.conf
    depends_on:
      - namesrv
    networks:
      - test-rocketmq
  console:
    image: apacherocketmq/rocketmq-dashboard:latest
    container_name: rocketmq-console
    ports:
      - "19876:8080"
    environment:
      JAVA_OPTS: "-Xms1024m -Xmx1024m -Xmn200m -Drocketmq.namesrv.addr=namesrv:9876"
    depends_on:
      - namesrv
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}


### Master: ./data/broker/conf/broker.conf
{{< code >}}
#所属集群名字
brokerClusterName=DefaultCluster
##broker名称，主从要一样，根据BrokerRole来确定谁是主谁是从
brokerName=broker-a
#0 表示 Master，>0 表示 Slave
brokerId=0
#nameServer 地址，分号分割
namesrvAddr=10.11.90.120:9876;10.11.90.128:9876
#在发送消息时，自动创建服务器不存在的 topic，默认创建的队列数
defaultTopicQueueNums=4
#是否允许 Broker 自动创建 Topic，建议线下开启，线上关闭
autoCreateTopicEnable=true
#是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
autoCreateSubscriptionGroup=true
#Broker 对外服务的监听端口
#brokerIP1 当前broker监听的IP
# brokerIP1=192.168.3.27
#存在broker主从时，在broker主节点上配置了brokerIP2的话,broker从节点会连接主节点配置的#brokerIP2来同步。
# brokerIP2=192.168.88.131
# listenPort=10911
#删除文件时间点，默认凌晨 4 点
deleteWhen=04
#文件保留时间，默认 48 小时
fileReservedTime=120
#commitLog 每个文件的大小默认 1G
mapedFileSizeCommitLog=1073741824
#ConsumeQueue 每个文件默认存 30W 条，根据业务情况调整
mapedFileSizeConsumeQueue=300000
#destroyMapedFileIntervalForcibly=120000
#redeleteHangedFileInterval=120000
#检测物理文件磁盘空间 checkTransactionMessageEnable
diskMaxUsedSpaceRatio=88
#存储路径
# /home/rocketmq/logs/rocketmqlogs
# storePathRootDir=/home/rocketmq/store
storePathRootDir=/opt/rocketmq-data/broker/store
#限制的消息大小
maxMessageSize=65536
#flushCommitLogLeastPages=4
#flushConsumeQueueLeastPages=2
#flushCommitLogThoroughInterval=10000
#flushConsumeQueueThoroughInterval=60000
#Broker 的角色
#- ASYNC_MASTER 异步复制 Master
#- SYNC_MASTER 同步双写 Master
#- SLAVE
brokerRole=ASYNC_MASTER
#刷盘方式
#- ASYNC_FLUSH 异步刷盘
#- SYNC_FLUSH 同步刷盘
flushDiskType=ASYNC_FLUSH
#checkTransactionMessageEnable=false
#发消息线程池数量
#sendMessageThreadPoolNums=128
#拉消息线程池数量
#pullMessageThreadPoolNums=128
#默认该值为false，表示该Broker不承载系统自定义用于存储消息轨迹的topic
traceTopicEnable=false
#默认回查次数是15次，15次后还没成功就回滚
transactionCheckMax=15
#transactionCheckInterval参数：默认定时回查间隔时间是1分钟
transactionCheckInterval=60000
#延迟消息
messageDelayLevel=1s 5s 10s 15s 30s 1m 5m 10m 15m 30m 1h 3h 6h 12h 1d 7d 15d 30d
{{< /code >}}

### Slave: ./data/broker/conf/broker.conf
{{< code >}}
#所属集群名字
brokerClusterName=DefaultCluster
##broker名称，主从要一样，根据BrokerRole来确定谁是主谁是从
brokerName=broker-a
#0 表示 Master，>0 表示 Slave
brokerId=1
#nameServer 地址，分号分割
namesrvAddr=10.11.90.120:9876;10.11.90.128:9876
#在发送消息时，自动创建服务器不存在的 topic，默认创建的队列数
defaultTopicQueueNums=4
#是否允许 Broker 自动创建 Topic，建议线下开启，线上关闭
autoCreateTopicEnable=true
#是否允许 Broker 自动创建订阅组，建议线下开启，线上关闭
autoCreateSubscriptionGroup=true
#Broker 对外服务的监听端口
#brokerIP1 当前broker监听的IP
# brokerIP1=192.168.3.27
#存在broker主从时，在broker主节点上配置了brokerIP2的话,broker从节点会连接主节点配置的#brokerIP2来同步。
# brokerIP2=192.168.88.131
# listenPort=10911
#删除文件时间点，默认凌晨 4 点
deleteWhen=04
#文件保留时间，默认 48 小时
fileReservedTime=120
#commitLog 每个文件的大小默认 1G
mapedFileSizeCommitLog=1073741824
#ConsumeQueue 每个文件默认存 30W 条，根据业务情况调整
mapedFileSizeConsumeQueue=300000
#destroyMapedFileIntervalForcibly=120000
#redeleteHangedFileInterval=120000
#检测物理文件磁盘空间 checkTransactionMessageEnable
diskMaxUsedSpaceRatio=88
#存储路径
storePathRootDir=/opt/rocketmq-data/broker/store
#commitLog 存储路径
storePathCommitLog=/opt/rocketmq-data/broker/store/commitlog
#消费队列存储路径存储路径
storePathConsumeQueue=/opt/rocketmq-data/broker/store/consumequeue
#消息索引存储路径
storePathIndex=/opt/rocketmq-data/broker/store/index
#checkpoint 文件存储路径
storeCheckpoint=/opt/rocketmq-data/broker/store/checkpoint
#abort 文件存储路径
abortFile=/opt/rocketmq/broker/store/abort
#限制的消息大小
maxMessageSize=65536
#flushCommitLogLeastPages=4
#flushConsumeQueueLeastPages=2
#flushCommitLogThoroughInterval=10000
#flushConsumeQueueThoroughInterval=60000
#Broker 的角色
#- ASYNC_MASTER 异步复制 Master
#- SYNC_MASTER 同步双写 Master
#- SLAVE
brokerRole=SLAVE
#刷盘方式
#- ASYNC_FLUSH 异步刷盘
#- SYNC_FLUSH 同步刷盘
flushDiskType=ASYNC_FLUSH
#checkTransactionMessageEnable=false
#发消息线程池数量
#sendMessageThreadPoolNums=128
#拉消息线程池数量
#pullMessageThreadPoolNums=128
#默认该值为false，表示该Broker不承载系统自定义用于存储消息轨迹的topic
traceTopicEnable=false
#默认回查次数是15次，15次后还没成功就回滚
transactionCheckMax=15
#transactionCheckInterval参数：默认定时回查间隔时间是1分钟
transactionCheckInterval=60000
#延迟消息
messageDelayLevel=1s 5s 10s 15s 30s 1m 5m 10m 15m 30m 1h 3h 6h 12h 1d 7d 15d 30d
{{< /code >}}