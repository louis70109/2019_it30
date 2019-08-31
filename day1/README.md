# Serverless 介紹

![](https://i.imgur.com/abW5lFB.jpg)

[TOC]

## 前言

那因為現任公司的服務都是基於 AWS，如此這般我就接觸到 Serverless(以下簡稱 sls) 這個框架啦 (想更深入了解 FaaS 架構可以參考 AWS)

---

## 為什麼要 Serverless

Serverless 架構是一個基於 FaaS(Function as a Service) 實作的一個服務，讓開發者可以更專注在開發功能，將 yml 檔設定好其餘維運的問題都交給 AWS、Google、Azure 這些服務商去處理，只要把信用卡準備好就好(?)，讓開發者在寫完程式之後不用煩惱部署得問題，減少的不少的麻煩事。

以用一個 Ruby on Rails 寫的機器人為例，在寫完個 webhook function 後，設定路由然後部署，一般主機或是虛擬機就很麻煩，要安裝佈署環境以及一安裝堆相關套件，完了之後設定 Domain 什麼的有夠花時間。但若是使用 heroku 部署就很方便，Login-Create-Deploy 馬上就結束，機器人馬上就上線了，實在是俗又大碗呀～

但我覺得像是 Line Message 這種 stateless 的服務，且只要一個 function + route 就能實作的程式最適合 Serverless 了，讓專案可以簡潔有力，只要寫一個 function+yml 設定並打個指令就部署了，而且 domain 還附贈 SSL，將服務交給 AWS 也不需要擔心，整個就是超級方便啊！

---

## Cold start 問題

依照我目前使用的結果下來，在 heroku 以及 AWS Lambda 同時睡著的情況下，AWS 起床的回應速度大概 1 秒左右，而 heroku 則大概落在 10~15 秒(參考)。

如下圖所示的免費方案，基本上開發階段應該是不至於到 100 萬/月 個請求吧 😆，所以這部分就別擔新大力地給他用下去!

![](https://i.imgur.com/hC1Dgz4.png)

---

## 結論

接下來會是使用 Serverless 這個**框架**去實作的，使用的語言為 Python，至於為什麼會選它呢，最主要原因還是在官網上有很多 [plugin](https://serverless.com/plugins/) 可以用 ( NodeJS & Python )，開發時資源相對多，接著是 AWS Lambda 的 Code-start 最快的就屬於 NodeJS & Python 了，而我原本就是寫 Ruby 來的，自然就選一個寫法相近的語言啦(好像有點牽強)，如果說你想了解 runtimes 相關的問題的話可以參考[這篇](https://medium.com/the-theam-journey/benchmarking-aws-lambda-runtimes-in-2019-part-i-b1ee459a293d)。

## 環境

- MacOS 10.14.6
- [npm](https://nodejs.org/zh-tw/download/) 6.9.0
- [python](https://www.python.org/downloads/) 3.7.3
- [Serverless 套件(框架)](https://github.com/serverless/serverless) 1.45.1

## 參考

- [Serverless wiki](https://zh.wikipedia.org/wiki/%E7%84%A1%E4%BC%BA%E6%9C%8D%E5%99%A8%E8%A8%88%E7%AE%97)
- [Serverless 優缺點](https://denny.qollie.com/2016/05/22/serverless-simple-crud/)