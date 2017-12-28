#### Redis命令

```
$ redis-cli -h host -p port -a password
redis 127.0.0.1:6379>
### 监测redis是否启动
redis 127.0.0.1:6379> PING
```

Redis常用命令

```
redis 127.0.0.1:6379> COMMAND KEY_NAME

# key
set key value
del key  # 删除key
exists key # 检查key存在

Expire key time_in_seconds  #给key设置过期时间
PERSIST key # 移除key的过期时间
PTTL key # 查看key剩余过期时间
TTL key # 剩余生存时间

RANDOMKEY # 返回随机的key
TYPE key # 返回key的类型
RENAME key1 key2 # key1 改名key2
keys ten* ## 查看以ten开头的keys *可以用pattern

# string
set key value
get key
GETRANGE key start end
MGET key1 key2
MSET key1 value1 key2 value2
INCR KEY # key中数字值加1

```

Python redis   

[Python操作redis](https://www.cnblogs.com/melonjiang/p/5342505.html)

```py
from redis import Redis
redis = Redis(host="", port=6379, db=0, password="")

# 集合
redis.srandmember('key', 2) # 从key中随机获取2个元素
redis.srem('key', 'bb', 'dd') # 从key中删除bb，dd


# 事务
pipeline = redis.pipeline()
#### pipeline 一系列redis操作
pipeline.execute()
```



