## 安裝python虛擬環境(windows)
```bash
pip install virtualenv
pip install virtualenvwrapper-win
mkvirtualenv flask #建立虛擬環境
mkdir my_flask
cd my_flask
setprojectdir . #配置工作目錄，以後只要進虛擬環境，會自動進到這個目錄
deactivate #跳出環境
workon flask #進入環境
```
## 安裝flask
```
pip install flask
nano app.py
```
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