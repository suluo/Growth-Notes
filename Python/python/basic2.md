#### 时间与日期处理

##### [http://www.wklken.me/posts/2015/03/03/python-base-datetime.html](http://www.wklken.me/posts/2015/03/03/python-base-datetime.html)

#### speed\_time

函数运行时间

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
```

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

自动发送邮件[http://www.runoob.com/python3/python3-smtp.html](http://www.runoob.com/python3/python3-smtp.html%29%29\)

