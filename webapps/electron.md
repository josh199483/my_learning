# 常見問題
## 1.image error
```
首先建議的圖片是 jpg or png 格式，ico 格式 linux 不支援，

在用 electron packager 遇到的問題是，打包好之後執行會跳出一個 error，如下

Uncaught Exception: TypeError: Error processing argument at index 0, conversion failure from <圖片path>
```
```js
// 使用 electron nativeImage
const path = require('path')
const nativeImage = require('electron')

const iconPath = path.join(__dirname, 'test.png')
let trayIcon = nativeImage.createFromPath(iconPath);
trayIcon = trayIcon.resize({ width: 16, height: 16 });
appIcon = new Tray(trayIcon)
// 接著再對 appIcon 做設定
```

## 2.node gyp error
```
npm install --global --production windows-build-tools
```
