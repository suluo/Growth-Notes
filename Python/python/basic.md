#### 字典

##### 删除字典

```
del dict2['name']#删除键为“name”的条目。
dict2.clear()#删除 dict2 中所有的条目
del dict2#删除整个 dict2 字典
dict2.pop('name')#删除并返回键为“name”的条目
```

##### update

```
dict3.update(dict1)#将dict1加入到dict3中
```

#### 排序

```
sort(cmp=None, key=None, reverse=False)   #改变原有序列
sorted(iterable, cmp=None, key=None, reverse=False)    #不改变原有序列
## cmp 比较函数， key=lambda x:x

### 示例
>>> a=[1,2,5,3,9,4,6,8,7,0,12]
>>> a.sort()

>>> a=[1,2,5,3,9,4,6,8,7,0,12]
>>> sorted(a)
```

#### 传参

不可变对象传值，如int，tuple，str等，可变对象传引用，如list，set，dict等

```
from copy import copy，deepcopy
a = {}
b = copy(a) # 浅复制
c = deepcopy(a) # 深复制

def p1(*arg1,**arg2):     # 函数定义arg1 tuple序列， arg2 dict
    a, b, c = arg1
p1（1，2，3，dict）
```





