  在微信官网我们可以找到开发者工具的windows和mac版,没有linux版。通过网上搜索发现k该开发者工具是使用nwjs开发的，nwjs是跨平台的，
所以我们只需要把应用文件在ubuntu下面重新打包即可。

操作过程如下:
####下载nwjs 的sdk并解压
    $ wget -c http://dl.nwjs.io/v0.17.6/nwjs-sdk-v0.17.6-linux-x64.tar.gz
    $ tar -zxvf nwjs-sdk-v0.17.6-linux-x64.tar.gz


####把windows下微信开发者工具安装目录下面的package.nw目录下面的文件拷贝到nwjs sdk目录下面
    $ mv package.nw/* nwjs-sdk-v0.17.6-linux-x64


####启动nw
    $ cd nwjs-sdk-v0.17.6-linux-x64/
    $ ./nw
