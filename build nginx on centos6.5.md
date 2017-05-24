#安装依赖
```
yum -y install gcc gcc-c++ autoconf automake make
yum -y install zlib zlib-devel openssl 
yum -y install openssl-devel pcre pcre-devel
```

#[下载源码](http://nginx.org/en/download.html)
```
wget http://nginx.org/download/nginx-1.10.3.tar.gz
tar xzf nginx-1.10.3.tar.gz
cd nginx-1.10.3
```

#执行编译安装

```
./configure \
  --user=nobody \
  --group=nobody \
  --with-http_ssl_module \
  --with-http_flv_module \
  --with-http_stub_status_module \
  --with-http_gzip_static_module \
  --with-pcre \
  --with-file-aio \ 
  --with-http_image_filter_module \
  
make & make install
```
