简单命令   Shell

```
# 访问远程服务器mysql
$ mysql -h 192.168.1.222 -P 3306 -u user -p password
```

Python  MySQLdb

[python操作mysql数据库](http://www.runoob.com/python/python-mysql.html)

```
$ pip install mysql-python

import MySQLdb
# 数据库连接
## 打开数据库连接
db = MySQLdb.connect("localhost","testuser","test123","TESTDB" )
## 使用cursor()方法获取操作游标 
cursor = db.cursor()
## 使用execute方法执行SQL语句
cursor.execute("SELECT VERSION()")
## 使用 fetchone() 方法获取一条数据
data = cursor.fetchone()
print "Database version : %s " % data
## 关闭数据库连接
db.close()
```



