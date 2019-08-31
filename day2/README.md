# Serverless 安裝並建立第一個專案

## 建立 AWS 存取金鑰

首先要先有個 AWS 帳號(廢話) ，如果沒有綁定信用卡記得去綁定才能繼續，接下來就可以去就可以去建立鑰匙囉！
![](https://i.imgur.com/Sw2gIsA.png)

如上圖，按下紅色框框的部分，接著按箭頭指的按鈕，就會建立一個我們要的金鑰囉！
![](https://i.imgur.com/4JObdl3.png)

AWS secret & ID key
如上圖所示，可以下載 key，AWS 不會幫忙保存 Secret key，若這視窗關掉的話 key 就不見了，所以使用者可以自行下載檔案保存，以免哪天要部署的時候找不到 key。
接著就將這兩個參數加入環境變數中：

```javascript=
export AWS_ACCESS_KEY_ID=<your-key-here>
export AWS_SECRET_ACCESS_KEY=<your-secret-key-here>
```
