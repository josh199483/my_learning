[參考網頁](https://spacewander.github.io/explore-flask-zh/7-blueprints.html)

一般小型專案不需要考慮太多的專案架構，只需要全部都寫在一個app.py裡就足夠了

但當網頁越來越複雜時，會覺得全部程式碼寫在一起很難維護，可讀性也很低

這時候才需要規畫專案的架構，又可分為兩種functional and divisional

functional structure
```
manage.py
config.py
requirements.txt
instance/
    config.py
myapp/
    __init__.py
    static/
    templates/
        home/
        control_panel/
        admin/
    views/
        __init__.py
        home.py
        control_panel.py
        admin.py
    models.py
```
divisional structure
```
manage.py
config.py
requirements.txt
instance/
    config.py
myapp/
    __init__.py
    admin/
        __init__.py
        views.py
        static/
        templates/
    home/
        __init__.py
        views.py
        static/
        templates/
    control_panel/
        __init__.py
        views.py
        static/
        templates/
    models.py
```    

