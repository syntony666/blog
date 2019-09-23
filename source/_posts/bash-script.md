---
title: Ubuntu 相關指令集
date: 2019-09-24 00:05:54
tags:
 - ubuntu
---

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
