

```
# 2000总量 200 并发 post_data.txt -> post 请求的数据
ab -c 200 -n 2000 -p post_data.txt -T 'application/json' http://url

# get 请求
ab -c 10 -n 100 http://a.ilanni.com/index.php
```



