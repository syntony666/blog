---
title: Discord 對話機器人製作全記錄（二）
date: 2020-07-23 08:33:34
tags:
 - discordBot
 - Python
---

# Discord 設定

## 開啟 Discord 開發者模式

![](discord-bot2-1.png)

## 建立 Discord Bot

到 [Discord 開發者頁面](https://discord.com/developers/applications/) 依照下圖操作：

### 新增機器人，然後輸入你的機器人名稱

![](discord-bot2-2.png)

### 填寫完機器人的基本資料，記住你的 CLIENT ID 及 CLIENT SECRET，等等會用到

![](discord-bot2-3.png)

### 邀請你的機器人

在網址列貼上:

```
https://discord.com/oauth2/authorize?client_id=<your_client_id>&scope=bot&permissions=8
```

選擇伺服器，按下邀請，就完成了

## 製作 Discord Bot

### 建立專案

在一個你喜歡的地方新增一個資料夾，然後新增 ```bot.py``` 

### 建立 ```bot.py```

在 ```bot.py``` 寫入下列程式碼：

```python
import discord
from discord.ext import commands

bot = commands.Bot(command_prefix='>')

if __name__ == "__main__":
    bot.run('your_client_secret') # 在括號中填入上面的CLIENT SECRET
```

接著在終端機輸入：

```bash
python3 bot.py
```

你會看到你的機器人已經在線上了！