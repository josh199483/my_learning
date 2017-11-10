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

net start MongoDB (關閉服務)

若遇到啟動失敗，可試著刪除db/mongod.lock，接著執行

mongod --config D:\mongodb\mongod.cfg --remove

mongod --config D:\mongodb\mongod.cfg --install

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
因mongodb預設安裝好後是沒有保護機制的，需自行建立登入機制保護資料<br>
```bash
use admin
db.createUser({user:"root",pwd:"PASSWORD",roles:[{role:"root",db:"admin"}]})
#這樣就有一個root帳號了!
```
```bash
接著創建專屬資料庫的帳號
use test
db.createUser({user:"admin",pwd:"PASSWORD",roles: [{ role: "dbOwner", db: "test" }]}) #db擁有者
db.createUser({user:"admin",pwd:"PASSWORD",roles: [{ role: "readWrite", db: "test" }]}) #擁有讀寫權限
db.createUser({user:"user",pwd:"PASSWORD",roles: [{ role: "read", db: "test" }]}) #擁有讀權限，接著登出

#這時候在進mongodb就要使用帳號密碼登入
登入遇到
1.about to fork child process, waiting until server is ready for connections，
ERROR: child process failed, exited with error number 100
因為mongodb不正常關閉，刪除DBPATH裡的mongod.lock文件
2.ERROR:  child process failed ,exited with error number 1
增加DBPATH的寫入權限即可

mongod --auth --fork --logpath ~/log/mongodb.log --dbpath ~/mongodb (要加--auth才行)

use admin
db.auth("root", "PASSWORD") #以root登入
use test
db.auth("admin", "PASSWORD") #以admin權限登入test資料庫(讀寫皆可)
db.auth("user", "PASSWORD") #以user權限登入test資料庫(只能讀)
```

## 基本操作
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
```
{<key>:<value>}
{<key>:{$lte:<value>}}
#小於等於
```
```
db.collection.find({'list':{ $exists: true}},{ timestamp: 1,list:1,_id:0 }).limit(10)
 #當list這個key存在時，只查詢timestamp,list欄位不要id欄位(因為預設是一定會給)，並且限制10筆
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