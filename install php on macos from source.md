###安装依赖
下载页: http://sourceforge.net/projects/mhash/
当前版本: http://ncu.dl.sourceforge.net/project/mhash/mhash/0.9.9.9/mhash-0.9.9.9.tar.gz
```
$ tar zxvf mhash-0.9.9.9.tar.gz
$ cd mhash-0.9.9.9
$ ./configure --prefix=/usr/local/lib/libmhash
$ make
$ sudo make install
```


下载页: http://mcrypt.hellug.gr/lib/
当前版本: ftp://mcrypt.hellug.gr/pub/crypto/mcrypt/libmcrypt/libmcrypt-2.5.7.tar.gz
```
$ tar zxvf libmcrypt-2.5.7.tar.gz
$ cd libmcrypt-2.5.7
$ ./configure --prefix=/usr/local/lib/libmcrypt
$ make
$ sudo make install
```


下载页: http://www.gnu.org/software/libiconv/
当前版本: http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.14.tar.gz
$ tar zxvf libiconv-1.14.tar.gz
$ cd libiconv-1.14
$ ./configure --prefix=/usr/local/lib/libiconv
$ make
$ sudo make install


下载页: http://www.libpng.org/pub/png/libpng.html
当前版本: http://ncu.dl.sourceforge.net/project/libpng/libpng16/1.6.17/libpng-1.6.17.tar.gz
```
$ tar zxvf libpng-1.6.17.tar.gz
$ cd libpng-1.6.17
$ ./configure --prefix=/usr/local/lib/libpng
$ make
$ sudo make install
```

下载页: http://www.ijg.org/
当前版本: http://www.ijg.org/files/jpegsrc.v9a.tar.gz
```
$ tar zxvf jpegsrc.v9a.tar.gz
$ cd jpeg-9a
$ ./configure --prefix=/usr/local/lib/libjpeg
$ make
$ sudo make install
```

在安装的过程中提示configure: error: Cannot find OpenSSL's <evp.h>
可能是系统自带的openssl提供的库文件的位置不对，可以自行通过brew install openssl ,然后在--with-openssl后面制定安装的位置
```
$ tar zxvf php-5.6.8.tar.gz
$ cd php-5.6.8
$ ./configure \
    --prefix=/usr/local/php \
    --with-config-file-path=/usr/local/php \
    --with-mysql \
    --with-mysqli \
    --enable-pdo \
    --with-pdo-mysql \
    --with-mysql-sock=/tmp/mysql.sock \
    --enable-opcache \
    --enable-cgi \
    --enable-fpm \
    --enable-sockets \
    --enable-mbstring \
    --enable-mbregex \
    --enable-bcmath \
    --enable-xml \
    --enable-zip \
    --with-zlib \
    --with-gd \
    --with-png-dir=/usr/local/lib/libpng \
    --with-jpeg-dir=/usr/local/lib/libjpeg \
    --with-openssl=/usr/local/Cellar/openssl/1.0.2j \
    --with-curl \
    --with-mhash=/usr/local/lib/libmhash \
    --with-mcrypt=/usr/local/lib/libmcrypt \
    --with-iconv=/usr/local/lib/libiconv \
    --with-readline
$ make
$ sudo make install
$ sudo cp php.ini-development /usr/local/php/php.ini
$ cd /usr/local/php/etc
$ sudo cp php-fpm.conf.default php-fpm.conf
```

