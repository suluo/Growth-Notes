CountDown

```
# -*- coding: utf-8 -*-  

import time
import threading

class CountDownLatch(object):
    def __init__(self, count=1):
        self.count = count
        self.lock = threading.Condition()

    def count_down(self):
        self.lock.acquire()
        self.count -= 1
        if self.count <= 0:
            self.lock.notifyAll()
        self.lock.release()

    def await(self):
        self.lock.acquire()
        while self.count > 0:
            self.lock.wait()
        self.lock.release()



def co_request(url1, url2):
	countDown = CountDownLatch(2)

	for url, sleep, whiteCheck in [(url1, 5, True), (url2, 10, False) ]:
		t = threading.Thread(target = thread_http, args=(countDown, url, sleep, whiteCheck)) 
		t.start() 

	countDown.await()
	print "co_request finished "


def thread_http(countDown, url, sleep, whiteCheck):
	msg =  "thread #%d,%d  %s finished" % (sleep,  1 if whiteCheck else 0, url)

	print "%s begin" % msg
	time.sleep(sleep)
	print "%s finished" % msg

	if whiteCheck:
		countDown.count_down()

	countDown.count_down()



url1 = "http://url1"
url2 = "http://url2"
co_request(url1, url2)

```



