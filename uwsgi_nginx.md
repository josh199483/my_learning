# ubuntu16.04 + flask + nginx + uwsgi + python3(ubuntu內建的3.5.2)
## 首先安裝python3環境和nginx
```bash
sudo apt-get update
sudo apt-get install python3-pip python3-dev nginx
```
## 安裝套件
```bash
sudo pip3 install virtualenv virtualenvwrapper
# 把以下幾行加到~/.bashrc
export WORKON_HOME=$HOME/.virtualenvs
#export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3.5
# 接著重新載入，讓以下兩個檔案知道我們有做更動
source /usr/local/bin/virtualenvwrapper.sh
source ~/.bashrc
# 若有需要update，pip3 install --upgrade pip即可
mkdir ~/myflask
cd ~/myflask
# 如果找不到該指令，可以pip3 uninstall virtualenv virtualenvwrapper
mkvirtualenv -p /usr/bin/python3 <name_of_env> # 要看當前系統有哪個版本的python
setvirtualenvproject . # 綁定目前資料夾，以後workon env時直接進該資料夾
workon <env_name> # 啟動env

pip install uwsgi flask #進到虛擬環境後再安裝，不需要打pip3
nano ~/myflask/myflask.py
sudo ufw allow 5000
```
## 簡單建立一個flask應用
```python
#~/myflask/myflask.py
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "<h1 style='color:blue'>Hello There!</h1>"

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```
## 建立啟動文件
```python
#~/myflask/wsgi.py
from myproject import app

if __name__ == "__main__":
    app.run()
```
## uwsgi
```bash
uwsgi --socket 0.0.0.0:5000 --protocol=http -w wsgi:app
# protocol使用http協議
# -w 是使用的module，wsgi(wsgi.py)和app(flask instance)
```
但每次啟動都要打一次指令很麻煩...

可在該目錄下建立一個配置檔 myflask.ini
```ini
[uwsgi]
module = wsgi:app
master = true
processes = 5

chdir = /home/ubuntu/myflask
socket = /path/to/sock/myflask.sock
logto = /home/to/log/myflask.log
chmod-socket = 664
vacuum = true

# processes = 5 代表要啟動5個子進程處理請求
# chdir = /home/ubuntu/myflask 切換路徑到wsgi.py的資料夾
# socket = /path/to/sock/myflask.sock 是uwsgi啟動後所需要創建的檔案，這檔案用來和Nginx通訊，會在配置Nginx時用到，所以 chmod-socket = 664是為了修改.sock檔案權限來和Nginx通訊
# logto = /home/to/log/myflask.log 選擇了uwsgi日誌資料夾，uwsgi會將請求log寫入該檔案
```
## nginx 配置
```
#sudo nano /etc/nginx/sites-available/myflask
server {
    listen 80;
    server_name server_domain_or_IP;

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/path/to/sock/myflask.sock;
    }
}
```
```bash
sudo ln -s /etc/nginx/sites-available/myflask /etc/nginx/sites-enabled
sudo nginx -t #檢查nginx是否成功
sudo nginx -s reload #sudo systemctl restart nginx
```
若無法成功，可去/var/log/nginx/error.log看看錯誤原因
若是permission denied代表.sock權限不足，可改為666，甚至懶得想問題直接改777，但風險比較高

[問題1](https://stackoverflow.com/questions/22071681/permission-denied-nginx-and-uwsgi-socket)
[問題2](https://stackoverflow.com/questions/6795350/nginx-403-forbidden-for-all-files)

## 將uwsgi設置為系統服務
當我們啟動uwsgi後，用ctrl-c或關閉ssh連線視窗都會導致uwsgi process關閉，這樣.sock檔案就會消失，訪問網站時，nginx抱錯會顯示502

這時候就需要使用process管理服務啦!ubuntu自帶的systemd是最簡易的方式，可以將我們的程式變為系統服務

```bash
sudo nano /etc/systemd/system/myflask.service #建立myflask.service
#以下全部都要絕對路徑
[Unit]
Description=uWSGI instance to serve myflask
After=network.target

[Service]
User=<username>
Group=www-data
WorkingDirectory=/home/ubuntu/myflask
Environment="PATH=/home/sammy/myflask/flaskenv/bin"
ExecStart=/home/<username>/myflask/flaskenv/bin/uwsgi --ini myflask.ini

[Install]
WantedBy=multi-user.target

#Environment 需要的環境變量，例:指定配置檔案或虛擬環境路徑
#ExecStart 啟動服務的指令
#WantedBy=multi-user.target 會隨著系統啟動而啟動服務
```
存檔之後
```bash
sudo systemctl daemon-reload # 更改服務內容可能會叫做這個重新載入的指令
sudo systemctl start myflask #扣掉.service即可
#sudo systemctl restart myflask
sudo systemctl enable myflask #開機後自動運行
sudo systemctl restart nginx

sudo ufw delete allow 5000 #這時候已經不用透過5000port連入，所以移除這個rule
sudo ufw allow 'Nginx Full' #要透過nginx server連入，增加此rule
```
