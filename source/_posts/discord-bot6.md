---
title: Discord 對話機器人製作全記錄（六）
date: 2020-09-19 21:29:41
categories: Discord Bot
tags:
 - discord-bot
 - Python
 - database
---

# 新增、修改及刪除回應組合

上一篇我們讓機器人可以利用資料庫，讓機器人知道我們說哪些話時，他需要回應我們什麼。但是如果我們想更動的話，我們就必須去操作資料庫，感覺很不方便，所以我們可以利用指令去設定機器人的回應組合。

> 此文的機器人前綴為 " ; " ，若一個指令為 ` ;command subcommand `，
>
> 則command為主指令，subcommand為子指令

## 新增機器人指令

還記得 `on_message()` 嗎？

我們當然可以使用這個方法來新增機器人指令，但如果指令一多，程式會顯得非常肥大，像是下面這樣：

```python
@commands.Cog.listener()
async def on_message(self, message):
    if message.author == self.bot.user: # 確認傳訊者不是機器人避免洗版
        return
    found = self.db['reply'].find_one({'server': message.guild.id, 'receive': message.content})
    if found is not None:
        await message.channel.send(found['send'])
  	if message.content[0] == ";command01":
        # do something...
  	elif message.content[0] == ";command02":
        # do something...
  	elif message.content[0] == ";command03":
        # do something...
  	elif message.content[0] == ";command04":
        # do something...
  	elif message.content[0] == ";command05":
        # do something...
```

感覺就不是很妥當，所以 discord.py 讓我們可以利用 `@commands.command()` 來處理機器人的指令。

### 指令函式

下列程式碼是設定指令的方式，我們先看一下，再來解釋這東西在做什麼。

```python
@commands.command(aliases=['alias1','alias2','alias3'])
def command_name(ctx, arg, arg2):
  	# do something
```

第一行的 `@commands.command()` 是告訴程式說這是一個指令，而指令的名稱就是下面那行的 `command_name`，括號中的aliases指的是利用陣列中的字串也能呼叫該指令，所以如果不需要括號中就不用放東西。

第二行括號中的ctx代表這個指令的訊息物件，就跟上面 `on_message()` 中的 message是同一個東西，如果後面還有其他參數就直接增加參數數量就行了。

### 指令函式組

當我們想在同一個主指令下新增子指令時，我們可能會需要再大指令內使用很多if判斷式來判斷有沒有這個指令，所以 discord.py 也提供我們用 `@group.command()` 的方式來增加子函式，用法如下列程式碼：

```python
@group.command(aliases=['alias1','alias2','alias3'])
def command_name(ctx):	# ;command_name
  	# do something
@command_name.command(aliases=['alias1','alias2','alias3'])
def subcommand1(ctx):		# ;command_name subcommand1
  	# do something
@command_name.command(aliases=['alias1','alias2','alias3'])
def subcommand2(ctx):		# ;command_name subcommand2
  	# do something
    
# 註解內容為指令

```

我們只要把主指令的  `@commands.command` 改成 `@group.command` ，然後子指令上加一行 `@command_name.command` 就行了。

## 利用指令操作資料庫

### 設定主指令

我們將主指令設為 **reply** ， 而且單純只輸入主指令時，機器人是不會反應的，所以我們就直接使用 `pass` 語句來讓主函式不做事，至於要做其他處理，先別急，在往後幾篇會提到。

我們先增加一個檔案叫做 `reply.py` ，然後輸入下列程式碼：

```python
import discord
from discord.ext import commands
from pymongo import MongoClient

class Reply(commands.Cog):
    def __init__(self, bot):
        self.bot = bot
        mongoClinet = MongoClient(<database_url>)
        self.db = mongoClient['bot-db']
    
    @group.command()
    async def reply(self, message):   #做一個不做事的指令
        pass

def setup(bot):
    bot.add_cog(Reply(bot))
```

~~就這樣，我們創造了一個不做事的主指令了~~

### 設定新增及修改指令

上面我們新增一個不做事的指令，現在我們可以利用子指令來讓機器人去新增回應組合，程式碼如下：

```python
@reply.command(aliases=['a', 'add'])	# 增加指令的同義詞
async def add_reply(self, ctx, keyword, *, msg):
    server = ctx.message.guild.id		# 獲取伺服器ID
    found = self.db['reply'].find_one({'server': server, 'receive': keyword})	# 搜尋回應組合
    
    if found is not None:		# 如果在資料庫有該回應組合，則直接修改
        self.db['reply'].find_one_and_update({'server': server, 'receive': keyword}, {'$set': {'send': msg}})
        return

    self.db['reply'].insert({'server': server, 'receive': keyword, 'send': msg})	# 若無，則加進資料庫
```

第一行的 aliases ，可以讓我們輸入 `;reply a aaa bbb`、 `;reply add aaa bbb` 或 `;reply add_reply aaa bbb` ，都可以讓機器人在我們說aaa的時候回應bbb

第七行的 `find_one_and_update` 的第一個參數是搜尋匹配值，第二個是將找到的資料更改成 `$set` 後面的dict

就這樣，我們就可以讓機器人去新增回應組合了

### 設定刪除指令

有時候我們會誤觸到一些語句讓機器人一直亂回應，我們可以利用子指令叫機器人把那個回應組合刪掉，程式碼如下：

```python
@reply.command(aliases=['d', 'delete'])
async def delete_reply(self, ctx, keyword):
    server = ctx.message.guild.id
    found = self.db['reply'].find_one({'server': server, 'receive': keyword})

    if found is not None:
        self.db['reply'].find_one_and_delete({'server': server, 'receive': keyword})
```

第七行的 `find_one_and_delete` 的參數是搜尋匹配值，然後將符合條件的刪掉一個

所以當我們輸入 `;reply d aaa`、 `;reply delete aaa` 或 `;reply delete_reply aaa` ，就可以刪除該aaa的回應組合

### 列出回應列表指令

回應組合用久了，數量一定會變多，這時我們需要一個列表來看到底有哪些回應組合，我們可以叫機器人幫我們列出來，程式碼如下：

```python
@reply.command(aliases=['l', 'list'])
async def get_list(self, ctx):
    if ctx.invoked_subcommand is None:
        server = ctx.message.guild.id
        await ctx.channel.purge(limit=1)
        if self.db['reply'].find_one({'server': server}) is None:
            await ctx.send('**沒有回應列表**')
            return
        for x in self.db['reply'].find({'server': server}):
        	await ctx.send(x['receive']+': '+x['send'])
```

我們可以用find來尋找在這伺服器中使用的所有回應組合，然後利用迴圈印出來





我們已經完整地作出回應機器人了，但他現在只能在自己的電腦上跑，這樣風險有點高

下一篇，我們來讓機器人在免費伺服器運作



## 範例程式碼

### reply.py

```python
import discord
from discord.ext import commands
from pymongo import MongoClient

class Reply(commands.Cog):
    def __init__(self, bot):
        self.bot = bot
        mongoClinet = MongoClient(<database_url>)
        self.db = mongoClient['bot-db']
    
    @group.command()
    async def reply(self, message):   #做一個不做事的指令
        pass
    
    @reply.command(aliases=['l', 'list'])
    async def get_list(self, ctx):
        if ctx.invoked_subcommand is None:
            server = ctx.message.guild.id
            await ctx.channel.purge(limit=1)
            if self.db['reply'].find_one({'server': server}) is None:
                await ctx.send('**沒有回應列表**')
                return
            for x in self.db['reply'].find({'server': server}):
                await ctx.send(x['receive']+': '+x['send'])
    
    @reply.command(aliases=['a', 'add'])	# 增加指令的同義詞
    async def add_reply(self, ctx, keyword, *, msg):
        server = ctx.message.guild.id		# 獲取伺服器ID
        found = self.db['reply'].find_one({'server': server, 'receive': keyword})	# 搜尋回應組合

        if found is not None:		# 如果在資料庫有該回應組合，則直接修改
            self.db['reply'].find_one_and_update({'server': server, 'receive': keyword}, {'$set': {'send': msg}})
            return

        self.db['reply'].insert({'server': server, 'receive': keyword, 'send': msg})		# 若無，則加進資料庫
    
    @reply.command(aliases=['d', 'delete'])
    async def delete_reply(self, ctx, keyword):
        server = ctx.message.guild.id
        found = self.db['reply'].find_one({'server': server, 'receive': keyword})

        if found is not None:
            self.db['reply'].find_one_and_delete({'server': server, 'receive': keyword})

def setup(bot):
    bot.add_cog(Reply(bot))
```

