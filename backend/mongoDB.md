# MongoDB
## mongoDB安裝(win10)V3.4.1
下載完mongoDB後，加入環境變數後，開啟CMD
> mongod --dbpath d:\mongodb\data\db(自定義的資料夾，該資料夾會記錄DB的訊息)
再開啟另一個CMD

> mongo 即可使用mongoDB
## 在win10 開啟mongoDB service
在剛剛mongodb的目錄下，create `mongod.cfg`
```cmd
systemLog:
    destination: file
    path: d:\mongodb\data\log\mongod.log
storage:
    dbPath: d:\mongodb\data\db
```
接著以管理員身分打開CMD

mongod --config D:\mongodb\mongod.cfg --install

net start MongoDB (啟動服務)

net stop MongoDB (關閉服務)

若遇到啟動失敗，可試著刪除db/mongod.lock，接著執行

mongod --config D:\mongodb\mongod.cfg --remove

mongod --config D:\mongodb\mongod.cfg --install

## ubuntu 16.04 安裝mongodb3.6
[官方文章，也有其他作業系統](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)
``` bash
# 透過public key，確保package的一致性
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5

# 建立mongodb的列表，這是ubuntu 14.04版的
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list

# 建立mongodb的列表，這是ubuntu 16.04版的
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.6.list

# 更新的本地package
sudo apt-get update
# 如果update結束後出現一些error是說找不到某些package或是有衝突等等，可以先清掉mongodb的repository，然後再從頭做一遍!
sudo rm /etc/apt/sources.list.d/mongodb*.list

# 安裝mongodb stable version
sudo apt-get install -y mongodb-org

# 啟動服務
sudo systemctl start mongod

# 開機後自動啟動
sudo systemctl enable mongod
```

## linux環境使用mongo
```bash
mongod --dbpath ~/mongodb #自定義路徑，儲存data files

mongod --fork --logpath ~/log/mongodb.log #背景執行，並且把log寫入指定log檔

```

# 三種關閉mongod的方式
* use admin #在mongo shell裡      
db.shutdownServer()
* mongod --shutdown
* kill -2 (mongod process ID)

## mongodb權限管理
因mongodb預設安裝好後是沒有保護機制的，需自行建立登入機制保護資料
```bash
# 進mongo shell
mongo
#這樣就有一個root帳號了!
>>> use admin
>>> db.createUser({user:"root",pwd:"PASSWORD",roles:[{role:"root",db:"admin"}]})

#接著創建專屬資料庫的帳號
>>> use test
>>> db.createUser({user:"admin",pwd:"PASSWORD",roles: [{ role: "dbOwner", db: "test" }]}) #db擁有者
>>> db.createUser({user:"admin",pwd:"PASSWORD",roles: [{ role: "readWrite", db: "test" }]}) #擁有讀寫權限
>>> db.createUser({user:"user",pwd:"PASSWORD",roles: [{ role: "read", db: "test" }]}) #擁有讀權限，接著登出
>>> exit or ctrl+c
#這時候在進mongodb就要使用帳號密碼登入
登入遇到
1.about to fork child process, waiting until server is ready for connections，
ERROR: child process failed, exited with error number 100
因為mongodb不正常關閉，刪除DBPATH裡的mongod.lock文件
2.ERROR:  child process failed ,exited with error number 1
增加DBPATH的寫入權限即可

mongod --auth --fork --logpath ~/log/mongodb.log --dbpath ~/mongodb (要加--auth才行)

# 如果要用系統服務，就去更改/etc/mongod.conf組態檔
security:
  authorization: enabled

# 服務重啟一次即可
sudo systemctl restart mongod

# 進mongo shell
mongo

>>> use admin
>>> db.auth("root", "PASSWORD") #以root登入，出現 1 就是成功
>>> use test
>>> db.auth("admin", "PASSWORD") #以admin權限登入test資料庫(讀寫皆可)，出現 1 就是成功
>>> db.auth("user", "PASSWORD") #以user權限登入test資料庫(只能讀)，出現 1 就是成功
# 之後就可以依照不同的權限使用mongodb了
```

## 基本操作
[覺得講的最詳細的網站，可參考](http://marklin-blog.logdown.com/posts/1392582)
show dbs
* Collections List
1. show collections
2. show tables
3. db.getCollectionNames()<br>
use (database name) #沒有該名稱資料庫就創建，若有就切換到該資料庫<br>
* Create Operations<br>
1. db.collection.insertOne() #創建一筆資料<br>
2. db.collection.insertMany() #創建多筆資料(list)
* Read Operations<br>
1. db.collection.find()
* Update Operations<br>
1. db.collection.updateOne()
2. db.collection.updateMany()
3. db.collection.replaceOne()
* Delete Operations
1. db.collection.deleteOne()
2. db.collection.deleteMany()
## 條件篩選
```bash
{<key>:<value>}
{<key>:{$lte:<value>}}
#小於等於
```
```
#當list這個key存在時，只查詢timestamp,list欄位不要id欄位(因為預設是一定會給)，並且限制10筆
db.collection.find({'list':{ $exists: true}},{ timestamp: 1,list:1,_id:0 }).limit(10)
```
```
#把list的第一個element的count給定NumberInt(2)，不然直接給2會是浮點數
db.collection.update({'ID':'1'},{$set:{'list.0.count':NumberInt(2)}})
#把list用unset取消掉該欄位
db.collection.update({'ID':'1'},{$unset:{'list':''}})
```
## mongodb schema 設計
[mongoDB設計模式](https://blog.toright.com/posts/4483/mongodb-schema-%E8%A8%AD%E8%A8%88%E6%8C%87%E5%8D%97.html)
NoSQL並不是代表就不需要schema設計，主要可以分為三大類設計模式

One-to-Few (少量), One-to-Many (多量) 與 One-to-Squillions (海量)，實際例子可去上述網站看

### 1.One-to-Few 少量級關聯模式 (Embedding)
若某個document底下的子document數量不多(大概幾十以下)，就可以使用embedding，這裡要做到的是反正規化來提升查詢效率
### 2.One-to-Many 多量級關聯模式 (Child-Referencing)
若子document數量太多(幾百上下)，那一筆document大小可能就會超過限制的16MB，這時候就必須使用類似RDB的正規化作法，這時候就必須把被關聯的document放在另一個collection，透過Object id做關聯，實際查詢時透過 Application-level Join 進行反查
### 3.One-to-Squillions 海量級關聯模式 (Parent-Referencing)
如果被參照的document有超過幾千筆以上，例如物聯網應用，會遇到用來存放Object id的陣列爆表，這時候就要用反過來進行參照，把主document的id也放進子document的參照id裡

## aggregate聚合
[覺得講的最詳細的網站，可參考](http://marklin-blog.logdown.com/posts/1394100-mongodb-polymerization-of-30-14-1-aggregate-framework-with-buckle)
### $lookup字符
mongoDB在3.2版後aggregate有支援$lookup功能，簡單來說就是join的功能，讓兩個有關連的表能夠依照foreign key來做embedded，廢話不多說直接看官方例子
```bash
orders
{ "_id" : 1, "item" : "almonds", "price" : 12, "quantity" : 2 },
{ "_id" : 2, "item" : "pecans", "price" : 20, "quantity" : 1 },
{ "_id" : 3  }
inventory
{ "_id" : 1, "sku" : "almonds", description: "product 1", "instock" : 120 },
{ "_id" : 2, "sku" : "bread", description: "product 2", "instock" : 80 },
{ "_id" : 3, "sku" : "cashews", description: "product 3", "instock" : 60 },
{ "_id" : 4, "sku" : "pecans", description: "product 4", "instock" : 70 },
{ "_id" : 5, "sku": null, description: "Incomplete" },
{ "_id" : 6 }
# 這邊以orders collection當作主document
db.orders.aggregate([
   {
     $lookup:
       {
         from: "inventory", # from就是要join哪個collection
         localField: "item", # 以自己的哪個field去對照foreign key
         foreignField: "sku", # 對照inventory collection的哪個field
         as: "inventory_docs" # 新取的一個field名字
       }
  }
])
# 結果如下，inventory_docs會變成一個list包含所有參照到foreign key的document
# 因orders {'_id':3}這筆資料沒有'item'視為null，所以對照到inventory的{'_id':5}、{'_id':6}
{
   "_id" : 1,
   "item" : "almonds",
   "price" : 12,
   "quantity" : 2,
   "inventory_docs" : [
      { "_id" : 1, "sku" : "almonds", "description" : "product 1", "instock" : 120 }
   ]
}
{
   "_id" : 2,
   "item" : "pecans",
   "price" : 20,
   "quantity" : 1,
   "inventory_docs" : [
      { "_id" : 4, "sku" : "pecans", "description" : "product 4", "instock" : 70 }
   ]
}
{
   "_id" : 3,
   "inventory_docs" : [
      { "_id" : 5, "sku" : null, "description" : "Incomplete" },
      { "_id" : 6 }
   ]
}
```
## 查看document大小
Object.bsonsize(db.test.findOne({name:"123"})) #可看到該筆紀錄大小

## mongodb的$字號運用!!!懂了後發現好神R!!!
```json
{ "_id" : 1,
  "name" : "wayne",
  "book" : [
    { "bookId" : 23,
      "title" : "23",
      "color" : "red",
    },
    { "bookId" : 41,
      "title" : "41",
      "color" : "blue"
    }
  ]
}
# 若要更新book list裡面當subdocument的bookId為特定值(例:41)的description(原本沒有的欄位)，有點拗口...，來看個範例吧!
# 因為是原本沒有的欄位，所以使用{upsert:true}參數，代表當沒有此欄位時就insert一筆
# $字號在此的用處是update的條件有book.bookId，那後面要更改的欄位加上$字號代表會參照前面相同位置的條件(placeholder)去更改
db.test.updateOne({'name':'wayne','book.bookId':41},{$set:{'book.$.description':'test'}},{upsert:true})
{ "_id" : 1,
  "name" : "wayne",
  "book" : [
    { "bookId" : 23,
      "title" : "23",
      "color" : "red"
    },
    { "bookId" : 41,
      "title" : "41",
      "color" : "blue",
      "description": "test"
    }
  ]
}
```
## mongodb的事務操作
[參考網頁](http://marklin-blog.logdown.com/posts/1394578)

事務操作是指甚麼呢?簡單來說就是一個工作流程，像是做麵包，要先買材料、擀麵糰、加調味料、放進烤箱，就是一個事務操作。

假設我有一個資料庫包含訂單資料、客戶資料，當一個客戶要下訂單時，會有兩階段，1.先在訂單資料新增一筆紀錄，2.再去客戶資料進行扣款，那如果只做到第一階段系統就當掉了，這個事務就會保證要嘛整個事務全部完成，要嘛全部沒完成。

ACID這四個原則可去wiki查看看

但MongoDB並不支援事務操作，只有符合各自特性的操作

1.在單個 document 上有提供原子性操作 findAndModify
```
如上面講的範例，在mongodDB我們就需要做反正規化，讓訂單和客戶資料在同一個collection，才可以符合原子性操作
```
2.對多個 document 使用 $isolate
```
$isolate可以讓我們在更新大量document，其他thread無法讀寫正在更新的document，但就會有幾個問題
1.效能問題，因為等於強制等更新完才能做操作
2.並沒有支援原子性操作
3.不支援分片
```
3.Two Phase Commits 來模擬事務操作
```
mongoDB官網有提供一種自行手動建立事務操作的範例，在進行大量更新時，若發生錯誤，則之前更新的會全部還原
```
## mongoDB索引
[參考網頁](http://marklin-blog.logdown.com/posts/1394035-30-11-index-of-mongodb-1-button)
建立索引就像建立一個目錄一樣

優點:
- 搜尋速度極快
- 使用分組或排序也很快

缺點:
- 進行增、刪、改動作時，會更花費時間，因為要連索引一起更改
- 索引也需要占空間

使用時機:
- 搜尋結果佔原collection越小
- 常用的搜尋
- 該搜尋造成性能瓶頸
- 在經常需要排序的搜尋
- 當索引性能大於增、刪、改性能時

## mongoDB性能調適
```bash
#可以查看該查詢的詳細資料，verbose有三個可選選項queryPlanner,executionStats,allPlansExecution，預設是queryPlanner
cursor.explain(verbose) 
# 我比較常看的是executionStats
# nReturned代表最後回傳的結果數目
# totalKeysExamined代表總共搜尋的索引數目
# totalDocsExamined代表總共搜尋的document數目
"executionStats" : {
   "executionSuccess" : <boolean>,
   "nReturned" : <int>,
   "executionTimeMillis" : <int>,
   "totalKeysExamined" : <int>,
   "totalDocsExamined" : <int>,
   "executionStages" : {
      ...
   },
   "allPlansExecution" : [
      { <partial executionStats1> },
      { <partial executionStats2> },
      ...
   ]
}
# 如果完美使用索引的話三個值會是完全相等，但在現實中不太可能遇到...
# 如果有多個索引，mongo會幫我們挑選最佳索引，但是是以totalDocsExamined最低的為挑選依據
# 所以如果有排序的狀況，mongo沒辦法幫我們挑選最佳索引
# 有時候我們挑選索引不希望用mongo挑選的最佳索引，可以使用hint強迫使用該索引
db.test.find({'name':'john','age':23}).sort({'age':1}).hint({'age':1,'name':1}).explain()

```

## 資料庫備份
[資料參考1](https://blog.toright.com/posts/4069/mongodb-%E6%95%99%E5%AD%B8-%E5%A6%82%E4%BD%95%E5%82%99%E4%BB%BD%E8%88%87%E9%82%84%E5%8E%9F-mongodb.html)

[資料參考2](https://itw01.com/NKDNEYK.html)

下面的指令其實都只用在資料量較小的時候，如果資料量好幾TB，就不適合這種傳統備份檔案的方式，最好是用replication或sharding的方式

```bash
mongodump -h 127.0.0.1 -d my-mongo -o ./mongo-backup
-h: 要備份的 MongoDB 連線位置
-d: 要備份的 Database 名稱
-u: 資料庫使用者名稱
-p: 資料庫密碼
-o: 會在該資料夾產生備份檔，以資料庫名稱作為資料夾分類
```
[備份的細節](https://ithelp.ithome.com.tw/articles/10165673)
mongodump會把所有查詢結果寫入硬碟中，但因為可能有很多document還在記憶體中未寫入硬碟中，或是在執行備份的同時仍可能有很多新增、修改操作也正在執行中，導致備份出來的檔案是不完整的

解法上面網址有寫， 備份前要做fsync和lock指令，讓記憶體的資料同步到硬碟中，還要鎖住這個db，讓寫入的操作都禁止
```bash
use admin
db.runCommand({fsync:1,lock:1})
db.currentOp() # 查看是否真的上鎖，true就是成功了，接著就可以備份資料了
db.fsyncUnlock() # 記得解鎖
```
一個較完整的備份腳本，mongodb_backup.sh
```bash
#!/bin/sh
# dump 命令執行路徑，根據mongodb安裝路徑而定
DUMP=/usr/bin/mongodump
# 臨時備份路徑
OUT_DIR=/home/user/backup/mongodb/tmp_dir
# 壓縮後的備份存放路徑
TAR_DIR=/home/user/backup/mongodb/tar_dir
# 當前系統時間
DATE=`date +%Y-%m-%d_%H:%M:%S`
# 資料庫帳號
DB_USER=admin
# 資料庫密碼
DB_PASS=<password>
# 刪除7天前的備份，即只保留近 7 天的備份
DAYS=7
# 最終儲存的資料庫備份檔案
TAR_BAK="mongodb_$DATE.tar.gz"
cd $OUT_DIR
rm -rf $OUT_DIR/*
mkdir -p $OUT_DIR/$DATE
mongo admin --eval "printjson(db.fsyncLock())"
$DUMP -h 127.0.0.1:27017 -u $DB_USER -p $DB_PASS -d pome -o $OUT_DIR/$DATE
mongo admin --eval "printjson(db.fsyncUnlock())"
# 壓縮格式為 .tar.gz 格式
tar -zcvf $TAR_DIR/$TAR_BAK $OUT_DIR/$DATE
# 刪除 7 天前的備份檔案
find $TAR_DIR/ -mtime +$DAYS -delete
exit
```
```bash
sudo vi /etc/crontab # 或是crontab -e，去增加排程任務
*       */1       *       *       *       root    sh /home/user/mongodb_backup.sh
# 啟動服務，ubuntu16.04 改叫 cron 服務，以前的版本好像是叫 crond
sudo systemctl start cron
sudo systemctl enable cron
```

```bash
mongorestore -h 127.0.0.1 -d my-mongo-new ./mongo-backup/my-mongo
-h: 要還原的 MongoDB 連線位置
-d: 要還原的 Database 名稱
-u: 資料庫使用者名稱
-p: 資料庫密碼
--drop: 如果資料庫存在就刪除重新建立 (小心使用)
<path>: 最後加入要還原的資料庫備份檔案
```


## 實際遇到的狀況，用pymongo實作
```python
# 一般使用日期區間的情形都是，判斷document裡的日期是否介於一段時間
# 類似這樣的解法，很簡單
db.test.find({'datetime':{'$gt':startDate,'$lte':endDate}})

# 但這個狀況是要判斷今天是否介於document裡的startDate和endDate
db.test.insert_many([
{
    name: "aa",
    startDate: datetime.now()-timedelta(days=1),
    endDate: datetime.now()
},
{
    name: "bb",
    startDate: datetime.now()-timedelta(days=1),
    endDate: datetime.now()
}
])

# solution，雖然是不同狀況，但也算簡單，利用$and字符
db.test.find({'$and':[{'startDate':{'$lt':datetime.now()}},{'endDate':{'$gte':datetime.now()}}]},{'_id':0})
```