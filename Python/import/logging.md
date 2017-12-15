#### logger.conf

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



