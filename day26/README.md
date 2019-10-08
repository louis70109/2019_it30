# Use CDN (1) - create Certificate

## 簡介

繼上一篇 warm-up 機制之後來了解一下如何使用 AWS 的 CDN 服務 - cloudfont，CDN 顧名思義就是要讓使用者可以到最接近的主機上去拉到對應的資料，透過撒點的方式讓主機去挑選離自己近的服務據點，藉此加速服務。

像我之前部署在 ohio，想想光網路的時間 + Cold start 的時間就已經去了一大半，即便我們有熱開機，服務沒有 Timeout 我該慶幸了，既然如此就不得不認識一下 CDN 服務，而 AWS 的 CDN 服務就是 cloudfont 啦！

只是說原本的 domain 已經有 SSL 了，但是代理沒有這個東西，所以我們要 ban 出一個 Certificate，那在在建立 cloudfont 以前需要先到 Certificate Manager，接著會用圖片帶大家一步一步做

![](https://i.imgur.com/vLLfKyr.png)

## 實作

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

最後就等他完成嚕！將將
![](https://i.imgur.com/495PjEw.png)
