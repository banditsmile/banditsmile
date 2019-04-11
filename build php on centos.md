
##编译前执行下列命令

添加组
```
groupadd www
```

添加php-fpm用户
```
useradd -c php-fpm-user -g www -M php-fpm
```

c和c++编译器
```
yum install -y gcc gcc-c++
```

PHP扩展依赖
```
yum install -y libxml2-devel openssl-devel libcurl-devel \
libjpeg-devel libpng-devel libicu-devel openldap-devel readline-devel
```

##编译指令
```
./configure --prefix=/usr/local/php\
 --with-libdir=lib64\
 --enable-fpm\
 --with-fpm-user=php-fpm\
 --with-fpm-group=www\
 --enable-mysqlnd\
 --with-mysql=mysqlnd\
 --with-mysqli=mysqlnd\
 --with-pdo-mysql=mysqlnd\
 --enable-opcache\
 --enable-pcntl\
 --enable-mbstring\
 --enable-soap\
 --enable-zip\
 --enable-calendar\
 --enable-bcmath\
 --enable-exif\
 --enable-ftp\
 --enable-intl\
 --with-openssl\
 --with-zlib\
 --with-curl\
 --with-gd\
 --with-zlib-dir=/usr/lib\
 --with-png-dir=/usr/lib\
 --with-jpeg-dir=/usr/lib\
 --with-gettext\
 --with-mhash\
 --with-ldap \
 --with-readline
```

##环境配置
```
cp php.ini-development /usr/local/php/php.ini
cd /usr/local/php/etc
cp php-fpm.conf.default php-fpm.conf
```

##服务配置
```
centos <7
cp $download_dir/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
chmod +x /etc/init.d/php-fpm
chkconfig php-fpm on
```

```
centos>=7
cp $download_dir/sapi/fpm/php-fpm.service  /usr/lib/systemd/system/
systemctl enable php-fpm
systemctl start php-fpm
```
