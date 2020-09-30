---
title: 用 Hexo 建立網站是否搞錯了什麼
date: 2020-09-27 14:41:05
categories: Github Page
tags:
 - hexo
 - GitHub-page
---

## Hexo 是什麼？

~~其實這我也不知道欸~~ 

我只知道他是一個可以讓你利用 markdown 語法寫文章，讓你用簡單的方式建立一個好看的網站

然後他是用 node 生成網頁的，所以你會需要 npm 來安裝他

## 安裝 Hexo CLI 

看到了這個標題，可能有人會問，什麼是CLI?

簡單來說就是可以讓你下指令的工具

既然都解釋完了，那我們先安裝 npm 吧！

### 安裝NPM

到 [Node官網](https://nodejs.org/en/download/package-manager/) 依照自己的作業系統選取安裝方式

~~絕對不是我懶才不寫的~~

安裝後在終端機輸入 `npm -v`

如果有出現一組版本號而不是奇怪的訊息，代表你成功安裝了

### 安裝 Hexo CLI

上面沒問題後，終端機不要關掉，我們在終端機輸入：

```bash
npm install -g hexo-cli
```

接著等他跑完，~~warning不要理他~~

就安裝完了

## 建立並設定網站

### 新增 Hexo 專案

在想要放網站的上一個資料夾中打開終端機，輸入：

```bash
hexo new your_new_website
```

等他跑完後，進到網站資料夾後，你就會一堆奇奇怪怪的東西

我們先不要理會裡面的東東，~~我們在乎的是結果~~

接著在終端機輸入：

```bash
hexo s
```

接著打開瀏覽器輸入 `http://localhost:4000` 你就會看到精美的網頁呈現在你面前

接下來我們就要把這個網頁變成自己的形狀

### 安裝主題套件

到 [Hexo 主題網站](https://hexo.io/themes/) 尋找你喜歡的樣式後，進入該主題的github將它下載下來

建議直接下載zip檔，省去之後刪除 `.git/` 的麻煩

下載並解壓縮後，將該資料夾放入 `theme/` 中，並在 `_config.yml` 中的theme欄位改成

```yaml
theme: <your_theme_name>
```

我自己適用 cactus 這個主題，所以下面有關主題的設定，我會使用 cactus 來教學

### 資料夾結構

我們到網站資料夾內會看到這樣子的檔案結構：

```
.
├── _config.yml
├── node_modules
├── package.json
├── scaffolds
│   ├── draft.md
│   ├── page.md
│   └── post.md
├── source
│   └── _posts/
├── themes
│   ├── cactus/
│   └── landscape/
└── yarn.lock
```

-   `_config.yml` 是這個網站的設定檔，主要設定都放在這裡
-   `scaffolds/` 資料夾內放的是新增文章時系統會幫你預設的範本
-   `source/` 資料夾內會放你的每一篇文章
-   `theme/` 資料夾內有你下載的各種主題
-   其他沒提到的就是npm, node 自帶的設定檔，可以當作沒看到

### 設定網頁資料

現在我們只需要好好的來看看 `_config.yml` 裡到底裝了什麼神秘的東西

我們先看第一段：

```yaml
# Site
title: Hexo
subtitle: ''
description: ''
keywords:
author: John Doe
language: en
timezone: ''
```

就... 看著英文標題指示輸入吧

下一段，這有點東西啊...

```yaml
# URL
## If your site is put in a subdirectory, set url as 'http://example.com/child' and root as '/child/'
url: http://example.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks
```

第4行的 `root` 是如果你的網站的網址是 `yourwebsite.com/blabla/` ，那你的 `root` 就要填入 `/blabla/` 

第5行的 `permalink` 是如果你想要更改後面網址的樣子可以自己更改

~~中間不怎麼重要~~

接下來我們到最下面deploy的部分

```yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: ''
```

空空如也，這東西我們等等處理，先把他記起來

接著，我們點進 `theme/` 發現裡面也有一個 `_config.yml` 

就照著上面英文註解填寫

最後記得在終端機輸入：

```bash
hexo s
```

確認自己網頁的樣子

### 部署到 Github

前置動作：[建立網站？ GitHub Page！]() (尚未完成，敬請期待)

然後在終端機輸入下列指令來新增 git 部署套件：

```bash
npm install hexo-deployer-git --save
```

接著在剛剛 `_config.yml` deploy 的位置改成這樣：

```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: <your_GitHub_repo>
  branch: gh-pages
  message:
```

輸入下列指令：

```bash
hexo deploy -g
```

這樣就完成了！



>   這篇的教學可能稍顯粗略，如果有什麼需要筆者補充或修正的，歡迎在下方留言區留言，我會盡快回覆！