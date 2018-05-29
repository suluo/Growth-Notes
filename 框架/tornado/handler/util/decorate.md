装饰器文件

```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
############################################
# File Name    : decorator.py
# Created By   : Suluo - sampson.suluo@gmail.com
# Creation Date: 2018-04-27
# Last Modified: 2018-05-16 15:31:10
# Descption    :
# Version      : Python 3.6
############################################
from __future__ import division
from __future__ import print_function
from __future__ import unicode_literals
import argparse
import logging
import json
import logging.config
# logging.config.fileConfig('./conf/logging.conf')
# logger = logging.getLogger(__file__)
import functools
from functools import wraps
import time
from errors import MissingArgumentError


def check_args(*required_args):
    """check arguments of function"""
    print(required_args)

    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            for arg in required_args:
                if arg not in args[1]:
                    logging.error('missing required argument: %s', arg)
                    raise MissingArgumentError(arg)
            return func(*args, **kw)
        return wrapper
    return decorator


def check_params(func):
    @functools.wraps(func)
    def wrapper(*args, **kw):
        params = func(*args, **kw)
        for arg in args[1:]:
            if arg not in params:
                raise MissingArgumentError(reason=arg)
        return params
    return wrapper


def timethis(func, level='DEBUG'):
    '''
    Decorator that reports the execution time.
    '''
    @wraps(func)
    def wrapper(*args, **kwargs):
        t0, t1 = time.clock(), time.time()
        result = func(*args, **kwargs)
        # print("func %s taks cpu：%s wall：%s" % (func.__name__, time.clock()-t0, time.time()-t1))
        if level == 'DEBUG':
            logging.debug("%s taks cpu：%s wall：%s" % (func.__name__, time.clock()-t0, time.time()-t1))
        else:
            logging.info("%s taks cpu：%s wall：%s" % (func.__name__, time.clock()-t0, time.time()-t1))
        return result
    return wrapper


#class MissingArgumentError(Exception):
#    """Raised when a function is called and not all parameters can be filled from the context / config.
#    Attributes:
#        message -- explanation of which parameter is missing
#    """
#
#    def __init__(self, param):
#        self.message = 'missing parameter [%s]' % param
#
#    def __str__(self):
#        return self.message
#

def main(num):
    return num


if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument('-e', '--num', type=int, default=100, help='input num')
    args = parser.parse_args()
    main(args.num)



```



