---
title: Discord 對話機器人製作全記錄（四）
date: 2020-07-26 23:36:37
tags:
 - discordBot
 - Python
---

# 增加機器人的腦容量

在上一篇，我們讓機器人能夠回應我們，但如果我們有多個關鍵詞要回應，我們需要一個東西來裝他的回應及觸發的關鍵詞。在這篇文章，我們使用 MongoDB Atlas 線上資料庫來存取我們的回應及關鍵詞。

## MongoDB



## 初始化資料庫

進到 [MongodDB Atlas 官網](https://www.mongodb.com/cloud/atlas) 後，按下圖右上角的 **Try Free** 註冊帳號。

![](discord-bot4-1.png)

按下 **Create an Organization** ，然後照步驟設定

![](discord-bot4-2.png)

接著會跳出下圖，一樣按下 **New Project** ，然後照步驟設定

![](discord-bot4-3.png)

最後會跳出下圖，做完這個，你就會擁有一個資料庫了，一樣按下 **Build Cluster** ，然後照步驟設定

![](discord-bot4-4.png)

新增完資料庫需要等待1~3分鐘系統初始化，