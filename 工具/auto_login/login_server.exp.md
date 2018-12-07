



login\_server.exp

```
#!/usr/bin/expect

trap {
set rows [stty rows]
set cols [stty columns]
stty rows $rows columns $cols < $spawn_out(slave,name)
} WINCH

set server [lindex $argv 0]
set username [lindex $argv 1]
set pwd [lindex $argv 2]

spawn ssh $username@$server
expect "Password"
send "$pwd\r"

expect "Select page"
send "0\r"

set server [lindex $argv 3]
set account [lindex $argv 4]
set pwd [lindex $argv 5]

expect "Select server"
send "$server\r"

expect "Input account"
send "$account\r"

expect "password"
send "$pwd\r"

interact
```

go 202

```
#!/bin/bash

basepath=$(dirname $0)

expect $basepath/login_server.exp 10.100.37.200 wangyangyang 'wyy123456' xx.xx.xx.xx robot 'pwd'
```



