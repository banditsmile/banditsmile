1. [docker安装教程](https://docs.docker.com/engine/installation/linux/ubuntulinux/)
```
https://docs.docker.com/engine/installation/linux/centos/

yum -y remove docker docker-common container-selinux
yum -y remove docker-selinux


yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://docs.docker.com/engine/installation/linux/repo_files/centos/docker.repo



yum makecache fast
yum -y install docker-engine
systemctl enable docker
vi /etc/systemd/system/multi-user.target.wants/docker.service
```
#ExecStart=/usr/bin/dockerd
ExecStart=/usr/bin/dockerd --registry-mirror=https://hcsmk3qk.mirror.aliyuncs.com
```
systemctl daemon-reload



systemctl start docker
docker run hello-world
```
2. [配置docker使用阿里云镜像](https://yeasy.gitbooks.io/docker_practice/content/install/mirror.html),我的加速器地址https://hcsmk3qk.mirror.aliyuncs.com
2. etcd安装教程[官方下载地址](https://github.com/coreos/etcd/releases/download/v2.3.8/etcd-v2.3.8-linux-amd64.tar.gz)
```
wget http://45.76.223.240/down/etcd-v2.3.8-linux-amd64.tar.gz
tar xzf etcd-v2.3.8-linux-amd64.tar.gz
cd etcd-v2.3.8-linux-amd64
mkdir -p /usr/local/etcd
cp etcd ./* /usr/local/etcd
vi /etc/bashrc #在最后添加PATH=$PATH:/usr/local/etcd/
soruce /etc/bashrc

#启动etcd
etcd -name etcd \
-data-dir /var/lib/etcd \
-listen-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
-advertise-client-urls http://0.0.0.0:2379,http://0.0.0.0:4001 \
>> /var/log/etcd.log 2>&1 &

#添加etcd端口到防火墙
iptables -I INPUT -p tcp --dport 2379 -j ACCEPT
iptables -I INPUT -p tcp --dport 4001 -j ACCEPT
service iptables restart

#检查etcd健康状态
etcdctl -C http://localhost:4001 cluster-health

```
