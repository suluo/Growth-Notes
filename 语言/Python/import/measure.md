### 效率

#### timeit

```
import timeit

timeit.timeit(stmt='pass', setup='pass', timer=<default timer>, number=1000000)
### stmt:要执行的语句（字符串形式）， setup：构建代码环境。
timeit.timeit('"-".join(str(n) for n in range(100))', number=10000)
timeit.repeat('"-".join(str(n) for n in range(100))',repeat=3, number=10000)  # 重复执行3次

# 脚本中使用
def test():
	L = []
	for i in range(100):
		L.append(i)
if __name__ == '__main__':
	import timeit
	print timeit.timeit("test()", setup="from __main__ import test"))
	
	
t1 = timeit.Timer('sum(x)', 'x = (i for i in range(1000))')  
t1.timeit()

### python中使用
def myfun():
    for i in range(100):
        for j in range(2, 10):
            math.pow(i, 1/j)
t1 = timeit.timeit(stmt=myfun, number=100)
t2 = timeit.repeat(stmt=myfun, number=n, repeat=5)
```

### 质量

#### coverage

查看代码测试覆盖率等统计信息

```
$ pip install coverage

$ coverage run my_program.py arg1 arg2
$ coverage report   # 控制台输出结果
$ coverage html -d covhtml  #输入到covhtml
$ python -m SimpleHTTPServer 8805  #网页上查看

### 
$ coverage combine


## python中使用

import coverage

cov = coverage.coverage()
cov.start()

# .. run your code ..

cov.stop()
cov.save()
```



