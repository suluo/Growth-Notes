#### 简单命令

##### 命令行命令

```
> show dbs
> use db
> show tables      show collections
> db.drop_collection("users") #删除表
> db.dropDataBase() #删除库

### 查看全部collections
> db.collection_names()
### find
> db.sample.find().count()
> db[table].findOne()
> db.sample.find("name": {$exists: false})
> db.sample.find().sort({"age": 1})
### insert
> db.sample.insert()
> db.sample.insertOne()
### update
> db.sample.update({查询}, {修改})
### 批量更新：第四个参数为true
> db.sample.update({}, {}, false, true)

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
> db.sample.distinct(‘name’,{‘ID’:{$lt:20}}) 
##select distinct(name) from linlin where ID<20
> db.sample.group({key:{'name':true},cond:{'name':'foo'},reduce:function(obj,prev){prev.msum+=obj.marks;},initial:{msum:0}})
##select name,sum(marks) from linlin group by name
> db.linlin.find('this.ID<20′,{name:1})  # select name from linlin where ID<20

> db.sample.find().limit(NUMBER).skip(NUMBER) # limit 显示多少条数据，skip偏移量
```

##### pymongo

```py
from pymongo import MongoClient
from pymongo import ASCENDING, DESCENDING

db = MongoClient(host="localhost", port=27017).db
db = MongoClient('mongodb://localhost:27017/').db

### find
db.sample.find().count()
db.sample.find_one({}, {"_id": 0})
db.sample.find({‘ID’:{$in:[25,35,45]}})
db.sample.find({'name': {"$exists": True}})
#### 默认为升序
db.sample.find().sort("username", ASCENDING) #升序
db.sample.find().sort("username", DESCENDING) #降序
db.sample.find().sort([("username": ASCENDING), "Email", DESCENDING]) #多项排序

### insert
db.sample.insert_one({})
db.sample.insert_one({}).inserted_id
db.sample.insert()
db.sample.insert_many([{}, {}])
### update
db.sample.update({}, {})
db.sample.update({"username": "lilei", {"$set":{"Email": "hanmeimei@163.com"}}})

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

```
## skip
DBCursor cursor = coll.find(query);
int rint = random.nextInt(cursor.count());
cursor.skip(rint);
DBObject word = null;
if(cursor.hasNext()){
    word = cursor.next();
    cursor.close();
}
```



