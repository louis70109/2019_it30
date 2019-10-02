# 最常用的搭配 Login + Message API to save User profile

## 前言

在第 Day 13 Python SDK 試玩的時候使用過 get_profile，當時使用時就是在每次訊息來的時候才去 SQL 儲存使用者資訊，若只是單純想要使用者資訊的話這樣用好像有點多餘 😓

## 實作

我們要將上篇的前端頁面(line_login_auth.html)改造一下

```html
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>LINE Login</title>
  </head>
  <body></body>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.js"></script>
  <script>
    $.ajax({
      url:
        "https://fu2eps8jsj.execute-api.us-east-1.amazonaws.com/dev/line/auth",
      method: "POST",
      success: function(data) {
        window.location.replace(data.result);
      }
    });
  </script>
</html>
```

這邊我將 button 的 onclick 事件以及 button 本身移除掉，目的是要讓使用者一到這個頁面之後就直接透過 AJAX call api

到了這裡應該有感應到什麼了吧！接著就是把這個 html 檔的 S3 路徑網址使用 QR code 包起來，讓他看起來像是 LINE bot 的 QR code

到了這邊基本上已經快完成了，參考 LINE 的[好友說明](https://developers.line.biz/en/docs/messaging-api/using-line-url-scheme/?fbclid=IwAR25G73QmKIGm1l7kPNFzDIpMwBKQRQVxPP8ZpPzp3p-FSES2fnWcVCue_c)或下圖
![](https://i.imgur.com/fEEzOsy.png)

接著到機器人的開發者頁面，並且往下滑會看到自己機器人的 QR code 以及 Bot 的 ID，把他複製下來並製作機器人的加好友網址
![](https://i.imgur.com/Mn3RG26.png)

像是這樣子

```
https://line.me/R/@1234
```

接著到 `controller/line_login_controller.py`的`def get(self)`最後一行，原本我們這邊做的是回傳 JSON

```python
if dt:
			return {'result': dt}, 200
```
