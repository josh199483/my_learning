# MongoDB
## mongoDB安裝(win10)
下載完mongoDB後，加入環境變數後，開啟CMD
>>mongod --dbpath d:\mongodb\data\db(自定義的資料夾，該資料夾會記錄DB的訊息)
再開啟另一個CMD
>>mongo 即可使用mongoDB
## 在win10 開啟mongoDB service
在剛剛mongodb的目錄下，create mongod.cfg
systemLog:<br>
    destination: file<br>
    path: d:\mongodb\data\log\mongod.log<br>
storage:<br>
    dbPath: d:\mongodb\data\db<br>
接著以管理員身分打開CMD<br>
mongod --config D:\mongodb\mongod.cfg --install<br>
net start MongoDB (啟動服務)<br>
net start MongoDB (關閉服務)<br>
若遇到啟動失敗，可試著刪除db/mongod.lock，接著執行<br>
mongod --config D:\mongodb\mongod.cfg --remove<br>
mongod --config D:\mongodb\mongod.cfg --install<br>
## 基本操作
