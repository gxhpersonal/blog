---
title: node版本管理工具n的使用
date: 2025-03-04 15:07:44
categories: node版本管理工具
tags: node版本管理工具
---

### nvm在 windows上用方便，n则在Mac上用方便

### 一、清除 node缓存
```
sudo npm cache clean --force
```
### 二、全局安装 n
```
sudo npm install n -g
```
### 三、安装不同版本的node
```
sudo n stable    // 安装最新稳定版
sudo n lts       // 安装最新长期支持版
sudo n 10.16.0   // 安装指定版本
```
### 四、n 切换不同 node版本
```
sudo n //用键盘上下键切换不同 node版本
```
### 五、删除node指定版本
```
sudo n rm 10.16.0
```