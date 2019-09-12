# 在 serverless 裡使用環境變數讓這些 key 好管理

## 前言

看著前幾天的文章，像我們 notify 驗證的 API 有使用到 `REDIRECT_URI`、`CLIENT_ID` 以及 `CLIENT_SECRET`
只是說若今天當程式碼變多的時候，抑或是這個參數有給其他 API 使用，那在尋找的時候不僅費工又浪費時間
那接下來就帶著大家在 serverless.yml 一步步加入變數值，並更改 code。

## 動手吧！

首先看到裡面會有像是這樣的階層

```
provider:
  name: aws
  runtime: python3.7
  region: us-east-2
```

在 name 的同一層加上 `environment`，並在子階層再新增我們 Notify 有用到的參數

```
provider:
  name: aws
  runtime: python3.7
  region: us-east-2
  environment:
    NOTIFY_REDIRECT_URI: 'your notify redirect_uri'
    NOTIFY_CLIENT_ID: 'client id'
    NOTIFY_CLIENT_SECRET: 'client secret'
```

然後在 notify_controller.py 引入 `os` 的套件，並搭配`os.environ`來取參數，如下範例

```
client = {
  'grant_type': 'authorization_code', 'code': code,
  'redirect_uri': os.environ["NOTIFY_REDIRECT_URI"],
  'client_id': os.environ["NOTIFY_CLIENT_ID"],
  'client_secret': os.environ["NOTIFY_CLIENT_SECRET"]
}
```

## 後記

其實我自己在實作的時候也覺得自己一直手動填超麻煩，以前都習慣有個`.env`來幫我管理這些，當然 Serverless 也有 python 的 [dot env 套件](https://serverless.com/plugins/serverless-dotenv-plugin/)，只是說主要還是都在使用 serverless，所以這次就帶大家在 serverless.yml 下設定囉！

## 參考

[os.environ](https://stackoverflow.com/questions/4906977/how-to-access-environment-variable-values)
[env setting](https://serverless.com/framework/docs/providers/aws/guide/variables/)
