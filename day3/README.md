LINE Notify 顧名思義就是通知屬性的服務，這個服務不是 LINE 的 Message API 哦，很多朋友都會把這兩個搞在一起，

> 今年中有帶著朝陽的學弟妹手把手實作 LINE Notify，[參考](https://www.slideshare.net/JiaYuLin6/step-by-step-to-use-line-notify-20190527)

當然首先還是先從官網抓的流程圖來解釋一番
![https://ithelp.ithome.com.tw/upload/images/20190903/20111481CBjMBQhTr9.png](https://ithelp.ithome.com.tw/upload/images/20190903/20111481CBjMBQhTr9.png)

- 當使用者拜訪你的網站時，會導向 LINE 請求認證
- 認證過了之後會回傳一個名為 code 的參數
- 接著網站需要持這個 code 在去找 LINE 討東西
- 討成功後就會拿到一個 access_token
- 網站就會知道這個 access_token = 來註冊的使用者
- 然後就可以透過 access_token 發送通知給使用者了?

> 若還不清楚可以參考 google OAuth2.0 的機制，看完或許就通了哦！

# 事前通知

首先就是要先加入他好友，如果之前有不小心**封鎖**的話要記得解除封鎖哦，不然後續會收不到消息。
![https://ithelp.ithome.com.tw/upload/images/20190903/20111481Zno98NSHwL.png](https://ithelp.ithome.com.tw/upload/images/20190903/20111481Zno98NSHwL.png)

### 另外這個服務是沒有辦法儲存訊息的哦

這是我平常用的，雖然我是用在 heroku 讓他排程通知我，不過這系列就是要讓我們的 Lambda 幫我們送出去給他
![https://ithelp.ithome.com.tw/upload/images/20190903/20111481APC7DQe8Bx.png](https://ithelp.ithome.com.tw/upload/images/20190903/20111481APC7DQe8Bx.png)

如果參考我的簡報實作的話可以只能拿到自己的 access_toekn
下一篇會帶著時做出簡單的 index.html + 使用 Serverless 蓋我們第一個 API 來做認證，

### jQuery -> AJAX

# 參考

[LINE Notify](https://notify-bot.line.me/zh_TW/)
