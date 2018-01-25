#### 简单命令

##### 命令行命令

[Mongo查询语法](https://www.cnblogs.com/think_fish/p/3422307.html)

```
## 连接远程数据库   mongo 远程IP地址/端口号 -u 用户名 -p 密码
$ mongo 192.168.1.200:27017/database -u user -p password

> show dbs
> use db
> show tables      show collections
> db.dropDatabase() #删除库
> db.sample.drop() #删除表sample

### 查看全部collections
> db.collection_names()

### find 
### $in & $nin; $or & $and; $mod & $not; $ne $exists
> db.sample.find({age:{$mod:[11,0]}}) # 将查询的值除以第一个给定的值，若余数等于第二个给定的值，则返回该结果
> db.sample.find({age:{$not:[11,0]}})
> db.sample.find().count()
> db[table].findOne()
> db.sample.find("name": {$exists: false})
> db.sample.find().sort({"age": 1})
> db.foo.find({$or:[{a: 1},{b: 2}]})
> db.sample.find({$where: "this."})

### insert
> db.sample.insert()
> db.sample.insertOne()
### update
> db.sample.update({查询}, {修改})
> db.sample.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
### 批量更新：第四个参数为true ## 如果你要修改多条相同的文档，则需要设置 multi 参数为 true
> db.sample.update({}, {}, false, true)
> db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})

### remove
> db.sample.remove({})

### index
> db.collection.creatIndex("")
> db.collection.dropIndex("")


## 一些特殊用法
> db.copyDatabase(fromdb,todb,fromhost) 
##复制数据库fromdb—源数据库名称，todb—目标数据库名称，fromhost—源数据库服务器地址
> db.sample.renameCollection(newName) #重命名些数据集名称
> db.sample.stats() #返回此数据集的状态
> db.sample.totalSize() #返回些数据集的总大小
> db.sample.distinct('name',{‘ID’:{$lt:20}}) 
> db.sample.find({"ID": {$lt: 20}}).distinct('name')
##select distinct(name) from linlin where ID<20
> db.sample.group({key:{'name':true},cond:{'name':'foo'},reduce:function(obj,prev){prev.msum+=obj.marks;},initial:{msum:0}})
##select name,sum(marks) from linlin group by name
> db.linlin.find('this.ID<20′,{name:1})  # select name from linlin where ID<20

> db.sample.find().limit(NUMBER).skip(NUMBER).batchSize(1000) # limit 显示多少条数据，skip偏移量, 
> db.sample.find().limit(NUMBER).batchSize(1000)  ##batchSize 一次取多少个数据
```

##### pymongo

```py
from pymongo import MongoClient
from pymongo import ASCENDING, DESCENDING

db = MongoClient(host="localhost", port=27017).db
db = MongoClient('mongodb://localhost:27017/').db

db.drop_collections("sample")
db.sample.drop()

## find
db.sample.find().count()
db.sample.find_one({}, {"_id": 0}) # 即使加上了列筛选，_id也会返回；必须显式的阻止_id返回
db.sample.find({‘ID’:{$in:[25,35,45]}})
db.sample.find({'name': {"$exists": True}})
#### 默认为升序
db.sample.find().sort("username", ASCENDING) #升序
db.sample.find().sort("username", DESCENDING) #降序
db.sample.find().sort([("username": ASCENDING), "Email", DESCENDING]) #多项排序

db.COLLECTION_NAME.find().sort([(KEY, 1)])

### 
db.users.find({"name" : {"$ne" : "joe"}}) # select * from users where username <> "joe"  
db.users.find({"price" : {"$in" : [725, 542, 390]}}) # select * from users where ticket_no in (725, 542, 390)  
db.users.find({"price" : {"$nin" : [725, 542, 390]}}) 
db.users.find({"$or" : [{"price" : 725}, {"winner" : true}]}) # select * form users where ticket_no = 725 or winner = true  
db.users.find({"id_num" : {"$mod" : [5, 1]}}) # select * from users where (id_num mod 5) = 1  
db.users.find({"$not": {"age" : 27}}) # select * from users where not (age = 27)  
db.users.find({"username" : {"$in" : [null], "$exists" : true}}) # select * from users where username is null // 如果直接通过find({"username" : null})进行查询，那么连带"没有username"的纪录一并筛选出来  
db.users.find({"name" : /joey?/i}) # 正则查询，value是符合PCRE的表达式  

db.people.find({"name.first" : "Joe", "name.last" : "Schmoe"})  # 嵌套查询  
#### 数组查询
db.food.find({fruit : {$all : ["apple", "banana"]}}) # 对数组的查询, 字段fruit中，既包含"apple",又包含"banana"的纪录  
db.food.find({"fruit.2" : "peach"}) # 对数组的查询, 字段fruit中，第3个(从0开始)元素是peach的纪录  
db.food.find({"fruit" : {"$size" : 3}}) # 对数组的查询, 查询数组元素个数是3的记录，$size前面无法和其他的操作符复合使用  
db.users.findOne(criteria, {"comments" : {"$slice" : 10}}) # 对数组的查询，只返回数组comments中的前十条，还可以{"$slice" : -10}， {"$slice" : [23, 10]}; 分别返回最后10条，和中间10条  
#### $elemMatch #嵌套查询，仅当嵌套的元素是数组时使用   
db.blog.find({"comments" : {"$elemMatch" : {"author" : "joe", "score" : {"$gte" : 5}}}}) # 嵌套查询，仅当嵌套的元素是数组时使用,  
db.foo.find({"$where" : "this.x + this.y == 10"}) # 复杂的查询，$where当然是非常方便的，但效率低下。对于复杂查询，考虑的顺序应当是 正则 -> MapReduce -> $where  
db.foo.find({"$where" : "function() { return this.x + this.y == 10; }"}) # $where可以支持javascript函数作为查询条件  
db.foo.find({"$where": "this.fields1 == this.fields2"}).limit(10)
db.foo.find().sort({"x" : 1}).limit(1).skip(10); # 返回第(10, 11]条，按"x"进行排序; 三个limit的顺序是任意的，应该尽量避免skip中使用large-number  
db.foo.find().limit(10000).batch_size(1000) # 一次取1000个数据

### insert
db.sample.insert_one({})
db.sample.insert_one({}).inserted_id
db.sample.insert()
db.sample.insert_many([{}, {}])
### update
db.sample.update({}, {})
db.sample.update({"username": "lilei", {"$set":{"Email": "hanmeimei@163.com"}}})
## 如果你要修改多条相同的文档，则需要设置 multi 参数为 true
db.col.update({'title':'MongoDB 教程'},{"$set":{'title':'MongoDB'}},{multi:true})

###
db.collection_names()

### 建立索引
posts = db.posts
>>> posts.create_index([("date", DESCENDING), ("author", ASCENDING)])
u'date_-1_author_1'
>>> posts.create_index([("mike", 1), ("eliot", 1)])
##### 检查是否存在些索引，如果不存在则创建，存在返回None, 参数同creat_index
>>> posts.ensure_index("mike")

>>> posts.find({"date": {"$lt": d}}).sort("author").explain()["cursor"]
u'BtreeCursor date_-1_author_1'
>>> posts.find({"date": {"$lt": d}}).sort("author").explain()["nscanned"]

### 过期时间
db.sample.ensure_index({"createdAt": 1},{expireAfterSeconds: 300})

for item in db.sample.find():
    print item
```

#### 随机记录

[获取随机记录三种方法](https://www.cnblogs.com/tr0217/p/4731486.html)

```
## skip
import random
cursor = db.sample.find(query);
rint = random.randint(cursor.count());
cursor.skip(rint).limit(0);
if(cursor.hasNext()){
    word = cursor.next();
    cursor.close();
}
# 添加一个random数值字段
var random=Math.random();
var result=db.user.findOne({"random":{"$lt":random}});
if(result==null){
    result=db.user.findOne({"random":{"$gte":random}});
}
# 增加一个random空间位置字段
db.coll.ensureIndex({  random: '2d' })
result = db.coll.findOne({ random: { $near: [Math.random(), 0] } })
```

mongo.conf

```
systemLog:
    destination: file
    #日志存储位置
    path: /opt/mongodb/data/log/mongodb.log
    logAppend: true

storage:
    #journal配置
    journal:
        enabled: true

    #数据文件存储位置
    dbPath: /opt/mongodb/data/db/
    #是否一个库一个文件夹
    directoryPerDB: true
    #数据引擎
    engine: wiredTiger
    #WT引擎配置
    wiredTiger:
        engineConfig:
            #WT最大使用cache（根据服务器实际情况调节）
            cacheSizeGB: 1
            #是否将索引也按数据库名单独存储
            directoryForIndexes: true
        #表压缩配置
        collectionConfig:
            blockCompressor: zlib
        #索引配置
        indexConfig:
            prefixCompression: true

#端口配置
net:
    #绑定之后只允许本机连接
    #bindIp: 127.0.0.1
    port: 27017

processManagement:
    fork: true
```





