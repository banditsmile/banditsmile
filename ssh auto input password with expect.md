ssh.ex
```
#!/usr/bin/expect

set timeout 5
set user [lindex $argv 0]
set host [lindex $argv 1]
set password [lindex $argv 2]

spawn ssh $user@$host -p 22
expect {
       	"(yes/no)" { send "yes\r"; exp_continue }
       	"password:" { send "$password\r" }
}
interact
```

使用方法
```
chmod +x ./ssh.ex
./ssh.ex user host password
```
