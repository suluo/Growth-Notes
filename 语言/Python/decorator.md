耗时@timethis

```
import time
from functools import wraps
def timethis(func):
    '''
    Decorator that reports the execution time.
    '''
    @wraps(func)
    def wrapper(*args, **kwargs):
        t0, t1 = time.clock(), time.time()
        result = func(*args, **kwargs)
        print("%s taks cpu：%s wall：%s".format(func.__name__, time.clock()-t0, time.time()-t1))
        return result
    return wrapper
```

检查参数@check\_args

```
import functools
def check_args(*required_args):
    """check arguments of function"""
    print(required_args)
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            for arg in required_args:
                if arg not in args[1]:
                    logger.error('missing required argument: %s', arg)
                    raise MissingArgumentError(arg)
            return func(*args, **kw)
        return wrapper
    return decorator
    
    
class MissingArgumentError(Exception):
    """Raised when a function is called and not all parameters can be filled from the context / config.
    Attributes:
        message -- explanation of which parameter is missing
    """

    def __init__(self, param):
        self.message = 'missing parameter [%s]' % param

    def __str__(self):
        return self.message
```



