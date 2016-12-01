##配置nginx支持png|jpg|gif自动转换成webp格式　　
###编译安装libwebp　　
下载地址https://storage.googleapis.com/downloads.webmproject.org/releases/webp/index.html　　　

0.选择编译好的二进制包下载，然后拷贝到对应的安装目录(例如/usr/local/libwebp)即可　　　　

####如果在使用中报错可以尝试手动编译　　　　
1.下载libwebp源码，更具机器情况选择最新版本　　　　

2.执行编译　　　　

```
wget https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-0.5.1.tar.gz
tar xzf libwebp-0.5.1.tar.gz
 cd libwebp-0.5.1
 ./configure  --prefix=/usr/local/libwebp
 make
 sudo make install 
```

###nginx配置　　　　

0.检查$/usr/local/nginx/sbin/nginx -V检查一下nginx是否支持lua脚本,如果不支持请重新编译nginx　　　　

１．将下面的脚本保存在/usr/local/nginx/lua/webp.lua里面　　　　

```
function file_exists(name)
   local f=io.open(name,"r")
   if f~=nil then io.close(f) return true else return false end
end

local newFile = ngx.var.request_filename;
local originalFile = newFile:sub(1, #newFile - 5); -- 去掉 .webp 的后缀

if not file_exists(originalFile) then -- 原文件不存在
  ngx.exit(404);
  return;
end

os.execute("/usr/local/libwebp/cwebp -q 75 " .. originalFile  .. " -o " .. newFile);   -- 转换原图片到 webp 格式，这里的质量是 75 ，你也可以改成别的

if file_exists(newFile) then -- 如果新文件存在（转换成功）
    ngx.exec(ngx.var.uri) -- Internal Redirect
else
    ngx.exit(404)
end
```
2.检查$/usr/local/nginx/conf/mime.types里面有没有"image/webp webp;",如果没有则加上　　　　

3.在nginx的虚拟主机对应的配置文件server模块里面添加一下配置　　　　

```
    location /images {#改成你的图片路径前缀
         expires 365d;
         try_files $uri $uri/ @webp; # 如果文件不存在尝试生成 webp 图片
    }

    location @webp{
        #if ($uri ~ "/([0-9]+)/([a-zA-Z0-9-_]+)\.(png|jpg|gif)\.webp") { # 这里可以改成你自己的路径匹配，很重要，否则可能会导致别的文件被恶意覆盖等
            content_by_lua_file "/usr/local/nginx/lua/webp.lua";
        #}
    }
```
