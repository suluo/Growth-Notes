timeout

```
import async_timeout

with async_timeout.timeout(0.001):
    async with session.get('https://github.com') as r:
        await r.text()
```



