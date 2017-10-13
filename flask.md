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