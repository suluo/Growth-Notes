

```
class DemoHandler(tornado.web.RequestHandler):
    def __init__(self, application, request, *kwargs):
    super(DemoHandler, self).__init__(application, request)
    self.logger = self.application.logger
    self.url = "xxxx"

    @tornado.web.asynchronous
    @tornado.gen.coroutine
    def get(self,):
        url = self.url
        self.redirect(url)
```



