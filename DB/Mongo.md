#### 简单命令

##### 命令行命令

```
### 查看全部collections
> db.collection_names()
### find
> db.sample.find().count()
> db[table].findOne()
### insert
> db.sample.insert()
> db.sample.insertOne()
### update
> db.sample.update({查询}, {修改})
### 批量更新：第四个参数为true
> db.sample.update({}, {}, false, true)

### remove
> db.sample.remove({})
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
>>> posts.find({"date": {"$lt": d}}).sort("author").explain()["cursor"]
u'BtreeCursor date_-1_author_1'
>>> posts.find({"date": {"$lt": d}}).sort("author").explain()["nscanned"]

for item in db.sample.find():
    print item
```



