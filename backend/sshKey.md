# convert key format
因為之前在 windows 環境下使用 mobaxterm 這套工具，來協助使用 ssh client 等功能

但後來改到 mac 環境下，找了一套叫 zoc 的工具，但卻發現他不支援 ppk 檔，所以這邊用 puttyGen 來轉換成 pem 檔(openSSH)
[putty generate key](https://www.puttygen.com/)

```bash
sudo brew install putty
# –cp /opt/local/bin/putty ~/Desktop/PuTTY
puttygen privatekey.ppk -O private-openssh -o privatekey.pem
```