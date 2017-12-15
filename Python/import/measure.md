### 效率

#### timeit

```

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



