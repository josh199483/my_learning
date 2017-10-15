# MQTT
## 1.認識MQTT
### 比較HTTP和MQTT通訊協定
MQTT和HTTP的底層都是TCP/IP(傳輸層/網路層)，也就是物聯網裝置可以沿用既有的網路架構和設備，只是在網路上流通的「訊息格式」以及應用程式的處理機制不同

## 2.win10安裝mosquitto(也就是mqtt broker)
Eclipse基金會的開源物聯網專案計畫（iot.eclipse.org）中的一環，支援MQTT 3.1和3.1.1版通訊協定<br>
可直接從Mosquitto官網下載（mosquitto.org/download)，請選擇“Native build”（原生編譯版）(win32)<br>
開啟cmd輸入 netstat -an|find "1883" 檢視所有連線，因mosquitto預設是1883 port，有如下結果代表成功運行
```
TCP    0.0.0.0:1883           0.0.0.0:0              LISTENING
TCP    [::]:1883              [::]:0                 LISTENING
```
### 開通防火牆埠號1883
windows的防火牆預設沒有開通1883埠號，因此本機電腦以外的MQTT裝置無法和Mosquitto伺服器連線<br>
因此進入控制台=>系統及安全性=>windows防火牆=>進階設定<br>
選擇新增輸入規則，選擇連接埠=>TCP=>特定連接連接埠(1883)=>允許連線=>三個都選=>取名即可
### mqtt基本操作
```python
import paho.mqtt.client as mqtt  #install paho-mqtt
# 當與broker連接時會呼叫的callback
def on_connect(pahoClient, userdata, flags, rc):  
    print("Connected with result code "+str(rc))
# 當接受來自broker的訊息時
def on_message(client, userdata, msg):
    print(msg.topic+" "+str(msg.payload))

broker_address = "localhost"  #指定安裝mosquitto server的機器
client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message
try:
    client.connect(broker_address)    #連線mqtt broker
except ConnectionRefusedError as e:
    print("connection failed "+str(e))

client.loop_start()
while True: #client.connected_flag: #wait in loop
    print("in Main Loop")
    ran = random.randint(0, 99)
    client.publish("MYTOPIC",ran)    #發布訊息
    time.sleep(1)

client.loop_stop()
```
### network loop
這些函數是client 背後的驅動力量。如果沒有被調用，進入網路的數據將不會被處理，走出網路的數據也不會被及時的發送

## 3.RPI3 install mosquitto MQTT broker
```bash
apt-get install mosquitto mosquitto-clients #安裝完成後服務會自動啟動
service mosquitto status #檢查mosquitto服務的狀態，若是綠色的active代表成功
mosquitto_sub -t first/test #訂閱這個主題
mosquitto_pub -t first/test -m "Hello, world!" #發布訊息到這個主題
```
### 訂閱多個主題
sensors/temperature/+ #可取得temparature階層後的所有主題(只有一層)
sensors/# #可取得sensors階層後的所有主題(不管有幾層)
### 使用者認證
預設的 Mosquitto 服務是允許匿名登入使用的，也就是說任何人都可以透過 MQTT 的協定傳送與接收資料，若是在封閉的網路環境之下是沒有問題，但若是開放性的網路，最好就要上一些安全機制<br>
最基本的安全機制就是設定登入的帳號密碼，只有經過認證的使用者可以透過 Mosquitto 服務傳送或是接收資料<br>
1. mosquitto_passwd -c /etc/mosquitto/passwd chen #建立chen使用者，接著輸入密碼
2. /etc/mosquitto/mosquitto.conf #編輯這個檔案
```
# 設定帳號密碼檔案
password_file /etc/mosquitto/passwd
# 禁止匿名登入
allow_anonymous false
```
3. service mosquitto restart #重啟服務
4. mosquitto_sub -t first/test -u chen -P PASSWD #以設定的帳號密碼訂閱主題
5. mosquitto_pub -t first/test -u chen -P PASSWD -m "Hello, world!" #以設定的帳號密碼發布訊息
### 安全加密TLS 連線
```
mkdir myCA     #建立一個只有自己可讀取的資料夾
chmod 700 myCA
cd myCA
wget https://github.com/owntracks/tools/raw/master/TLS/generate-CA.sh
bash generate-CA.sh  #產生金鑰方式可參考mosquitto-tls，此處直接使用別人寫好的script
sudo cp ca.crt /etc/mosquitto/ca_certificates/       #將金鑰複製到mosquitto資料夾下
sudo cp raspberrypi.crt raspberrypi.key /etc/mosquitto/certs/
service mosquitto restart  #重啟服務
sudo chown mosquitto /etc/mosquitto/certs/raspberrypi.key   #有權限問題時，改變擁有者
mosquitto_sub -t '$SYS/broker/bytes/#' \
  -u chen -P PASSWD \
  --cafile ca.crt -v                       #有收到訊息表示正常
mosquitto_sub -t first/test -u chen -P PASSWD --cafile ca.crt    #使用密碼配合TLS加密來訂閱主題
mosquitto_pub -t first/test -u chen -P PASSWD -m "Hello, world!" --cafile ca.crt  #使用密碼配合TLS加密發布訊息
```



