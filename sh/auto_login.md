auto\_login.sh

```
#!/usr/bin/expect -f

set ip "10.10.58.37"
set password "123456"
set timeout 10
spawn ssh suluo@$ip
expect {
    "*password:" { send "$password\r" }
}

interact
```



