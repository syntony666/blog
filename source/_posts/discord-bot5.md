---
title: Discord 對話機器人製作全記錄（五）
date: 2020-09-12 19:44:47
tags: 
 - discord-bot
 - Python
 - database
---

# 將資料庫連結至 Discord Bot

我們在第三篇學到如何讓機器人回應我們，第四篇知道如何連接資料庫，這篇我們要應用這兩篇的方法來讓機器人使用資料庫來記憶及讀取我們要他回應的話。

## 使用 MongoPy 連結機器人

利用上一篇的方法，在 `event.py` 的 `__init__(self)` 中新增下列程式碼：

```python
mongoClinet = MongoClient(<database_url>)  # 連結到資料庫
self.db = mongoClient['bot-db']
```

在 Python 中，使用 self 可讓該變數在相同類別中使用，所以我們就可以讓這個類別去取用 DB(資料庫) 中的資料。

## 取用資料庫

我們先來看看第三篇的 `on_message()` :

```python
@commands.Cog.listener()
    async def on_message(self, message):
        if message.author == self.bot.user: # 確認傳訊者不是機器人避免洗版
            return
        if message.content == 'foo': # 當收到訊息為‘foo’
            await message.channel.send('bar') # 回應‘bar’
```

依照上面的範本，我們可以利用資料庫讓機器人記住更多回應組合。

### 將回應組合寫入資料庫

要讓機器人回話之前，我們必須先告訴機器人要回應的話跟觸發的詞語，所以我們先新增一些回應組合到 DB：

```python
from pymongo import MongoClient
mongoClinet = MongoClient(<database_url>)  # 連結到資料庫
db = mongoClient['bot-db']
collection = db['reply']  # 進入Collection，若原本沒有則會自行新增

post = {
  "guild": <your_guild_id>, # 請自行輸入伺服器ID
  "receive": 'fool',
  "send": 'bar',
   }
collection.insert_one(post)  # 插入資料
```

在變數 `post`，我們儲存了伺服器 ID、觸發的話及機器人要回應的話

`insert_one` 是將傳入的 post 寫入至 DB

當然，你可以自由改變 receive 與 send 的值，甚至執行多次

### 讀取資料庫

資料寫入後，我們必須將資料讀取後，把結果告訴機器人讓他去回應。

我們必須先尋找內部的值，我們根據情境的不同可以利用 `find` 或是 `find_one` 來找出我們需要的值：

```python
from pymongo import MongoClient
mongoClinet = MongoClient(<database_url>)  # 連結到資料庫
db = mongoClient['bot-db']
collection = db['reply']  # 進入Collection

query={"server": <your_guild_id>}

for found in collection.find(query):	# 尋找多個結果
  print(found)

print(collection.find_one(query))	# 尋找單一結果
```

`find()`: 會回傳一個迭代器，必須利用for迴圈輸出結果

`find_one()`: 回傳單一值，若有多個結果只會回傳一個

`query`: 尋找的條件，型別為 dict

## 讓機器人回應資料庫內的回應組合

在上面，我們知道怎麼如何寫入及讀取資料庫，現在我們可以來讓腦容量增加之後的機器人回應我們說的話了。

```python
@commands.Cog.listener()
    async def on_message(self, message):
        if message.author == self.bot.user: # 確認傳訊者不是機器人避免洗版
            return
        found = self.db['reply'].find_one({'server': message.guild.id, 'receive': message.content})
        if found is not None:	# 當收到訊息有在資料庫內，則回應訊息
            await message.channel.send(found['send'])
```

我們更改了兩樣東西：

1. 利用 find_one 判斷是否要回話，若找得到這伺服器中有此回應組合就回應訊息
2. 讓機器人回應資料庫內的內容



在這篇，我們讓機器人利用資料庫回應訊息，但還是需要利用程式碼來添加回應組合。

下一篇，我們會知道如何使用指令來讓機器人存取資料庫

## 範例程式碼

### event.py

```python
import discord
from discord.ext import commands

class Event(commands.Cog):
    def __init__(self, bot):
        self.bot = bot
        mongoClinet = MongoClient(<database_url>)  # 連結到資料庫
				self.db = mongoClient['bot-db']
    
    @commands.Cog.listener()
    async def on_message(self, message):
        if message.author == self.bot.user: # 確認傳訊者不是機器人避免洗版
            return
        found = self.db['reply'].find_one({'server': message.guild.id, 'receive': message.content})
        if found is not None:	# 當收到訊息有在資料庫內，則回應訊息
            await message.channel.send(found['send'])

def setup(bot):
    bot.add_cog(Event(bot))
```

