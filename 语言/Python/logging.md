```
#logger.ini
###############################################
[loggers]
keys=root,tornado

[logger_root]
level=INFO
handlers=ConsoleHandler,timeHandler,fileHandler,ConcurrentHandler

[logger_tornado]
level=INFO
handlers=ConsoleHandler,timeHandler,fileHandler,ConcurrentHandler
qualname=tornado
propagate=0

###############################################
[handlers]
keys=ConsoleHandler,fileHandler,RotateHandler,timeHandler,ConcurrentHandler

[handler_ConsoleHandler]
class=StreamHandler
level=DEBUG
formatter=fmt02
args=(sys.stdout,)
#args=(sys.stderr,)

[handler_fileHandler]
class=FileHandler
level=INFO
formatter=fmt01
args=('./log/myapp.log', 'a')

[handler_RotateHandler]
class=logging.handlers.RotatingFileHandler
level=INFO
formatter=fmt02
args=('./log/myapp.log', 'a', 10*1024*1024, 5)

[handler_timeHandler]
class=logging.handlers.TimedRotatingFileHandler
level=INFO
formatter=fmt02
#args=('./log/tst.log','S',20,10)
args=('./log/tornado.log','midnight',1,30)

[handler_ConcurrentHandler]
class=cloghandler.ConcurrentRotatingFileHandler
level=INFO
formatter=form01
args=("./log/concurrent.log", "a", 10*1024*1024, 20)

###############################################
[formatters]
keys=fmt01,fmt02,fmt03

[formatter_fmt01]
format=%(color)s[%(levelname)1.1s %(asctime)s %(module)s:%(lineno)d]%(end_color)s %(name)s %(message)s
datefmt=%y%m%d %H:%M:%S
colors={
    logging.INFO:4, #Blue
    logging.DEBUG:2, #green
    logging.WARNING:3, #Yellow
    logging.ERROR:1, #Red
}

[formatter_fmt02]
format=[ %(levelname)1.1s  %(asctime)s  %(module)s:%(lineno)d  %(name)s ]  %(message)s
datefmt=%y%m%d %H:%M:%S

[formatter_fmt03]
format=%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s
datefmt=%a, %d %b %Y %H:%M:%S

[formatter_fmt04]
format=%(name)-12s: %(levelname)-8s %(message)s
datefmt=
```

formatter

```
levelname
asctime
module
lineno
name
thread
filename
funcName
```



