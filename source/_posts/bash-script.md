---
title: Ubuntu 相關指令集
date: 2019-09-24 00:05:54
tags:
  - ubuntu
---

## Update the package lists:

```bash
sudo apt update
```

## Upgrade the package version:

```bash
sudo apt upgrade
```

## Install C++ with Google Test

```bash
sudo apt update
sudo apt install g++ make libgtest-dev cmake
cd /usr/src/gtest
sudo cmake CMakeLists.txt
sudo make
sudo cp *.a /usr/lib
```

## Install Java

```bash
sudo apt install default-jdk default-jre maven
```

## Install Python

Linux would have installed Python 2.x and Python 3.x.

If you don't find it, install it by using following commands:

```bash
sudo apt install python3   //安裝python 3.x
sudo apt install python    //安裝python 2.x
```
