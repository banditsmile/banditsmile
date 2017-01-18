###编译nginx支持lua

参考![nginx和lua安装教程](http://blog.kissdata.com/2014/11/14/nginx-lua-install.html)
注意事项
1. 教程中使用的lua-nginx-module-v0.9.13.tar.gz编译是出错，下载最新版本即可
2. 教程中在安装luajit的时候,make和make install的时候均设置了PREFIX,luajit官方文档指导在make 时候设置 ，实际上需要在make install的时候设置,make时候设置无效

测试配置
```
location /lua {
                #下面四行代码要写在http块里面,否则报错
                # 查找 *.lua
                #lua_package_path '/usr/local/nginx//lua_so/?.lua;;';
                # # 查找 *.so
                #lua_package_cpath '/usr/local/nginx/lua_so/?.so;;';
                
                #测试内容开始
                default_type 'text/plain';
                content_by_lua 'ngx.say("hello world!")';
        }

```
