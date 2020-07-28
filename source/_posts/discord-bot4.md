---
title: Discord 對話機器人製作全記錄（四）
date: 2020-07-26 23:36:37
tags:
 - discord-bot
 - Python
 - database
---

# 增加機器人的腦容量

在上一篇，我們讓機器人能夠回應我們，但如果我們有多個關鍵詞要回應，我們需要一個東西來裝他的回應及觸發的關鍵詞。在這篇文章，我們使用 MongoDB Atlas 線上資料庫來存取我們的回應及關鍵詞。

## 初始化資料庫

進到 [MongoDB Atlas 官網](https://www.mongodb.com/cloud/atlas) 後，按下圖右上角的 **Try Free** 註冊帳號。

![](discord-bot4-1.png)

按下 **Create an Organization** ，然後照步驟設定

![](discord-bot4-2.png)

接著會跳出下圖，一樣按下 **New Project** ，然後照步驟設定

![](discord-bot4-3.png)

最後會跳出下圖，做完這個，你就會擁有一個資料庫了，一樣按下 **Build Cluster** ，然後照步驟設定

![](discord-bot4-4.png)

新增完資料庫需要等待1~3分鐘系統初始化，結束後會看到下圖畫面

![](discord-bot4-5.png)

點選左側選單中的 **Database Access** ，點選 **Add New Database User** ，設定使用者帳號密碼

![](discord-bot4-6.png)

點選左側選單中的 **Network Access** ，點選 **Add IP Address** 

![](discord-bot4-7.png)

點選 **ALLOW ACCESS FROM ANYWHERE** 後，按下  **Confirm**

![](discord-bot4-8.png)

這樣我們就成功設定好資料庫了

## 連接資料庫

### 確認資料庫URL

點選下圖中的 **CONNECT** 

![](discord-bot4-5.png)

接著，依下圖指示操作

![](discord-bot4-9.png)

![](discord-bot4-10.png)

將 `mongodb+srv://<url>` 記起來等等會用到

### 使用 MongoDB API

安裝 mongopy (因為網址有srv所以要安裝mongo[srv])

```bash
pip install pymongo[srv] pymongo
```

接著，新增檔案 `database.py`，這個檔案只是要測試是否有連接到資料庫

在 `database.py` 寫入下列程式碼：

```python
from pymongo import MongoClient
mongoClinet = MongoClient(<database_url>)  # 連結到資料庫
db = mongoClient['bot-db']
collection = db['aaa']  # 進入Collection，若原本沒有則會自行新增

print('connected')

post = {"foo":"bar"}
collection.insert_one(post)  # 插入資料

print(collection.find_one())  # 將資料從資料庫印出
```

這時，我們確定可以正常連接到資料庫，下一篇，我們就可以將資料庫連結到機器人。