协程模块

支持协程的模块

```
https://github.com/aio-libs
aioredis、aiohttp、aioes、aiocassandra、aiomysql等等
```

#### asyncio.coroutine和yield from

```
@asyncio.coroutine
def smart_fib(n):
    index = 0
    a = 0
    b = 1
    while index < n:
        sleep_secs = random.uniform(0, 0.2)
        yield from asyncio.sleep(sleep_secs)
        print('Smart one think {} secs to get {}'.format(sleep_secs, b))
        a, b = b, a + b
        index += 1

@asyncio.coroutine
def stupid_fib(n):
    index = 0
    a = 0
    b = 1
    while index < n:
        sleep_secs = random.uniform(0, 0.4)
        yield from asyncio.sleep(sleep_secs)
        print('Stupid one think {} secs to get {}'.format(sleep_secs, b))
        a, b = b, a + b
        index += 1

if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    tasks = [
        asyncio.async(smart_fib(10)),
        asyncio.async(stupid_fib(10)),
    ]
    loop.run_until_complete(asyncio.wait(tasks))
    print('All fib finished.')
    loop.close()
```

#### async和await

```py
async def smart_fib(n):
    index = 0
    a = 0
    b = 1
    while index < n:
        sleep_secs = random.uniform(0, 0.2)
        await asyncio.sleep(sleep_secs)
        print('Smart one think {} secs to get {}'.format(sleep_secs, b))
        a, b = b, a + b
        index += 1

async def stupid_fib(n):
    index = 0
    a = 0
    b = 1
    while index < n:
        sleep_secs = random.uniform(0, 0.4)
        await asyncio.sleep(sleep_secs)
        print('Stupid one think {} secs to get {}'.format(sleep_secs, b))
        a, b = b, a + b
        index += 1

if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    tasks = [
        asyncio.ensure_future(smart_fib(10)),
        asyncio.ensure_future(stupid_fib(10)),
    ]
    loop.run_until_complete(asyncio.wait(tasks))
    print('All fib finished.')
    loop.close()
```

python3.7 +

```
asyncio.run(main()) # main 异步函数，调起异步函数获取结果
```



