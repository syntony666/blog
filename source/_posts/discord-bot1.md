---
title: Discord 對話機器人製作全記錄（一）
date: 2020-07-21 21:49:08
tags:
 - discord-bot
 - Python
---

# Python 環境設定

## 這東西可以幹嘛？

我們都知道Discord是一個非常著名的遊戲通訊軟體，跟台灣人常使用的Line比起來，有更多東西可以設定跟運用，當然API的取得比Line方便，我們可以拿來做歡迎訊息、投票、回話等互動功能，甚至於權限管理及文字獄他都做得到。

## 要怎麼做？？？

做這個機器人有很多套件可以用，你用Nodejs, Python等語言都行。一開始，我是使用Nodejs，但現在我是以Python來開發，所以這一系列文章會使用discord.py來教學。

首先，我們需要電腦上有Python，所以請依照你的作業系統的不同選擇你的安裝方法：

### Windows

到 [Python官網](https://www.python.org/downloads/)點Download，然後一路按繼續，記得選加到PATH不然你下指令會沒反應，直到你按下完成，就安裝完了。

### Ubuntu

正常來說，系統為幫你裝好，如果你進終端機輸入python3沒反應，那就輸入下列指令：

```bash
sudo apt install python3
```

### Mac

你要先安裝Homebrew這個套件管理器，指令如下：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

如果系統要叫你安裝任何東西，就大膽地按下去吧！

裝完之後就輸入下列指令後就完成了

```bash
brew install python3
```

如果卡在Updating homebrew的話可按Ctrl+C略過

## 安裝Python 虛擬環境

為什麼要做這件事？？？？？

簡單來說，Python安裝所有套件都是安裝在他的直譯器上，所以如果一但你不同的專案用到不同的套件，這樣你的套件庫會顯得很亂，所以我們需要在每一個專案上開不同的虛擬環境，指令如下：

```bash
pip3 install virtualenv
```

然後，在你的專案資料夾下輸入：

```bash
virtualenv <your_env_name>
```

啟動虛擬環境：

```bash
source /<your_env_name>/bin/active # For Unix or Linux
bot-env\Scripts\activate.bat # For Windows
```

接者你就可以在這狀態下安裝套件及開發了！

## 安裝所需套件

我們會需要discord.py這個套件，指令如下

```bash
pip install -U discord.py
```

