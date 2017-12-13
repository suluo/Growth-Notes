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
db = MongoClient(host="localhost", port=27017).db
db = MongoClient('mongodb://localhost:27017/').db

### find
db.sample.find().count()
db.sample.find_one({}, {"_id": 0})
### insert
db.sample.insert_one({})
db.sample.insert_one({}).inserted_id
db.sample.insert()
db.sample.insert_many([{}, {}])

###
db.collection_names

### 建立索引
from pymongo import ASCENDING, DESCENDING
>>> posts.create_index([("date", DESCENDING), ("author", ASCENDING)])
u'date_-1_author_1'
>>> posts.find({"date": {"$lt": d}}).sort("author").explain()["cursor"]
u'BtreeCursor date_-1_author_1'
>>> posts.find({"date": {"$lt": d}}).sort("author").explain()["nscanned"]

```





