---
title: "把程式移動至背景執行的方式"
date: 2024-03-11T11:38:00+08:00
lastmod: 2024-03-11T11:38:00+08:00
draft: false
author: "Divazone King"
authorLink: ""
tags: ["linux", "nohup"]
categories: ["linux"]
---

我們在登入伺服器，執行一個較耗時的程式時，通常我們會使用 ```nohup command &``` 的方式執行，如果我們在啟動時，忘記加上這 ```nohup``` 能否成功呢？

1. 首先使用 ```control + z``` 讓目前process暫停（Suspend）。
2. 然後我們使用 ```jobs``` 來查看它的jobspec。
3. 再用 ```bg %jobspec``` 來放入背景並繼續執行。
4. 最後使用 ```disown -h %jobspec``` 來使該作業忽略 HUP 訊號。

這個方法可以用在 ```scp``` 的指令中，在沒有設定 ssh 無密碼登入的情況下，我們無法使用 ```nohup``` 來執行 ```scp``` 指令，所以只能在開始大檔案copy後，透過上述方法讓這個程式被放置在背景執行。


## References
[Shell Scripting – JOB SPEC & Command](https://www.geeksforgeeks.org/shell-scripting-job-spec-command/)