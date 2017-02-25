ssh自动登录脚本，省去每次手动输入密码的烦恼。  
一共有三个文件组成  

1. ssh.ex  
    ```
    #!/usr/bin/expect  -f
    #  ./ssh.exp  192.168.1.11 user password
    set server [lrange $argv 0 0]
    set name [lrange $argv 1 1]
    set pass [lrange $argv 2 2]
    
    spawn ssh $name@$server
    match_max 100000
    expect "*?assword:*"
    send -- "$pass\r"
    send -- "\r"
    interact
    ```
2. ssh.sh
    ```
    #!/bin/sh
    echo $1
    account_info=`/bin/grep $1 ./acount.txt|xargs echo`;
    ./ssh.ex $account_info
    ```
3. acount.txt用来保存服务器账号和密码  
    ```
    服务器地址 用户名 密码  #还可以写一些备注信息
    10.10.98.3	root	123456  #我是备注
    ```
4. 使用方法  
    ```
    chmod a+x ssh.sh
    chmod a+x ssh.ex
    
    ./ssh.sh 98.3
    98.3
    spawn ssh root@10.10.98.3
    root1@10.10.98.3's password: 
    Last login: Tue Feb 21 15:55:04 2017 from 10.0.3.143
    ```
