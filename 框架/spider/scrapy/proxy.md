### downloadermiddleares {#toc_2}

#### 反爬与处理：

* 503            浏览器伪装， 用户代理池 user\_agent
* ip限制        IP代理池
* ajax，js     模拟浏览器
* 验证码        打码平台

备注：

* scrapy中如果请求2次就会放弃，说明该代理ip不行。
* 有时候需要为不同的网站设置不同的代理，譬如国内和国外两种不同代理
* [IP代理池](http://blog.csdn.net/u011781521/article/details/70194744?locationNum=4&fps=1)



**process\_request\(request,spider\)**

当每个request通过下载中间件时，该方法被调用，这里有一个要求，该方法必须返回以下三种中的任意一种：None,返回一个Response对象，返回一个Request对象或raise IgnoreRequest。三种返回值的作用是不同的。

* None:Scrapy将继续处理该request，执行其他的中间件的相应方法，直到合适的下载器处理函数\(download handler\)被调用,该request被执行\(其response被下载\)。

* Response对象：Scrapy将不会调用任何其他的process\_request\(\)或process\_exception\(\) 方法，或相应地下载函数；其将返回该response。 已安装的中间件的 process\_response\(\) 方法则会在每个response返回时被调用。

* Request对象：Scrapy则停止调用 process\_request方法并重新调度返回的request。当新返回的request被执行后， 相应地中间件链将会根据下载的response被调用。

* raise一个IgnoreRequest异常：则安装的下载中间件的 process\_exception\(\) 方法会被调用。如果没有任何一个方法处理该异常， 则request的errback\(Request.errback\)方法会被调用。如果没有代码处理抛出的异常， 则该异常被忽略且不记录。

备注：

* 访问https站点,要用https类型的代理。http同理



