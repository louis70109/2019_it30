# Serverless 安裝並建立第一個專案

# 基礎設施

透過 npm 全域安裝 serverless 指令

```bash=
npm install serverless -g
```

> 因為我是用 python3 在 AWS 上實作

- `--template`可以選擇你想要的模板 ([參考](https://serverless.com/framework/docs/providers/aws/cli-reference/create/))
- `name` service 的名稱
- `--path` 專案路徑，若找不到資料夾時會自動新增

```bash=
serverless create --template aws-python3 --name aws-wsgi-flask --path aws-wsgi-flask
```

![](https://i.imgur.com/6GZVQNo.png)

接著後面會需要使用套件，而 python 這邊會認得 requirements.txt 這個檔案，因此我們需要用 serverless 的 plugin 幫我們處理，讓專案在 deploy 上去之後會透過 requirements.txt 去安裝套件。

而接下來我們會需要一個介面幫我們處理一些麻煩事，這邊就選用 wsgi 來處理。[詳細參考](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/362704/)

```bash=
sls plugin install -n serverless-python-requirements
sls plugin install -n serverless-wsgi
```

接著去看 serverless.yml 檔案最後出現下面這樣就成功安裝了。

```yaml=
plugins:
  - serverless-python-requirements
  - serverless-wsgi
```

然後新增一個 setup.cfg 並加入以下內容：

```bash=
[install]
prefix=
```

> 由於 AWS Lambda 上都會將套件安裝在本地端(Lambda 上)，因此在執行 `pip install -t . -r requirements.txt` 時會需要 setup.cfg。

接著選用 Flask 框架作我們開發的

```bash=
echo "Flask==1.0.2" > requirements.txt
pip install -r requirements.txt -—user
```

置換以下內容至 serverless.yml，讓 WSGI 去處理路由。

```yaml=
service: aws-wsgi-flask
provider:
  name: aws
  runtime: python3.7
functions:
  api:
    handler: wsgi_handler.handler
    events:
      - http: ANY /
      - http: ANY {proxy+}
custom:
  wsgi:
    app: api.app
plugins:
  - serverless-python-requirements
  - serverless-wsgi
```

因為我們不是單純只用 python 架設，所以要將 handler.py 替換成 api.py，並加入以下內容

```python=
from flask import Flask
app = Flask(__name__)

@app.route("/", methods=['GET'])
def handler():
    return 'hello world'

if __name__ == '__main__':
    app.run(debug=True)
```

到這邊基礎設施都弄好了，但總不能部署才測試吧，所以這時候我們就出動

```bash=
serverless wsgi serve
```

透過 wsgi 去幫忙起一個 local server 供使用者可以測試，而且每次儲存都重新整理一次哦！詳細用法請[參考](https://github.com/logandk/serverless-wsgi)
![](https://i.imgur.com/S4CQJxI.png)

接著用 postman 來測試一下
![](https://i.imgur.com/ci26g4O.png)

燈燈，hello world 出來了～

最後也是最重要的一個步驟『部署』
只是說部署前需要有 AWS 的 secret key & token

# 建立 AWS 存取金鑰

首先要先有個 AWS 帳號(廢話) ，如果沒有綁定信用卡記得去綁定才能繼續，接下來就可以去就可以去建立鑰匙囉！
![](https://i.imgur.com/A34OlY5.png)
如上圖，按下紅色框框的部分，接著按箭頭指的按鈕，就會建立一個我們要的金鑰囉！
![](https://i.imgur.com/JOgIjXQ.png)
如上圖所示，可以下載 key，AWS 不會幫忙保存 Secret key，若這視窗關掉的話 key 就不見了，所以使用者可以自行下載檔案保存，以免哪天要部署的時候找不到 key。
接著就將這兩個參數加入環境變數中：

```bash=
export AWS_ACCESS_KEY_ID=<your-key-here>
export AWS_SECRET_ACCESS_KEY=<your-secret-key-here>
```

或是藉由 serverless 指令來幫忙加入參數：

```
serverless config credentials --provider aws --key <your-key-here> --secret <your-secret-key-here>
```

> 這步驟很重要哦，不然 AWS 不認識你就不給你上傳了 🤣

最後就是下 `serverlesss deploy`進行部署
![](https://i.imgur.com/MK8Yks7.png)

這邊 AWS 幫忙建立一個含有 SSL 的 domain，可以直接對這個網址下

```yaml=
endpoints: ANY - https://4omvn4z7re.execute-api.us-east-1.amazonaws.com/dev
  ANY - https://4omvn4z7re.execute-api.us-east-1.amazonaws.com/dev/{proxy+}
```

接著把網址丟到瀏覽器上來試試看
![](https://i.imgur.com/tptNSd4.png)
出現 `hello world`就成功囉！

# 參考

- [sls cli 相關參數](https://serverless.com/framework/docs/providers/aws/cli-reference/)
- [Serverless](https://serverless.com/)
- [Serverless Github](https://github.com/serverless/serverless)
- [serverless install](https://serverless.com/framework/docs/providers/aws/guide/quick-start/)
- [serverless wsgi](https://serverless.com/plugins/serverless-wsgi/)
- [requirements.txt](https://serverless.com/plugins/serverless-python-requirements/)
