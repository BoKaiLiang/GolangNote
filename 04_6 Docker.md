# 04_6 Docker

## Docker

Ref. [Docker介紹部落格](https://blog.hellosanta.com.tw/%E7%B6%B2%E7%AB%99%E8%A8%AD%E8%A8%88/%E4%BC%BA%E6%9C%8D%E5%99%A8/%E6%95%99%E4%BD%A0%E4%B8%80%E6%AC%A1%E5%AD%B8%E6%9C%83%E5%AE%89%E8%A3%9D-docker-%E9%96%8B%E5%A7%8B%E7%8E%A9%E8%BD%89-container%C2%A0%E5%AE%B9%E5%99%A8%E4%B8%96%E7%95%8C)

### 前言

* Docker 安裝開始玩轉 Container 容器世界
* 容器的解釋，想像是一個 「不被影像的獨立空間」，分成兩種
	+ 作業系統層 : 希望能夠獲得全系統完整控制的使用者。如 VirtualBox、VMWare ... 等。
	+ 應用軟體層 : 不論是服務或是被服務方，針對某種軟體有需求的使用者。如 [Docker](https://www.docker.com/)、[Rocket](https://coreos.com/rkt/docs/latest/)
* 其中 Docker 特色
	+ 需要在一台伺服器上安裝不同環境測試這個案子
	+ 案子在開發上的時候需要的伺服器資源種類很多
	+ 我真的需要一個甚麼都安裝的環境來滿足每一個案子嗎?
	+ 如果這樣想到就安裝會不會有版本衝突而互相牽制的可能？<br>
以上四點整理就是：一個有彈性管理環境

### 安裝前須知

* 需要有一台 64bit 伺服器資源，大部分 image 都使用 64 位元作業系統，也是 Docker 預設支援。
	* Image : Docker 在執行時需要一個基底環境。我們將這個環境稱為 Image，與作業系統安裝時的映象檔有差不多概念。而由 Image 建立起的容器稱為 Container。
* 伺服器選用 Linux 為 Host 是 Docker 最原始標準配備。
	* Host : 執行 Docker 需要一個作業系統管理資源。作業系統就稱為 Host。
* Linux Kernel 版本需大於 3.10
* 