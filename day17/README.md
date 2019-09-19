# AWS IoT 介紹

## iot:Connect

代表連接至 AWS IoT 訊息中介裝置的許可。中介裝置每次接收 iot:Connect 要求，就會檢查 **CONNECT** 許可。==訊息中介裝置**不允許**兩個相同 ID 的用戶端同時在線==。第二個用戶端連線後，中介裝置會偵測到本情況並中斷其中一個用戶端的連線。可使用 iot:Connect 許可，以確保僅已授權的用戶端可使用特定用戶端 ID 來連線。

## iot:Publish

代表於 MQTT 主題的發佈許可。中介裝置每次接收 PUBLISH 要求，就會檢查本許可。用來允許用戶端==發佈訊息==至特定主題。

_注意_

> 您必須授予 iot:Connect 的許可，方能授予 iot:Publish 的許可。

## iot:Receive

代表自 AWS IoT 接收訊息的許可。訊息每次傳送至用戶端，就會檢查 iot:Receive 許可。由於每此交付都會檢查本許可，因此可用來==撤銷==目前訂閱某主題的用戶端的許可。

## iot:Subscribe

代表主題篩選條件的訂閱許可。中介裝置每次接收 SUBSCRIBE 要求，就會檢查本許可。用來允許用戶端訂閱符合之主題。

注意

> 您必須授予 iot:Connect 的許可，方能授予 iot:Subscribe 的許可。

# Set Policy

1. **Connect** -> 所有人皆可連
2. **Publish** -> 只能傳到 mytopic 上 (topic/ 之後的就是 MQTT 需要 pub & sub's topics)
   ![add policy](https://i.imgur.com/eMwCWXD.png)

```
arn:aws:iot:us-east-2:658765043707:topic/aaa
服務        :地區     : Policy ID  :     /需被訂閱之主題
```

## Policy normal example:

```json=
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Publish",
      "Resource": "arn:aws:iot:us-east-2:658765043707:topic/aaa"
    }
  ]
}
```

## Policy wildcard example

```json=
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iot:Receive",
      "Resource": "arn:aws:iot:us-east-2:658765043707:topic/aaa/*/sss"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Connect",
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Subscribe",
      "Resource": "arn:aws:iot:us-east-2:658765043707:topicfilter/aaa/*/sss"
    },
    {
      "Effect": "Allow",
      "Action": "iot:Publish",
      "Resource": "arn:aws:iot:us-east-2:658765043707:topic/aaa/*/sss"
    }
  ]
}
```

# Policy attach Thing

```json=
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["iot:Connect"],
      "Resource": ["*"],
      "Condition": {
        "Bool": {
          "iot:Connection.Thing.IsAttached": ["true"]
        }
      }
    }
  ]
}
```

# Test Case

| Connect | Publish  | Subscribe | Receive  | Result |          Note           |
| :-----: | :------: | :-------: | :------: | :----: | :---------------------: |
|   \*    |    \*    |    \*     |    \*    |  :o:   |        權限全開         |
|   \*    | a/ \* /z | a/ \* /z  | a/ \* /z |  :o:   |                         |
|   aaa   | a/ \* /z | a/ \* /z  | a/ \* /z |  :o:   | aaa 這個 Client ID 才能 |
|   \*    | a/ \* /z | a/ \* /z  | a/ \* /x |  :x:   |     替換掉 reveive      |
|   \*    | a/ \* /z | a/ \* /x  | a/ \* /x |  :x:   |  令 reveive & Sub 一樣  |
