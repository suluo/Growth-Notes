tqdm

```py
from tqdm import tqdm
tqdm(iterator)

# pandas
import pandas
df = pandas.read_csv(filepath, sep="\t", header=None, names=[])
for index, row in tqdm(df.iterrows(), total=df.shape[0]):
   print("index",index)
   print("row",row)
```

作用：是一个快速、扩展性强的进度条工具库

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

##### [LivePython](https://github.com/agermanidis/livepython)

作用：Watch your Python run like a movie.

##### [Halo](https://github.com/ManrajGrover/halo)

```
$ pip install halo

from halo import Halo

@Halo(text='Loading', spinner='dots')
def long_running_function():
    # Run time consuming work here
    pass

long_running_function()
```

作用：Beautiful terminal spinners for Python。

