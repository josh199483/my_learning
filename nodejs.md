## 安裝 express.js
```bash
# 建立資料夾
mkdir myapp<自己取名>
cd myapp<自己取名>
# 為自己的專案建立一個 package.json 紀錄了一些安裝的套件
npm init
# 會問你一些問題要不要做填寫，除了 entry point 以外，都不用修改
entry point : <看自己喜歡的名字 + .js>
# 安裝 express，將 express 儲存在 dependency list
npm install express --save
# 快速建立 application structre
npm install express-generator -g
# 會產生一個 myapp 的資料夾，裡面包含許多內建的架構
cd myapp
# 大概長這樣~~
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.pug
    ├── index.pug
    └── layout.pug
# 安裝 dependency
npm install
# 啟動 server
DEBUG=myapp:* npm start # linux or MacOS
set DEBUG=myapp:* & npm start # windows
# 為了讓 server 只要在檔案有改動時就會重啟...好扯竟然原本不支援，要安裝 nodemon
npm install --save-dev nodemon
# 在 package.json 修改
"scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www"
  }
# 啟動指令改為
SET DEBUG=express-locallibrary-tutorial:* & npm run devstart



```
