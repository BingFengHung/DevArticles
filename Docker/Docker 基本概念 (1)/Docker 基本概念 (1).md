# Docker 基本概念 (1)

## 前言
在軟體開發過程中，最擔心開發環境與生產環境不一致，再加上環境的安裝與配置又是另外一個很耗時的工作。以前一般來說，要解決這方面的問題，是透過虛擬機 (Virutal Machine) 的方式，但 Virtual Machine 的缺點就是慢，體積又龐大。

因此 Docker 容器化技術的出現，大大的解決了這些不便，接下來將針對 Docker 進行說明。

## 什麼是 Docker?
> Docker 的概念是：**Build and Ship any Application Anywhere**

Docker 是一門輕量化、容器化的技術，他是一個開放的平台，可以用來開發、發布與執行。

Docker 讓開發人員可以將其開發的應用程式以及應用程式所需要的相關的依賴打包成一個獨立的單元，也就是容器 (Container)。

容器是一個獨立的隔離環境，可以與其他容器相互隔離開來，執行自己需要做的事情，有效的避免各自應用程式導致環境汙染的問題。

## Docker 的核心概念
Docker 核心概念在於在於映像檔與容器。

1. 映像檔 (Image)：他是一個只可以被讀取的模板，主要用來建立 Docker 容器。
2. 容器 (Container)：透過映像檔所建立出來的實例。

## Docker 運作方式
Docker 基本的運作方式如下：
1. Dockerfile：要建立映像檔時，所需的腳本檔案。
2. Docker image：根據 Dockerfile 產生的映像檔。
3. Docker container：透過映像檔產生出來的執行環境。

Dockerfile ---> Docker image ---> Docker container


## 系列文章
[Docker 基本概念 (1)-Docker 是甚麼](https://bingfenghung.github.io/blog/articles/Docker%3C_%3E%3EDocker%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(1))

[Docker 基本概念 (2)-映像檔與容器](https://bingfenghung.github.io/blog/articles/Docker%3C_%3E%3EDocker%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(2))

[Docker 基本概念 (3)-父映像檔](https://bingfenghung.github.io/blog/articles/Docker%3C_%3E%3EDocker%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(3))

[Docker 基本概念 (4)-使用 Dockerfile 建立映像檔](https://bingfenghung.github.io/blog/articles/Docker%3C_%3E%3EDocker%20%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5%20(4))
