---
title: Discord 對話機器人製作全記錄（三）
date: 2020-07-24 00:16:01
tags:
 - discord-bot
 - Python
---

# 製作回應機器人

在前一篇文章，我們可以讓機器人上線了，但是我們必須要讓他做點事才行。

這篇文章會教你如何讓機器人回應你所指定的話。

## Cog 

### 概念

為了清楚的分類我們的指令及機器人的行為，我們可以使用Cog來將我們的指令包成不同的物件。

### 主要架構

在每個 Cog 文件都會包含一個類別及一個setup()來告訴Cog來取用。

形式如下：

```python
import discord
from discord.ext import commands

class CogName(commands.Cog): # 物件需繼承 Cog
    def __init__(self, bot):
        self.bot = bot
        
    ...some methods

def setup(bot):
    bot.add_cog(CogName(bot))
```

在 Cog 中，處理機器人狀態需使用裝飾詞`@commands.Cog.listener()` 標註，定義指令則需使用 `@commands.command()` 標註。

## 處理機器人狀態

我們需要先新增一個檔案 `event.py` ，寫入下列程式碼：

```python
import discord
from discord.ext import commands

class Event(commands.Cog):
    def __init__(self, bot):
        self.bot = bot

def setup(bot):
    bot.add_cog(Event(bot))
```

### on_ready

在上一篇文章，我們利用 Discord 自己的提示讓我們知道機器人上線了，但這方法不是不好，就感覺不夠明確，我們可以叫機器人建置好後告訴我們，我們可以在 `on_ready` 中定義準備好時的行為。

範例如下 :

```python
@commands.Cog.listener()
    async def on_ready(self):
        print('Ready!')
        print('Logged in as ---->' , self.bot.user) # self.bot.user 回傳 機器人名稱#1234
        print('ID:', self.bot.user.id) # self.bot.user.id 回傳 機器人ID
```

將上面的函式放入 `event.py` 的 `class event` 後，在終端機輸入 `python3 bot.py` ，就會顯示下列文字：

```
Ready!
Logged in as ----><your_bot_name>
ID:<your_bot_id>
```

則表示你的機器人已經上線了

### on_message

這個函式是處理當任何使用者傳訊息時，機器人要做的事，我們可以用它來向使用者回話。

這個函式接收了一個 `message` 物件，我們可以利用它來確認傳訊者的訊息內容、ID及頻道並回應傳訊者。

所以我們可以使用if判斷式來讓機器人回應，範例如下：

```python
@commands.Cog.listener()
    async def on_message(self, message):
        if message.author == self.bot.user: # 確認傳訊者不是機器人避免洗版
            return
        if message.content == 'foo': # 當收到訊息為‘foo’
            await message.channel.send('bar') # 回應‘bar’
```

將上面的函式放入 `event.py` 的 `class event` 後，在終端機輸入 `python3 bot.py` ，確認機器人在線上後，當我們在機器人所在的伺服器傳送 'foo' 時，他會回應我們 'bar'。

## 範例程式碼

### bot.py

```python
import discord
from discord.ext import commands
bot = commands.Bot(command_prefix='>')

if __name__ == "__main__":
    bot.run('your_client_secret') # 在括號中填入上面的 CLIENT SECRET
```

### event.py

```python
import discord
from discord.ext import commands

class Event(commands.Cog):
    def __init__(self, bot):
        self.bot = bot

    @commands.Cog.listener()
    async def on_ready(self):
        print('Ready!')
        print('Logged in as ---->' , self.bot.user) # self.bot.user 回傳 機器人名稱#1234
        print('ID:', self.bot.user.id) # self.bot.user.id 回傳 機器人ID
    
    @commands.Cog.listener()
    async def on_message(self, message):
        if message.author == self.bot.user: # 確認傳訊者不是機器人避免洗版
            return
        if message.content == 'foo': # 當收到訊息為‘foo’
            await message.channel.send('bar') # 回應‘bar’

def setup(bot):
    bot.add_cog(Event(bot))
```

