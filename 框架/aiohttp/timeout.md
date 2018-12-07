timeout

async\_timeout

```
import async_timeout

with async_timeout.timeout(0.001):
    async with session.get('https://github.com') as r:
        await r.text()
```

aiohttp.Timeout

```

with aiohttp.Timeout(1):　　# 配置http连接超时时间
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as resp:
            print(resp.status)
            print(await resp.read())
```



