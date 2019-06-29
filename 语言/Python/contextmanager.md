contextmanager



```
class TimeThis:
    """上下文管理的 timethis"""
    def __init__(self, label):
        self.label = label

    def __enter__(self):
        self.start = time.time()

    def __exit__(self, exc_ty, exc_val, exc_tb):
        end = time.time()
        print('{}: {}'.format(self.label, end - self.start))


@contextmanager
def timethis(label):
    start = time.time()
    try:
        yield
    finally:
        end = time.time()
        print('{}: {}'.format(label, end - start))


with TimeThis(label=) as timethis:  # as: __enter__  # with 结束： __exit__ 可捕获异常
    do something
    
```



