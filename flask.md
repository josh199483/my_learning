## 安裝flask
pip install flask<br>
nano app.py
```python
from flask import Flask,render_template

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

@app.route('/hello/<name>')
def helloname(name):
    return render_template('hello.html', name=name)
if __name__ == "__main__":
    app.run(host='0.0.0.0', debug=True)
```
mkdir templates<br>
cd templates<br>
nano hello.html
```html
<html>
  <head>
  <link rel="stylesheet" href='/static/style.css' />
  </head>

  <body>
  <h1>Hello {{ name }}!!</h1>
  </body>
</html>
```
mkdir static<br>
cd static<br>
nano style.css
```css
body {
      background: red;
      color: yellow;
  }

```
接著python app.py即可在5000port看到該網頁
## 模板繼承
先定義一個基礎模板base.html，使用時機是有多個網頁需要大部分相同內容時，有提供4個block供使用，head、title、body、footer
```html
<!doctype html>
<html>
  <head>
    {% block head %}
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    <title>{% block title %}{% endblock %} - My Webpage</title>
    {% endblock %}
  </head>
<body>
  <div id="content">{% block content %}{% endblock %}</div>
  <div id="footer">
    {% block footer %}
    &copy; Copyright 2010 by <a href="http://domain.invalid/">you</a>.
    {% endblock %}
  </div>
</body>
</html>
```
```html
//layout.html
{% extends "layout.html" %}
<html>
  <head>
{% block title %}Index{% endblock %}

{% block head %}
  {{ super() }}
  <style type="text/css">
    .important { color: #336699; }
  </style>
{% endblock %}
  </head>
  
  <body>
{% block content %}
  <h1>Index</h1>
  <p class="important">
    Welcome on my awesome homepage.
{% endblock %}
  </body>
</html>
```
