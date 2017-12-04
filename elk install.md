elk安装体验
---
1. 创建安装目录
```
mkdir /data/elk
mkdir /data/elk/src
```
2. 下载安装包
```
cd /data/elk/src
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.2.tar.gz
tar xzf elasticsearch-5.6.2.tar.gz
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.6.2-linux-x86_64.tar.gz
tar xzf kibana-5.6.2-linux-x86_64.tar.gz
mv elasticsearch-5.6.2 ../elasticsearch
mv kibana-5.6.2-linux-x86_64.tar.gz ../kibana
```
3. 安装x-pack扩展
```
cd /data/elk/elasticsearch/
bin/elasticsearch-plugin install x-pack
cd /data/elk/kibana/
bin/kibana-plugin install x-pack
```

4. 启动elasticsearch
```
cd /data/elk/elasticsearch/
bin/elasticsearch
```
我用的是虚拟机只有512m内存，启动默认需要2g内存，所以在启动前需要修改config/jvm.options文件的默认配置
```
-Xms2g  
-Xmx2g
```
修改为
```
-Xms256m  
-Xmx256m
```
启动
```
bin/elasticsearch
```

5. 启动Kibana
vim config/jvm.options
```
server.host: "localhost"
elasticsearch.username: "user"
elasticsearch.password: "pass"
```
修改为
```
server.host: "youip"
elasticsearch.username: "elastic"
elasticsearch.password: "changeme"
```
启动
```
bin/kibana
```