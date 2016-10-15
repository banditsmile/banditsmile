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
./test.ex bandit 192.168.1.104 xlh1234
spawn ssh bandit@192.168.1.104 -p 22
bandit@192.168.1.104's password:
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-42-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

21 packages can be updated.
1 update is a security update.

Last login: Sat Oct 15 14:07:43 2016 from 192.168.1.240
bandit@bandit-Aspire-4738G:~$ ls
Desktop  Documents  Downloads  examples.desktop  Music  pCloudDrive  pCloud Sync  Pictures
```
