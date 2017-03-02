shadowsocks server目前有两个版本
1. [shadowsocks(python)](https://github.com/shadowsocks/shadowsocks)
```
ubuntu 可以通过apt直接安装,但是由于原作者被请去喝茶,源码已经从github删除,
估计后续不会再继续开发维护
```
2. [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)
```
shadowsocks的c语言版本,当前正在开发和维护
ubuntu 16.10以后可以直接从使用apt进行安装,16.10以前的版本可以手动设置安装源进行安装
```
3. [shadowsocks-go](https://github.com/shadowsocks/shadowsocks-go)
```
shadowsocks的go语言版本
```
shadowsocks的github wiki上面有[不同版本的server以及客户端的对比](https://github.com/shadowsocks/shadowsocks/wiki/Feature-Comparison-across-Different-Versions)

鉴于目前shadowsocks-libev是开发最活跃的版本,所以在安装的时候我优先考虑shadowsocks-libev.

shadowsocks-libev安装实践
```
#安装依赖
sudo apt-get install --no-install-recommends gettext build-essential autoconf libtool libpcre3-dev asciidoc xmlto libev-dev libudns-dev automake libmbedtls-dev
#构建
./autogen.sh && ./configure --prefix=/usr/local/shadowsocks-server && make
#安装
sudo make install
```

放开firewalld对应的端口
```
firewall-cmd --permanent --add-port=8388/tcp
firewall-cmd --reload
```
