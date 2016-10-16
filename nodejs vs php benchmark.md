硬件信息(acer-4738g 换了8g内存)
```
cpu:Intel(R) Core(TM) i5 CPU       M 480  @ 2.67GHz
内存:8g
```
软件信息
```
nodejs:v4.6.0
php:5.6.25
nginx:1.10.0 
os:ubuntu 14.04 lts
```

1.使用php 内置的web server(php >5.4)
```
$ cd ~/public_html
$ php -S localhost:8000
```
index.php
```
<?php
echo "<h1>Hello Swoole. #".rand(1000, 9999)."</h1>";
```

2.使用php的swoole扩展创建一个简单的http server
app.php
```
$http = new swoole_http_server("0.0.0.0", 9501);

$http->on('request', function ($request, $response) {
    $response->header("Content-Type", "text/html; charset=utf-8");
    $response->end("<h1>Hello Swoole. #".rand(1000, 9999)."</h1>");
});

$http->start();
```
运行方式 php app.php

3.使用nginx + php-fpm的方式
nginx配置使用https://www.nginx.com/resources/wiki/start/topics/recipes/codeigniter/
index.php
```
<?php
echo "<h1>Hello Swoole. #".rand(1000, 9999)."</h1>";
```

4.使用nginx+php-fpm+codeigniter框架
测试环境：从codeigniter的github仓库克隆现在的代码，nginx配置同第3个测试用例

5.使用nodejs
app.js
```
var http = require('http');

http.createServer(function (request, response) {

       	// 发送 HTTP 头部
       	// HTTP 状态值: 200 : OK
       	// 内容类型: text/plain
       	response.writeHead(200, {'Content-Type': 'text/plain'});

       	// 发送响应数据 "Hello World"
       	response.end('Hello World\n');
}).listen(8888);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
```
6.使用pm2 start app.js -i max 方式(脚本内容和第5个一样)

7.使用pm2 start app.js -x

8.使用nodejs + express框架
测试环境
```
$ npm install -g express-generator@4
$ express ./express.benchmark
npm start
```

测试指令
ab -n 1000 -c 100 http://localhost:9501/


测试结果
1.
```
ab -n 1000 -c 100 http://localhost:8000/
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
ab -n 1000 -c 100 http://localhost:8000/index.php
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8000

Document Path:          /index.php
Document Length:        28 bytes

Concurrency Level:      100
Time taken for tests:   0.163 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      154000 bytes
HTML transferred:       28000 bytes
Requests per second:    6119.35 [#/sec] (mean)
Time per request:       16.342 [ms] (mean)
Time per request:       0.163 [ms] (mean, across all concurrent requests)
Transfer rate:          920.29 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.9      0       4
Processing:     1   15   2.3     15      19
Waiting:        1   15   2.3     15      19
Total:          6   15   1.7     16      20

Percentage of the requests served within a certain time (ms)
  50%     16
  66%     16
  75%     16
  80%     16
  90%     17
  95%     17
  98%     18
  99%     19
 100%     20 (longest request)
```
2.
```
ab -n 1000 -c 100 http://localhost:9501/
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        swoole-http-server
Server Hostname:        localhost
Server Port:            9501

Document Path:          /
Document Length:        28 bytes

Concurrency Level:      100
Time taken for tests:   0.064 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      191000 bytes
HTML transferred:       28000 bytes
Requests per second:    15661.46 [#/sec] (mean)
Time per request:       6.385 [ms] (mean)
Time per request:       0.064 [ms] (mean, across all concurrent requests)
Transfer rate:          2921.23 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        1    3   0.6      2       5
Processing:     1    4   1.1      3       8
Waiting:        1    3   1.0      3       7
Total:          3    6   1.3      6      11
WARNING: The median and mean for the initial connection time are not within a normal deviation
        These results are probably not that reliable.

Percentage of the requests served within a certain time (ms)
  50%      6
  66%      6
  75%      6
  80%      7
  90%      8
  95%      9
  98%     10
  99%     11
 100%     11 (longest request)
```
3.
```
ab -n 1000 -c 100 http://benchmark.php.fpm/index.php
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking benchmark.php.fpm (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        nginx/1.10.0
Server Hostname:        benchmark.php.fpm
Server Port:            80

Document Path:          /index.php
Document Length:        28 bytes

Concurrency Level:      100
Time taken for tests:   0.174 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      200000 bytes
HTML transferred:       28000 bytes
Requests per second:    5752.88 [#/sec] (mean)
Time per request:       17.383 [ms] (mean)
Time per request:       0.174 [ms] (mean, across all concurrent requests)
Transfer rate:          1123.61 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.9      0       4
Processing:     4   16   2.8     16      29
Waiting:        4   16   2.8     16      29
Total:          9   17   3.1     16      30

Percentage of the requests served within a certain time (ms)
  50%     16
  66%     16
  75%     16
  80%     17
  90%     19
  95%     25
  98%     28
  99%     29
 100%     30 (longest request)
```
4.
```
ab -n 1000 -c 100 http://benchmark.nginx.php.fpm.codeigniter/
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking benchmark.nginx.php.fpm.codeigniter (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        nginx/1.10.0
Server Hostname:        benchmark.nginx.php.fpm.codeigniter
Server Port:            80

Document Path:          /
Document Length:        1905 bytes

Concurrency Level:      100
Time taken for tests:   4.387 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      2077000 bytes
HTML transferred:       1905000 bytes
Requests per second:    227.93 [#/sec] (mean)
Time per request:       438.735 [ms] (mean)
Time per request:       4.387 [ms] (mean, across all concurrent requests)
Transfer rate:          462.31 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.9      0       5
Processing:    21  418  69.1    425     524
Waiting:       21  418  69.1    425     524
Total:         26  419  68.3    425     525

Percentage of the requests served within a certain time (ms)
  50%    425
  66%    433
  75%    446
  80%    453
  90%    460
  95%    476
  98%    504
  99%    508
 100%    525 (longest request)
``` 
5.
```
ab -n 1000 -c 100 http://localhost:8888/
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8888

Document Path:          /
Document Length:        12 bytes

Concurrency Level:      100
Time taken for tests:   0.240 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      113000 bytes
HTML transferred:       12000 bytes
Requests per second:    4160.37 [#/sec] (mean)
Time per request:       24.036 [ms] (mean)
Time per request:       0.240 [ms] (mean, across all concurrent requests)
Transfer rate:          459.10 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   1.2      0       7
Processing:     3   23   4.4     24      28
Waiting:        3   22   4.4     24      28
Total:         10   23   3.3     24      28

Percentage of the requests served within a certain time (ms)
  50%     24
  66%     24
  75%     25
  80%     25
  90%     26
  95%     27
  98%     27
  99%     28
 100%     28 (longest request)
```
6.
```
ab -n 1000 -c 100 http://localhost:8888/
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8888

Document Path:          /
Document Length:        12 bytes

Concurrency Level:      100
Time taken for tests:   0.498 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      113000 bytes
HTML transferred:       12000 bytes
Requests per second:    2009.23 [#/sec] (mean)
Time per request:       49.770 [ms] (mean)
Time per request:       0.498 [ms] (mean, across all concurrent requests)
Transfer rate:          221.72 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   1.1      0       6
Processing:    18   48  13.4     44     110
Waiting:       14   47  13.1     44     105
Total:         24   48  13.2     45     110

Percentage of the requests served within a certain time (ms)
  50%     45
  66%     50
  75%     53
  80%     55
  90%     67
  95%     79
  98%     83
  99%     88
 100%    110 (longest request)
```
7.
```
ab -n 1000 -c 100 http://localhost:8888/
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8888

Document Path:          /
Document Length:        12 bytes

Concurrency Level:      100
Time taken for tests:   0.324 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      113000 bytes
HTML transferred:       12000 bytes
Requests per second:    3086.22 [#/sec] (mean)
Time per request:       32.402 [ms] (mean)
Time per request:       0.324 [ms] (mean, across all concurrent requests)
Transfer rate:          340.57 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   1.2      0       5
Processing:    19   31  12.3     27      72
Waiting:       19   31  12.2     27      72
Total:         19   31  13.1     27      74

Percentage of the requests served within a certain time (ms)
  50%     27
  66%     32
  75%     37
  80%     41
  90%     48
  95%     63
  98%     71
  99%     72
 100%     74 (longest request)
```
8.
```
ab -n 1000 -c 100 http://localhost:3000/
This is ApacheBench, Version 2.3 <$Revision: 1706008 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            3000

Document Path:          /
Document Length:        170 bytes

Concurrency Level:      100
Time taken for tests:   5.376 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      366000 bytes
HTML transferred:       170000 bytes
Requests per second:    186.02 [#/sec] (mean)
Time per request:       537.574 [ms] (mean)
Time per request:       5.376 [ms] (mean, across all concurrent requests)
Transfer rate:          66.49 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   1.3      0       7
Processing:   416  517 172.1    439    1251
Waiting:      416  517 172.1    439    1251
Total:        416  517 173.0    439    1252

Percentage of the requests served within a certain time (ms)
  50%    439
  66%    488
  75%    501
  80%    560
  90%    677
  95%    994
  98%   1152
  99%   1207
 100%   1252 (longest request)
```
