# create domain route53

讓使用者使用像這樣子的路由能看嗎 🤣，一般的使用者根本記不起來
![](https://i.imgur.com/vLLfKyr.png)
這時候就需要一個 domain 讓我們的網址比較好看，這邊我已經先在 Route 53 上註冊了一個 `*.nijialin.com` 的網域了

只是說原本的 domain 已經有 SSL 了，但是代理沒有這個東西

route 53

像我部署在 ohio，想想光網路的時間+Cold start 的時間就已經去了一大半，服務沒有 Timeout 我該慶幸了，既然如此就不得不認識一下 CDN 服務，而 AWS 的 CDN 服務就是 cloudfont 啦

但在建立 cloudfont 以前需要先到 Certificate Manager，接著會用圖片帶大家一步一步做

- 先來到 Certificate Manager 頁面，按下左下角的這個

![](https://i.imgur.com/WVk2YFJ.png)

- 接著就直接按右下角啦，但是要確定是不是 public

![](https://i.imgur.com/sNxd3LG.png)

- 這裡輸入你在 route53 註冊的 domain，這邊我是輸入 \*.nijialin.com

![](https://i.imgur.com/FXnknbj.png)

- 到第四部之後就會看到申請的 SSL Pending

![](https://i.imgur.com/WlY5PCf.png)

- 最後回首頁之後就會看到 SSL 簽證正在送簽中
  ![](https://i.imgur.com/kRRtvJ5.png)
