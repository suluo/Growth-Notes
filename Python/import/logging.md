#### logging的三种方法

* 使用python代码显式创建loggers，handlers，和formatters并分别调用他们的配置函数

```
# basicConfig

logging.basicConfig(
    format="[ %(levelname)1.1s  %(asctime)s  %(module)s:%(lineno)d  %(name)s  ]  %(message)s",
    datefmt="%y%m%d %H:%M:%S",
    filemodel="a",
    filename="./data_dump.log",
    stream=sys.stdout, # 默认stderr, 和filename同时指定时，stream被忽略
    level=logging.INFO
)

# 显式： StreamHandler，FileHandler，RotatingFileHandler，TimedRotatingFileHandler
# console
console = logging.StreamHandler()
console.setLevel(logging.INFO)
formatter = logging.Formatter('%(name)-12s: %(levelname)-8s %(message)s')
console.setFormatter(formatter)
# add the handler to the root logger
# logging.getLogger("").addHandler(console)
logging.getLogger(__name__).addHandler(console)



# file
fileHandler = logging.FileHanlder("/data_dump.log", mode="w")
fileHandler.setLevel(logging.INFO)
formatter = logging.Formatter("")
fileHandler.setFormatter(formatter)
logging.getLogger("").addHandler(fileHandler)

# Now, we can log to the root logger, or any other logger. First the root...
logging.info('Jackdaws love my big sphinx of quartz.')

# Now, define a couple of other loggers which might represent areas in your
# application:
logger = logging.getLogger(__file__)
```

* logging.config.fileConfig\("logging.conf"\)

logger.conf

```
#logger.conf
###############################################
[loggers]
keys=root,tornado

[logger_root]
level=INFO
handlers=ConsoleHandler,timeHandler,fileHandler

[logger_tornado]
level=INFO
handlers=ConsoleHandler,timeHandler,fileHandler
qualname=tornado
propagate=0

###############################################
[handlers]
keys=ConsoleHandler,fileHandler,RotateHandler,timeHandler

[handler_ConsoleHandler]
class=StreamHandler
level=DEBUG
formatter=fmt01
args=(sys.stdout,)
#args=(sys.stderr,)

[handler_fileHandler]
class=FileHandler
level=ERROR
formatter=fmt01
args=('./log/log_error.log', 'a')

[handler_RotateHandler]
class=logging.handlers.RotatingFileHandler
level=INFO
formatter=fmt01
args=('./log/nouse.log', 'a', 10*1024*1024, 5)

[handler_timeHandler]
class=logging.handlers.TimedRotatingFileHandler
level=INFO
formatter=fmt01
#args=('./log/tst.log','S',20,10)
args=('./log/tornado.log','midnight',1,30)
###############################################
[formatters]
keys=fmt01,fmt03

[formatter_fmt01]
format=[ %(levelname)1.1s  %(asctime)s  %(module)s:%(lineno)d  %(name)s ]  %(message)s
datefmt=%y%m%d %H:%M:%S

[formatter_fmt03]
format=%(name)-12s: %(levelname)-8s %(message)s
datefmt=
```

* logging.config.dictConfig\(\)

```
import logging.config

logging.config.dictConfig({
    'version': 1,
    'disable_existing_loggers': True,
    'formatters': {
        'verbose': {
            'format': "[%(asctime)s] %(levelname)s [%(name)s:%(lineno)s] %(message)s",
            'datefmt': "%Y-%m-%d %H:%M:%S"
        },
        'simple': {
            'format': '%(levelname)s %(message)s'
        },
    },
    'handlers': {
        'null': {
            'level': 'DEBUG',
            'class': 'logging.NullHandler',
        },
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'verbose'
        },
        'file': {
            'level': 'DEBUG',
            'class': 'logging.RotatingFileHandler',
            # 当达到10MB时分割日志
            'maxBytes': 1024 * 1024 * 10,
            # 最多保留50份文件
            'backupCount': 50,
            # If delay is true,
            # then file opening is deferred until the first call to emit().
            'delay': True,
            'filename': 'logs/mysite.log',
            'formatter': 'verbose'
        }
    },
    'loggers': {
        '': {
            'handlers': ['file'],
            'level': 'info',
        },
    }
})
```

多进程log

[Python 中 logging 日志模块在多进程环境下的使用](http://www.cnblogs.com/restran/p/4743840.html)

```
from logging import getLogger, INFO
from cloghandler import ConcurrentRotatingFileHandler
logging.config.dictConfig({
    ...
    'handlers': {
        'file': {
            'level': 'DEBUG',
            # 如果没有使用并发的日志处理类，在多实例的情况下日志会出现缺失
            'class': 'cloghandler.ConcurrentRotatingFileHandler',
            # 当达到10MB时分割日志
            'maxBytes': 1024 * 1024 * 10,
            # 最多保留50份文件
            'backupCount': 50,
            # If delay is true,
            # then file opening is deferred until the first call to emit().
            'delay': True,
            'filename': 'logs/mysite.log',
            'formatter': 'verbose'
        }
    },
    ...
}）


[loggers]
keys=root

[handlers]
keys=hand01

[formatters]
keys=form01

[logger_root]
level=NOTSET
handlers=hand01

[handler_hand01]
class=handlers.ConcurrentRotatingFileHandler
level=NOTSET
formatter=form01
args=("rotating.log", "a", 512*1024, 5)

[formatter_form01]
format=%(asctime)s %(levelname)s %(message)s
```

备注：

* [修改源代码使得logging支持](https://www.jianshu.com/p/d615bf01e37b)多进程RotatingFileHandler



