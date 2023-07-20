---
title: CakePHP2 升级PHP8.1流程
date: 2022-12-07 16:26:30
tags: [cakephp]
categories: [技术,cakephp]
---
暂停Apache 服务
替换原本php安装包，原安装包备份
![图片未加载](CakePHP2-升级PHP8-1流程/1.jpg)

修改infrasys\apache\conf\httpd.conf
![图片未加载](CakePHP2-升级PHP8-1流程/2.jpg)

修改php.ini和php-cil.ini
所有路径保证没有错误
![图片未加载](CakePHP2-升级PHP8-1流程/3.jpg)

错误处理：
1：cakephp版本不兼容
![图片未加载](CakePHP2-升级PHP8-1流程/4.jpg)
替换新的cakephp2 兼容文件 
