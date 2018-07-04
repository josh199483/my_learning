# websocket
在 HTML5 之前，客户端和伺服器通過 HTTP協議交換數據，但是HTTP 協議具有兩個特點:
* HTTP 協議是一種單向的網路協議。在建立連接後，它只允許客戶端 Browser 向伺服器 WebServer 發送請求後，WebServer 才能返回相應的數據，而 WebServer 不能主動發送數據给 Browser。

* HTTP 協議是無狀態的。客户端向伺服器發送連接請求中會包含 identity info（身分資料），每次當一個連結結束時，伺服器就會將這些身分資料丢掉，客户端再次發送 HTTP 請求時，就需要重新發送這些信息。

這時候如果要做一些real time的應用，就需要client不斷的發送請求給server，反覆的進行http通訊。

比較常用的方法有兩種，ajax polling和long polling，但這兩種方式本質上還是持續地與server端作連接，並等待處理。
## websocket 協議
WebSocket 協議是一種持久化的雙向通訊協議，它建立在TCP之上，和 HTTP 一樣通過 TCP 來傳輸數據。

主要有兩大不同點:

* WebSocket 是一種雙向通訊協議，在建立連接後，WebSocket 伺服器和 Browser 都能主動的向對方發送或接收數據。
* WebSocket 需要通過握手連接，類似於 TCP 它也需要客戶端和伺服器端進行握手連接，連接成功後才能相互通訊。

## websocket工作流程
browser通過 JavaScript 向server發出建立 WebSocket 連接的請求，連接建立以後，客户端和server端就可以通過 TCP 連接直接交換數據。因為 WebSocket 連接本質上就是一個 TCP 連接，所以在數據傳輸的穩定性和數據傳輸量的大小方面，和傳統
polling比較，具有很大的性能優勢。

客戶端首先要向server發送一個http請求(header會附帶一個升級訊息表示是websocket)，server會解析這些訊息並回傳，就代表websocket連接成功了，g汪方就可以自由的傳輸資訊，直到其中一方關閉。

## socket.io
是一個支持 WebSocket 協議的用於即時通訊、跨平台的開源框架，Socket.io 設計的目標是支持任何的browser，但socket.io並不是只支持websocket通訊，如上面介紹的ajax polling、long polling都有支持

## flask socket.io
testing
