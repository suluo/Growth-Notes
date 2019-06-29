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


# 使用类中的方法    
import time
from functools import wraps
def timethis(func):
    '''
    Decorator that reports the execution time.
    '''
    @wraps(func)
    def wrapper(self, *args, **kwargs):
        t0, t1 = time.clock(), time.time()
        result = func(self, *args, **kwargs)
        print self.timeout
        print("%s taks cpu：%s wall：%s".format(func.__name__, time.clock()-t0, time.time()-t1))
        return result
    return wrapperdef attach_wrapper(obj, func=None):
    if func is None:
        return partial(attach_wrapper, obj)
    setattr(obj, func.__name__, func)
    return func


def timethis(level, name=None, message=None):
    """
    Add logging to a function. level is the logging
    level, name is the logger name, and message is the
    log message. If name and message aren't specified,
    they default to the function's module and name.
    """
    def decorate(func):
        logname = name if name else func.__module__
        log = logging.getLogger(logname)
        logmsg = message if message else func.__name__
        t0, t1 = time.clock(), time.time()

        @wraps(func)
        def wrapper(*args, **kwargs):
            # log.log(level, logmsg)
            res = func(*args, **kwargs)
            msg = f"{logmsg} CPU {time.clock()-t0} WALL {time.time()-t1}"
            log.log(level, msg)
            return res
        return wrapper

        # Attach setter functions
        @attach_wrapper(wrapper)
        def set_level(newlevel):
            nonlocal level
            level = newlevel

        @attach_wrapper(wrapper)
        def set_message(newmsg):
            nonlocal logmsg
            logmsg = newmsg
    return decorate
    
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

参数类型 检查

```
from inspect import signature#python3才有的模块
def typeassert(*args,**kwargs):
    def decorator(fun):
        sig=signature(fun)
        btypes=sig.bind_partial(*args,**kwargs).arguments
        def wrapper(*funargs,**funkwargs):
            for name,stype in sig.bind_partial(*funargs,**funkwargs).arguments.items():
                if name in btypes:
                    if not isinstance(stype,btypes[name]):
                        raise TypeError("'%s' must be '%s'"%(name,btypes[name]))
            return fun(*funargs,**funkwargs)
        return wrapper    
    return decorator
    
from inspect import signature

def typeassert(*ty_args, **ty_kwargs):
    def decorate(func):
        # If in optimized mode, disable type checking
        if not __debug__:
            return func

        # Map function argument names to supplied types
        sig = signature(func)
        bound_types = sig.bind_partial(*ty_args, **ty_kwargs).arguments

        @wraps(func)
        def wrapper(*args, **kwargs):
            bound_values = sig.bind(*args, **kwargs)
            # Enforce type assertions across supplied arguments
            for name, value in bound_values.arguments.items():
                if name in bound_types:
                    if not isinstance(value, bound_types[name]):
                        raise TypeError(
                            'Argument {} must be {}'.format(name, bound_types[name])
                            )
            return func(*args, **kwargs)
        return wrapper
    return decorate


@typeassert(int,str)
def myFun(a,b):
    print(a,b)
myFun(12,"12")
```

可参数可不参数

```
import functools

def log(something):
    # if callable, this is a decorator, shape of @log
    if callable(something):
        func = something
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            print('call {}'.format(func.__name__))
            func(*args, **kwargs)
        return wrapper

    elif isinstance(something, str):
        # else, this is a decorator function, shape of @log(something)
        def decorator(func):
            @functools.wraps(func)
            def wrapper(*args, **kwargs):
                print('{} {}'.format(something, func.__name__))
                func(*args, **kwargs)
            return wrapper

        return decorator

    else:
        raise AttributeError("Attribute other than str is not supported")

@log
def f():
    pass

@log('execute')
def f2():
    pass

f()
f2()
```



