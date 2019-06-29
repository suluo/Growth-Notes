**hashlib**

```
import hashlib

h1 = hashlib.md5()
str1 = str(query)
h1.update(str1.encode(encoding="utf-8))
sn = h1.hexdigest()
```



