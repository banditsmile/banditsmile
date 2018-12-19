 1.部署环境:
nsqlookupd节点:
10.33.13.231 tw06950s6 tcp: 9101 http: 9151

nsqd节点:
10.33.13.231 tw06950s6 tcp: 9201 9202 http: 9251 9252 

nsqadmin节点:
10.33.13.231 tw06950s6 http: 9301

software部署目录:
/data/nsq

logs目录:
/data/logs/nsq

数据目录
/data/nsq/data_9202

 

2.部署步骤
tw06950s6 节点:
# cd /data
# wget https://s3.amazonaws.com/bitly-downloads/nsq/nsq-1.0.0-compat.linux-amd64.go1.8.tar.gz
# tar -zxvf nsq-1.0.0-compat.linux-amd64.go1.8.tar.gz
# mv nsq-1.0.0-compat.linux-amd64.go1.8 nsq
# cd nsq/bin
# mkdir -p /data/logs/nsq/
# mkdir -p /data/nsq/data_9202


      启动nsqlookupd节点

//tw06950s6 10.33.13.231
# nohup ./nsqlookupd -broadcast-address=10.33.13.231 \
-tcp-address=10.33.13.231:9101 \
-http-address=10.33.13.231:9151 >> /data/logs/nsq/nsqlookupd_9101.log 2>&1 &

启动nsqd节点

//tw06950s6 10.33.13.231
   # nohup ./nsqd -broadcast-address=10.33.13.231 \
-data-path=/data/nsq/data_9201 \
-tcp-address=10.33.13.231:9201 \
-http-address=10.33.13.231:9251 \
-lookupd-tcp-address=10.33.13.231:9101  >> /data/logs/nsq/nsqd_9201.log 2>&1 &

# nohup ./nsqd -broadcast-address=10.33.13.231 \
-data-path=/data/nsq/data_9202 \
-tcp-address=10.33.13.231:9202 \
-http-address=10.33.13.231:9252 \
-lookupd-tcp-address=10.33.13.231:9101  >> /data/logs/nsq/nsqd_9202.log 2>&1 &

启动nsqadmin节点
     // tw06950s6 10.33.13.231
    # nohup ./nsqadmin -http-address="0.0.0.0:9301" \
-lookupd-http-address="10.33.13.231:9151"  >> /data/logs/nsq/nsqadmin_9301.log 2>&1 &

查看管理控制台

浏览器输入http://61.155.183.138:9301

 

外网环境
消费方：

tw06895s5
tw06895s4
tw06895s3
tw06869s7
