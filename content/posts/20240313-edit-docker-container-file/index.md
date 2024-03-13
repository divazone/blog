---
title: "編輯docker容器中的文件"
date: 2024-03-13T08:39:00+08:00
lastmod: 2024-03-13T08:39:00+08:00
draft: false
author: "Divazone King"
authorLink: ""
tags: ["container"]
categories: ["container"]
---

## 寫在前面
### 為什麼要這麼做？
實際上我們並不需要也不需要直接編輯容器中的檔案。Docker 容器是不可修改的工作單元，用於運行單一、特定的進程。鏡像應該在沒有任何影響的情況下能夠被建立和運行。

只是在開發期間，對 Docker 容器中的文件進行編輯可能才有些用處，這讓我們在不需要重新建立鏡像的狀態下驗證是否達到了預期的效果，可以達到節省時間、提高開發效率的目的，但是在完成驗證後，應該刪除增加到鏡像中的多於內容，使驗證後的結果永久化到鏡像中。

另外需要提醒的一點是，當我們在一個運行著的容器中編輯一個文件後需要確保所依賴的這個文件的process收到了文件編輯的通知並進行了設定更新，如果沒有類似的通知機制，需要手動重啟這些process使修改能生效。

下面假設你所使用的容器中沒有 vi 等文字編輯工具，我們以 openjdk:23 作為示範：
```
➜ docker run -it openjdk:23 bash
root@d0fb3a0b527c:/# vi demo.java
bash: vi: command not found
root@d0fb3a0b527c:/#
```

## 以下介紹五種常用方法：
### 方法1：使用掛載
準備Dockerfile：
```
FROM openjdk:23
WORKDIR "/app"
```

編譯鏡像：
```
docker build -t sample .
```

最後，運行標記為掛載的容器：
```
docker run --rm -it --name=sample_demo -v $PWD/app-vol:/app sample bash
```

如果本機 $PWD/app-vol 目錄不存在，會自動建立。此後在 $PWD/app-vol 下的檔案操作會對應在容器的 /app 目錄下。

### 方法2：安裝編輯器
準備Dockerfile：
```
FROM openjdk:23
WORKDIR "/app"
```

編譯鏡像：
```
docker build -t sample .
```

啟動容器並進行vim編輯器安裝：
```
docker run --rm -it --name=sample_demo sample bash

root@4b72fbabb0af:/app# apt-get update
root@4b72fbabb0af:/app# apt-get -y install vim
```

如果需要重複使用，更好的做法是寫在 Dockerfile 中：
```
FROM openjdk:23
RUN ["apt-get", "update"]
RUN ["apt-get", "-y", "install", "vim"]
WORKDIR "/app"
```

### 方法3：將文件複製到正在運行的容器中
```
docker cp demo.java sample_demo:/app
```
另一個類似的方法只要 docker exec 和 cat 結合使用，下面的指令同樣把 demo.java 檔案複製到正在運作的容器中：
```
docker exec -i sample_demo sh -c 'cat > /app/demo.java' < demo.java
```

### 方法4：使用Linux工具
雖然容器中通常沒有安裝編輯工具，但是其他Linux工具，如：sed、awk、echo、cat、cut等還是有的，可以派上用場。例如sed和awk可以編輯文件的適當位置，還可以將echo , cat, cut 聯合起來並藉助強大的指令流和編輯文件。如前文所示，這些工具可以與docker exec 命令結合使用，從而發揮更強大的威力。

### 方法5：使用遠端vim（或其他編輯器）
這種方法只是為了開拓思路，並不會在實際中使用，因為並不安全。

準備Dockerfile：
```
FROM openjdk:23
RUN ["apt-get", "update"]
RUN ["apt-get", "install", "-y", "openssh-server"]
RUN mkdir /var/run/sshd
RUN echo 'root:password' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN ["/etc/init.d/ssh", "start"]
EXPOSE 22
WORKDIR "/app"
CMD ["/usr/sbin/sshd", "-D"]
```

因為我們要藉助 scp 來遠端進行檔案編輯，所以需要安裝 openssh-server 並開放其連接埠。

編譯並運行：
```
docker build -t sample .
docker run --rm -p 2222:22 -d --name=sample_demo sample
```

現在我們可以使用以下指令來編輯demo.java檔了：
```
vim scp://root@localhost:2222//app/demo.java
```

註：在 vi 需要先執行 ```:set bt=acwrite``` 指令再去編輯文件，相關討論請見：https://github.com/vim/vim/issues/2329


編輯完成儲存並退出後，可以使用下面的命令來驗證檔案確實被建立和儲存了：
```
docker exec -it sample_demo cat /app/demo.java
```