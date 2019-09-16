# Message API 應聲蟲實作

新增兩個 LINE Channel key 至 `.env`

```
LINE_CHANNEL_TOKEN=
LINE_CHANNEL_SECRET_KEY=
```

接著加入 LINE 的 python SDK 至 requirements.txt

```
line-bot-sdk==1.12.1
```

透過 pip 安裝一下

```
pip install -r requirements.txt --user
```

在 controller/ 底下新增一個 message_api_controller.py 並輸入以下程式

```
from flask import Flask, request, abort
from flask_restful import Resource
import json
import os
from linebot import (
    LineBotApi, WebhookHandler
)
from linebot.exceptions import (
    InvalidSignatureError
)
from linebot.models import (
    MessageEvent, TextMessage, TextSendMessage,
)


class LineMessageApiWebhookController(Resource):
    def post(self):
        line_bot_api = LineBotApi(os.getenv('LINE_CHANNEL_TOKEN'))
        handler = WebhookHandler(os.getenv('LINE_CHANNEL_SECRET_KEY'))

        # get X-Line-Signature header value
        signature = request.headers['X-Line-Signature']

        body = request.get_data(as_text=True)
        event = json.loads(body)
        print(event)
        try:
            handler.handle(body, signature)
        except InvalidSignatureError:
            print(
                "Invalid signature. Please check your channel access token/channel secret.")
            abort(400)

        token = event['events'][0]['replyToken']
        if token == "00000000000000000000000000000000":
            pass
        else:
            line_bot_api.reply_message(token, TextSendMessage(
                text=event['events'][0]['message']['text']))
        return 'OK'
```

接著在 api.py 加入下面兩段 code，新增一個`webhook`的路由

```
from controller.message_api_controller import LineMessageApiWebhookController
api.add_resource(LineMessageApiWebhookController, '/webhook')
```

這邊可以使用`sls wsgi serve` + `ngrok`去搭配做測試

接著就部署啦

```
sls deploy
```

把網址貼到 `webhook URL` 上並在尾端加上 `/webhook`
![](https://i.imgur.com/uoiU96f.png)

測試結果如下
![](https://i.imgur.com/9wPNuma.png)

## 結論

這次整合了以前寫的 [Repo](https://github.com/louis70109/aws-line-wsgi-python)，加入了`.env`讓我在抓下來使用時也更方便，也整合成 Restful 格式讓之後有需要的人在看 code 時可以更容易理解了～

專案也會持續更新，更多詳情可以 follow 我的專案 [aws-python-line-api](https://github.com/louis70109/aws-python-line-api)。
