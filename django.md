# django
## django 指令
```bash
python manage.py runserver 0.0.0.0:8000 #讓內網每個人都可在8000 port 開啟網頁
python manage.py makemigrations #當把models.py的class都寫完後，執行此命令，會在migrations目錄裡記錄models.py的變化
python manage.py migrate #依照上面指令紀錄的migration檔案，去建立對應的資料庫table
python manage.py inspectdb > models.py #把已經建立好的資料庫直接匯入models.py裡(裡面會有寫好的class)
python manage.py shell #可直接進python的command line，使用django
python manage.py dbshell #進到settings.py裡設定的database，直接做操作
```
