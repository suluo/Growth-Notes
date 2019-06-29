retry

```
import time
def do_something():
    xxx

for i in range(5):
    try:
        do_something()
        break
    except:
        time.sleep(2)


from retry import retry

@retry(tries=5, delay=2)
def do_something():
    xxx

do_something()
```

##### 

##### 



