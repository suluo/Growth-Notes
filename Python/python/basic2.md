#### [时间与日期处理](http://www.wklken.me/posts/2015/03/03/python-base-datetime.html)

### 函数运行时间

[10种检测Python程序运行时间、CPU和内存占用的方法](http://www.jb51.net/article/63244.htm)

##### 使用装饰器衡量函数执行时间

```
def speed_time(func):
    t0, t1 = time.clock(), time.time()
    func()
    logger.info("it takes cpu: %s---wall:%s" % (time.clock()-t0, time.time()-t1))
    return func


def spend_time(func):
    def _spend_time(*args, **kwargs):
        t0, t1 = time.clock(), time.time()
        ret = func(*args, **kwargs)
        logger.info("it takes cpu: %s---wall:%s" % (time.clock()-t0, time.time()-t1))
        return ret
    return _spend_time


import time
def time_me(fn):
  def _wrapper(*args, **kwargs):
    start = time.clock()
    fn(*args, **kwargs)
    print "%s cost %s second"%(fn.__name__, time.clock() - start)
  return _wrapper
#这个装饰器可以在方便地统计函数运行的耗时。
#用来分析脚本的性能是最好不过了。
#这样用：
@time_me
def test(x, y):
  time.sleep(0.1)
@time_me
def test2(x):
  time.sleep(0.2)
test(1, 2)
test2(2)
#输出：
#test cost 0.1001529524 second
#test2 cost 0.199968431742 second


import time
import functools
def time_me(info="used"):
  def _time_me(fn):
    @functools.wraps(fn)
    def _wrapper(*args, **kwargs):
      start = time.clock()
      fn(*args, **kwargs)
      print "%s %s %s"%(fn.__name__, info, time.clock() - start), "second"
    return _wrapper
  return _time_me

@time_me()
def test(x, y):
  time.sleep(0.1)

@time_me("cost")
def test2(x):
  time.sleep(0.2)
test(1, 2)
test2(2)


import cProfile
cProfile.run('time_1()') ###time_1()函数名
```

##### 使用timeit模块

```
python -m timeit -n 4 -r 5 -s "import timing_functions" "timing_functions.random_sort(2000000)"
### 这里的timing_functions 是python脚本名称
```

##### 使用Unix系统中的time命令

```
time -p python timing_functions.py
```

##### 使用cProfile模块

如果想知道每个函数和方法消耗了多少时间，以及这些函数被调用了多少次，可以使用cProfile模块。

```
python -m cProfile -s cumulative timing_functions.py
```

##### **使用line\_profiler模块**

line\_profiler模块可以给出执行每行代码所需占用的CPU时间。 -l表示逐行解释，-v表示输出详细结果

```
@profile
def random_sort2(n):
  l = [random.random() for i in range(n)]
  l.sort()
  return l
  
if __name__ == "__main__":
  random_sort2(2000000)

$ kernprof -l -v timing_functions.py
```

##### **使用memory\_profiler模块**

memory\_profiler模块用来基于逐行测量代码的内存使用。使用这个模块会让代码运行的更慢。

```
pip install memory_profiler
pip install psutil ## 建议安装psutil包，这样memory_profile会运行的快一点

## 与line_profiler相似，使用@profile装饰器来标识需要追踪的函数。接着输入
$ python -m memory_profiler timing_functions.py
```

##### **使用guppy包**

通过这个包可以知道在代码执行的每个阶段中，每种类型（str、tuple、dict等）分别创建了多少对象。

```
$ pip install guppy

from guppy import hpy
  
def random_sort3(n):
  hp = hpy()
  print "Heap at the beginning of the functionn", hp.heap()
  l = [random.random() for i in range(n)]
  l.sort()
  print "Heap at the end of the functionn", hp.heap()
  return l
  
if __name__ == "__main__":
  random_sort3(2000000)

$ python timing_functions.py
```

#### 

#### encode

判断文件编码

```
'''
    识别判断文件编码
    file 编码
    字符串 编码
    fencoding = chardet.detect('我')['encoding']
'''
import codecs
import chardet
encoding_map = {'GB2312': 'GB18030', 'GBK': 'GB18030'}
@spend_time
def get_fileencoding(filename):
    f = open(filename, 'rb')
    buf = f.read()
    fencoding = chardet.detect(buf)['encoding']
    f.close()
    return encoding_map[fencoding] if fencoding in encoding_map else fencoding
```

#### argparse

```
def main(num):
    return num


if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument('-e', '--num', type=int, default=100, help='demo input num')
    args = parser.parse_args()
    main(args.num)
```

#### 发送邮件

[自动发送邮件](http://www.runoob.com/python3/python3-smtp.html)

