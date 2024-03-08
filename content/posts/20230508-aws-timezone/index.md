---
title: "AWS Lambda 時區變更"
date: 2023-05-08T10:58:25+08:00
lastmod: 2023-05-08T10:58:25+08:00
draft: false
author: "Divazone King"
authorLink: ""
tags: ["aws"]
categories: ["coding_life"]
---

在使用AWS Lambda的時候，發現它裡面的時區並不會使用使用者帳號所設定的時區，都是使用UTC時區。

如果要改變AWS Lambda所使用的時區的話，只要去各Lambda function的Environment variables裡面設定就可以了

## Environment variables
| Key | Value | 
| -------- | -------- |
| Tz    | Asia/Taipei     |
