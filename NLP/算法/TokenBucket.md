令牌桶算法

```
#!/usr/bin/env python
# coding:utf-8

import time


class TokenBucket(object):
    """
    令牌桶:
        在网络中传输数据时， 为了防止网络拥塞， 需要限制流出网络的流量，
        使流量以比较均匀的速度向外发送；

    令牌桶算法:
        控制发送到网络上数据的数， 并允许突发数据的发送;



    令牌桶思路:
        我们用1块令牌来代表发送1字节数据的资格,假设我们源源不断的发放令牌给程序,程序就有资格源源不断的发送数据,
        当我们不发放令牌给程序,程序就相当于被限流,无法发送数据了。

        接下来我们说说限速器,所谓限速器,就是让程序在单位时间内,最多只能发送一定大小的数据。假设在1秒发放10块令牌,
        那么程序发送数据的速度就会被限制在10bytes/s。如果1秒内有大于10bytes的数据需要发送,就会因为没有令牌而被丢弃。


        限速器改进:
            1秒产生10块令牌,但是我们把产生出来的令牌先放到一个桶里,当程序需要发送的时候,从桶里取令牌,不需要的时候,
            令牌就会在桶里沉淀下来,
    """

    # rate是令牌发放速度， capacity是桶的大小;
    def __init__(self, rate, capacity):
        self._rate = rate
        self._capacity = capacity
        self._current_amount = 0
        self._last_consume_time = int(time.time())

    # token_amount是发送数据需要的令牌数;
    def consume(self, token_amount):
        # 计算令牌桶中令牌的数量:(now_time-last_time)*speed， 从而避免程序阻塞;
        increment = (int(time.time() - self._last_consume_time))
        # 令牌容量不能超过桶容量;
        self._current_amount = min(
            increment + self._current_amount, self._capacity)

        if token_amount > self._current_amount:
            return  False

        self._last_consume_time = int(time.time())
        self._current_amount -= token_amount
```



