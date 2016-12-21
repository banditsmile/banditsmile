##在本地搭建prometheus.io镜像

###安装ruby
ruby有多种安装方式，这里我们选择比较通用的源码安装方式
1. 下载源码 
```
$ wget https://ruby.taobao.org/mirrors/ruby/ruby-2.3-stable.tar.gz
$ tar xzf ruby-2.3-stable.tar.gz
$ cd ruby-2.3
```
2. 编译配置
```
$ ./configure --prefix=/usr/local/ruby
```

3. 编译安装
```
$ make 
$ sudo make install
```

4. 把/usr/local/ruby/bin添加到path里面，这样我们可以在任何地方调用到ruby和gem
```
vi /etc/profile
在最后添加
export PATH=$PATH:/usr/local/ruby/bin
```
5. 更换gem镜像地址
```
$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
$ gem sources -l
*** CURRENT SOURCES ***

https://gems.ruby-china.org
# 请确保只有 gems.ruby-china.org
$ gem install rails
```

5.更换bundle镜像地址
```
$ bundle config mirror.https://rubygems.org https://gems.ruby-china.org
```



###初始化prometheus.io环境
1. 获取源码
```
$ git clone https://github.com/prometheus/docs.git
cd docs
```

2. 安装依赖
```
$ vim Gemfile
#将“gem 'nanoc'”修改为"gem 'nanoc', '~> 3.8'"
$ bundle
```

3. 生成静态文件
```
$ bundle exec nanoc
```

4. 搭建一个http服务器用来访问这些静态文件
```
# Rebuild the site whenever relevant files change:
$ bundle exec guard
# Start the local development server:
$ bundle exec nanoc view
```

如果3000端口以前没有被占用,这时候我们访问http://localhost:3000/就可以看到镜像站点了
