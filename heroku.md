# heroku
## 註冊
先去heroku官網註冊，註冊完後選擇python語言

接著依據作業系統選擇heroku cli(這邊選擇win64)

## 部屬網站的設定檔
```bash
heroku login #接著輸入email、password
heroku create <app名字> #若沒有重複，可在https://<app名字>.herokuapp.com看到網頁代表成功
#接著要寫幾個檔案讓我們部屬網站時，heroku可依照這些檔案去部屬
```
### requirements.txt
```bash
pip freeze #可看到目前環境所載的套件
pip freeze > requirements.txt #此處要先進到安裝flask的環境下再進行
例:
Flask==0.12.2
gunicorn==19.7.1
itsdangerous==0.24
Jinja2==2.9.6
MarkupSafe==1.0
Werkzeug==0.12.2
```
### runtime.txt
[可到該網站](https://devcenter.heroku.com/articles/python-runtimes)

看現在最新heroku支援的python版本，例:python-3.6.2

把該版本貼進runtime.txt
### Procfile
```bash
#gunicorn 為webserver，script1為py檔名，app為flask建立instance的名字
web: gunicorn script1:app
#有多種部屬方式
#web: gunicorn gettingstarted.wsgi --log-file -
#web: python manage.py runserver 0.0.0.0:5000
```
## 部屬網站
```bash
git init 
git add .
git commit -m "for test"
heroku git:remote --app <app名字> #建立對應remote repository
git push heroku master #把app專案push到heroku 
#確認verifying deploy done 即成功
heroku open #開啟網站
#heroku ps:scale web=1 #scale app
```
## 更多
heroku還可以作更進階的設定和addons

像是一些database的串聯等，都可到官網document查詢