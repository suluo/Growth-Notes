#### 条件

```
## for 循环完整后执行else，中途break跳出，则连else一起跳出
for xxx:
else:
```

#### None

```
if x is None  ## None
if not x  # None,  False, 空字符串"", 0, 空列表[], 空字典{}, 空元组()都相当于False
```

#### List

```
# # 交并 差
intersection = list(set(a).intersection(set(b)))
union = list(set(a).union(set(b))
difference = list(set(a).difference(set(b)))
```

#### 字典

```
import copy
>>> a = [1,2,3,4,['a','b']]  #原始对象
>>> b = a  #赋值，传对象的引用
>>> c = copy.copy(a)
>>> d = copy.deepcopy(a)

dict.keys()
dict.values()
for key, value in dict.iteritems()
```

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

#### 作用域

```
作用域：def class import 新的作用域，其他诸如逻辑循环都不会新添作用域
列表表达式：[for x in for y in ]for x in是内层
```

#### 常用

```
# 队列
from collections import deque
deque(maxlen=N)

# 最大最小
import heapq
nums = [1, 8, 2, 23, 7, -4, 18, 23, 42, 37, 2]
print(heapq.nlargest(3, nums)) # Prints [42, 37, 23]
print(heapq.nsmallest(3, nums)) # Prints [-4, 1, 2]

min(prices, key=lambda k: prices[k]) # Returns 'FB'
max(prices, key=lambda k: prices[k]) # Returns 'AAPL'

# 默认值类型字典
from collections import defaultdict
d = defaultdict(list)
# 有序字典
from collections import OrderedDict
d = OrderedDict()

# 只能访问一次
zip(a, b)

# 
a = {'x' : 1,'y' : 2,'z' : 3}
b = {'w' : 10,'x' : 11,'y' : 2}
# Make a new dictionary with certain keys removed
c = {key:a[key] for key in a.keys() - {'z', 'w'}}
# c is {'x': 1, 'y': 2}

from collections import Counter
word_counts = Counter(words)
# 出现频率最高的3个单词
top_three = word_counts.most_common(3)
print(top_three)
# Outputs [('eyes', 8), ('the', 5), ('look', 4)]

# 数据分组
from itertools import groupby

# random
shuffle() 打断顺序

# 生成器
from itertools import permutations
from itertools import combinations
reversed() 反向迭代
dropwhile() 跳过开头
islice() 切片
permutations() 打乱顺序
combinations 选择元素
combinations_with_replacement 同一元素可选择多次
enumerate
zip(a, b)
zip_longest(a, b) a,b长度不一致
from itertools import chain
chain()  多个对象执行相同的操作for i in chain(a, b)
heapq.merge(a, b) a,b 排序序列，比chain()多了排序
iter 函数一个鲜为人知的特性是它接受一个可选的 callable 对象和一个标记(结尾)值作为输入参数


yield from


# io
import os.path
# Get all regular files
names = [name for name in os.listdir('somedir')
        if os.path.isfile(os.path.join('somedir', name))]

from pprint import pprint

# 注解
def add(x:int, y:int) -> int:
    return x + y
```

#### 读写文件

```
# csv
with open('stocks.csv') as f:
    f_csv = csv.reader(f)
    headers = next(f_csv)
    for row in f_csv:
        # Process row
        ...

from collections import namedtuple
with open('stock.csv') as f:
    f_csv = csv.reader(f)
    headings = next(f_csv)
    Row = namedtuple('Row', headings)
    for r in f_csv:
        row = Row(*r)
        # Process row
        ...

# Example of reading tab-separated values
with open('stock.tsv') as f:
    f_tsv = csv.reader(f, delimiter='\t')
    for row in f_tsv:
        # Process row
        ...


headers = ['Symbol','Price','Date','Time','Change','Volume']
rows = [('AA', 39.48, '6/11/2007', '9:36am', -0.18, 181800),
         ('AIG', 71.38, '6/11/2007', '9:36am', -0.15, 195500),
         ('AXP', 62.58, '6/11/2007', '9:36am', -0.46, 935000),
       ]
with open('stocks.csv','w') as f:
    f_csv = csv.writer(f)
    f_csv.writerow(headers)
    f_csv.writerows(rows)

headers = ['Symbol', 'Price', 'Date', 'Time', 'Change', 'Volume']
rows = [{'Symbol':'AA', 'Price':39.48, 'Date':'6/11/2007',
        'Time':'9:36am', 'Change':-0.18, 'Volume':181800},
        {'Symbol':'AIG', 'Price': 71.38, 'Date':'6/11/2007',
        'Time':'9:36am', 'Change':-0.15, 'Volume': 195500},
        {'Symbol':'AXP', 'Price': 62.58, 'Date':'6/11/2007',
        'Time':'9:36am', 'Change':-0.46, 'Volume': 935000},
        ]

with open('stocks.csv','w') as f:
    f_csv = csv.DictWriter(f, headers)
    f_csv.writeheader()
    f_csv.writerows(rows)


foo-package/
    spam/
        blah.py

bar-package/
    spam/
        grok.py
>>> import sys
>>> sys.path.extend(['foo-package', 'bar-package'])
>>> import spam.blah
>>> import spam.grok

# 安装私有包
Python有一个用户安装目录，通常类似”~/.local/lib/python3.3/site-packages”。 要强制在这个目录中安装包，可使用安装选项“–user”。例如：
python3 setup.py install --user
或者
pip install --user packagename
在sys.path中用户的“site-packages”目录位于系统的“site-packages”目录之前。 因此，你安装在里面的包就比系统已安装的包优先级高 （尽管并不总是这样，要取决于第三方包管理器，比如distribute或pip）。
```



