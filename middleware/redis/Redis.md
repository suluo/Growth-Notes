#### Redis命令

```
$ ./redis-server ../redis.conf
$ redis-cli -h host -p port -a password
redis 127.0.0.1:6379>
### 监测redis是否启动
redis 127.0.0.1:6379> PING
```

Redis常用命令

```
redis 127.0.0.1:6379> COMMAND KEY_NAME

# key
> set key value
> del key  # 删除key
> exists key # 检查key存在

> Expire key time_in_seconds  #给key设置过期时间
> PERSIST key # 移除key的过期时间
> PTTL key # 查看key剩余过期时间
> TTL key # 剩余生存时间

> RANDOMKEY # 返回随机的key
> TYPE key # 返回key的类型
> RENAME key1 key2 # key1 改名key2
> keys ten* ## 查看以ten开头的keys *可以用pattern

# string
> set key value
> get key
> GETRANGE key start end
> MGET key1 key2
> MSET key1 value1 key2 value2
> INCR KEY # key中数字值加1

# Zrank 返回有序集中指定成员的排名。
> ZRANK key member
```

Python redis

[Python操作redis](https://www.cnblogs.com/melonjiang/p/5342505.html)

[https://www.jianshu.com/p/2639549bedc8](https://www.jianshu.com/p/2639549bedc8)

```py
from redis import Redis
redis = Redis(host="", port=6379, db=0, password="")

# 连接池
pool = redis.ConnectionPool(host='localhost', port=6379, decode_responses=True)
r = redis.Redis(connection_pool=pool)
r.set('food', 'beef', px=3)
# ex，过期时间（秒）
# px，过期时间（毫秒）
# nx，如果设置为True，则只有name不存在时，当前set操作才执行
# xx，如果设置为True，则只有name存在时，当前set操作才执行

# 集合
redis.srandmember('key', 2) # 从key中随机获取2个元素
redis.srem('key', 'bb', 'dd') # 从key中删除bb，dd
redis.


# 事务
pipeline = redis.pipeline()
#### pipeline 一系列redis操作
pipeline.execute()
```

发布订阅

简单发布订阅

[https://blog.csdn.net/xiemanR/article/details/73380058](https://blog.csdn.net/xiemanR/article/details/73380058)

[https://www.cnblogs.com/bigberg/p/8298411.html](https://www.cnblogs.com/bigberg/p/8298411.html)

发布订阅封装

[https://segmentfault.com/a/1190000016898228](https://segmentfault.com/a/1190000016898228)

生产者消费者

[https://www.jianshu.com/p/9c04890615ba](https://www.jianshu.com/p/9c04890615ba)

http://xzane.cc/2016/08/26/%E5%88%A9%E7%94%A8redis%E6%9E%84%E5%BB%BA%E5%A4%9A%E6%B6%88%E8%B4%B9%E8%80%85%E8%AF%B7%E6%B1%82%E9%98%9F%E5%88%97/

