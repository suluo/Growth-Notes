简单命令   Shell

```
# 访问远程服务器mysql
$ mysql -h 192.168.1.222 -P 3306 -u user -p password


SELECT column_name,column_name
FROM table_name
WHERE column_name operator value;
```

Python  MySQLdb

[python操作mysql数据库](http://www.runoob.com/python/python-mysql.html)

```
$ pip install mysql-python

import MySQLdb


# # 使用cursor()方法获取操作游标  cursorclass = msd.cursors.DictCursor 加人这个就是获取键值对
cursor = db.cursor(cursorclass = MySQLdb.cursors.DictCursor)

## 普通的cursor中cursor.description有所选列的信息。下面的代码会打印结果中各列的名字
for field_desc in cursor.description:
  print field_desc[0]

try:
    conn=MySQLdb.connect(host='localhost',user='root',passwd='root',db='test',port=3306)
    cur=conn.cursor()
    cur.execute('select * from user')
    cur.close()
    conn.close()
except MySQLdb.Error,e:
     print "Mysql Error %d: %s" % (e.args[0], e.args[1])
```



