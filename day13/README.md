# 試玩 python SDK

get_profile 回傳自己的個人狀態

```
profile = line_bot_api.get_profile(
                event['events'][0]['source']['userId'])
state = f"Hello 👉 `{profile.display_name}` 👈"
line_bot_api.reply_message(token, [
  TextSendMessage(text=state),
  ImageSendMessage(original_content_url=profile.picture_url, preview_image_url=profile.picture_url)
  ])
```
