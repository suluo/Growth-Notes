主函数

```
#! /usr/bin/env python
# -*- encoding: utf-8 -*-
from __future__ import division
import logging
import logging.handlers
import logging.config
import os
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
import time
from pymongo import MongoClient
import tornado
import tornado.httpserver
import tornado.ioloop
import tornado.autoreload
import tornado.escape
import tornado.web
from tornado.options import define, options

from handlers import *

logging.config.fileConfig("./conf/logging.conf")

define("port", default=8701, help="run on the given port", type=int)
define("dev", default=True, help="dev mode", type=bool)
define("num", default=2, help="process num", type=int)


class stopHandler(tornado.web.RequestHandler):
    def __init__(self, application, request, **kwargs):
        super(stopHandler, self).__init__(application, request)
        self.logger = self.application.logger

    def get(self, *args, **kwargs):
        # logging.info("Server stop")
        self.logger.info("Server stop")
        self.finish("finish will work")
        tornado.ioloop.IOLoop.instance().stop()

        ioloop = tornado.ioloop.IOLoop.instance()
        ioloop.add_callback(ioloop.stop)

        ioloop.add_callback(lambda x: x.stop(), iploop)


class Application(tornado.web.Application):
    def __init__(self):
        handlers = [
            (r'/', HomeHandler),
            (r'/home', HomeHandler),

            (r"/stop", stopHandler)
        ]
        settings = dict(
            template_path=os.path.join(os.path.dirname(__file__), "templates"),
            static_path=os.path.join(os.path.dirname(__file__), "static"),
            debug=options.dev
        )
        tornado.web.Application.__init__(self, handlers, **settings)
        self.logger = logging.getLogger(__name__)
        logging.info('init starting ...')
        t0, t1 = time.clock(), time.time()
        try:
            # self.sync_client = tornado.curl_httpclient.AsyncHTTPClient()
            # self.http_client = tornado.httpclient.HTTPClient()
            self.client = MongoClient("xx.xx.xxx", 27017)
        except Exception, e:
            logging.error("init error.%s" % e, exc_info=True)
            # self.logger.error("init error.%s" % e)
        finally:
            logging.info('init done -- takes cpu:%s-- wall:%s'
                         % (time.clock() - t0, time.time() - t1))


def main():
    tornado.options.parse_command_line()
    print '   Port:  ', options.port
    print '   Debug: ', options.dev

    if options.dev:
        http_server = tornado.httpserver.HTTPServer(Application(),
                                                    xheaders=True)
        http_server.listen(options.port)
        tornado.ioloop.IOLoop.instance().start()
    else:
        http_server = tornado.httpserver.HTTPServer(Application(),
                                                    xheaders=True)
        http_server.bind(options.port)
        http_server.start(options.num)    ### 参数为零则建立num_process个进程，一般=4

        # sockets = tornado.netutil.bind_sockets(8888)
        # tornado.process.fork_processes(0)
        # server = HTTPServer(app)
        # server.add_sockets(sockets)
        # IOLoop.current().start()
        tornado.ioloop.IOLoop.current().start()




if __name__ == '__main__':
    main()
```



