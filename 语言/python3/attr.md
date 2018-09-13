

getattr

setattr



系统函数

```
## attr 能够使用.

__getattr__

# .访问键值对
#  增加属性
#  组合优于继承：getattr 别的类的方法或属性
https://www.cnblogs.com/xybaby/p/6280313.html

__setattr__

# 通过属性赋值

__delattr__
# 删除属性


## __init__ & __new__

__new__
# 创建类实例的时候调用
http://python.jobbole.com/86506/
__init__
# 创建类实例之后调用


## __call__


## __str__ & __repr__
#  友好打印  https://blog.csdn.net/zj19880814/article/details/12651291


## __iter__ & __next__
# https://blog.csdn.net/liweibin1994/article/details/77374854
# for .. in ..    第一件事是获得一个可迭代器，即调用了__iter__()函数, 第二件事是循环的过程，循环调用__next__()函数

## __sizeof__
# obj.__sizeof__
```

type，metaclass

type是一个生成类对象的类工厂，实际上也是一个类，专门构建类对象的类称为元类，http://blog.jobbole.com/21351/

```

Init signature: type(self, /, *args, **kwargs)
Docstring:     
type(object_or_name, bases, dict)
type(object) -> the object's type
type(name, bases, dict) -> a new type

# 第一示例，传入一个对象的时候，返回得是这个对象的Type类型信息,用于知道一个对象的类型是什么
# 第二个示例，传入的是类对象的信息，生成一个类对象，name为类名，bases为继承的父类，dict为属性字典，用于动态创建类
```



