---
title: WSL 安裝
date: 2019-09-25 18:30:00
tags: 
  - wsl
---

## WSL介紹

WSL 全名為 Windows Subsystem for Linux (適用於 Linux 的 Windows 子系統)

簡單來說就是在 Windows 底下做一個 Linux 的系統，Microsoft 目前只有支援文字化的指令，還沒有官方圖形化介面。

而 bash 就是實現 Linux 在 Windows 運作的介面。

## 安裝說明

### 方法一

進入控制台/程式集/開啟或關閉 Windows 功能中，將"適用於 Linux 的 Windows 子系統"旁的方塊打勾

重新開機後進入 Windows 市集搜尋 Ubuntu 安裝想要的版本即可

### 方法二

以系統管理員身分執行 Powershell 輸入指令:

```bash
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

重新開機後進入 Windows 市集搜尋 Ubuntu 安裝想要的版本即可

## 基本設定

### 設定使用者名稱及密碼

這裡只需照指示作即可

## 相關指令

### 更新資料庫及更新套件

更新資料庫:

```bash
sudo apt update
```

更新套件:

```bash
sudo apt upgrade
```
