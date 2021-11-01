# Ubuntu安裝Docker.md

這裡介紹在 Ubuntu Linux 中安裝 Docker 的步驟、基本操作教學以及 hello world 程式。

目前主流的架構虛擬化技術，是將整個完整的作業系統包起來，放在 hypervisor 上面執行（例如 VirtualBox 與 VMWare），這種方式可以讓一台硬體主機同時執行多種不同的作業系統，但缺點就是執行多的作業系統需要較多的硬體資源。


Docker 是屬於另外一種虛擬化技術，它透過 container 的方式將應用程式與相關所需的執行環境包起來，讓每個應用程式共用系統核心（kernel），但還是都有各自獨立的根目錄、檔案系統、網路環境、行程管理等，對於程式而言就好像在一般的獨立的系統上執行一樣。



[![docker-containers-20161117-1](https://blog.gtwang.org/wp-content/uploads/2016/11/docker-containers-20161117-1.png)](https://blog.gtwang.org/wp-content/uploads/2016/11/docker-containers-20161117-1.png)

Docker 架構

以下我們以 Ubuntu 16.04.1 LTS 作為測試環境，示範如何安裝基本的 Docker 執行環境，以及 container 的操作方式。

## 安裝 Docker

在 Ubuntu Linux 中，使用 apt 安裝 Docker 比較方便：

```bash
sudo apt-get install docker.io
```

安裝好之後，查看一下 docker 服務是否有正常啟動：

```bash
service docker status
```

正常的話，應該可以看到綠色的狀態：

[![ubuntu-linux-install-docker-tutorial-20161117-1](https://blog.gtwang.org/wp-content/uploads/2016/11/ubuntu-linux-install-docker-tutorial-20161117-1.png)](https://blog.gtwang.org/wp-content/uploads/2016/11/ubuntu-linux-install-docker-tutorial-20161117-1.png)

查看 docker 服務狀態

接著將自己的使用者帳號加入至 `docker` 群組：

```bash
sudo usermod -aG docker gtwang
```

登出再重新登入之後，就可以開始使用 Docker 了。首先查看 Docker 的版本資訊：

```bash
docker version
```

正常來說輸出會類似這樣：

```bash
Client:
 Version:      1.12.1
 API version:  1.24
 Go version:   go1.6.2
 Git commit:   23cf638
 Built:        Tue, 27 Sep 2016 12:25:38 +1300
 OS/Arch:      linux/amd64

Server:
 Version:      1.12.1
 API version:  1.24
 Go version:   go1.6.2
 Git commit:   23cf638
 Built:        Tue, 27 Sep 2016 12:25:38 +1300
 OS/Arch:      linux/amd64
```

如果出現這樣的訊息，就表示有問題，通常是因為帳號沒有加入 `docker` 群組，權限不足的因素，只要按照上述的方式將帳號加入 `docker` 群組即可解決。

```bash
Client:
 Version:      1.12.1
 API version:  1.24
 Go version:   go1.6.2
 Git commit:   23cf638
 Built:        Tue, 27 Sep 2016 12:25:38 +1300
 OS/Arch:      linux/amd64
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

## 取得 Docker Container 映像檔

安裝好 Docker 之後，接著就要下載 container 的映像檔，在 [Docker Hub](https://hub.docker.com/) 上面有許多公開的 container 映像檔，我們可以利用 `docker` 的 `search` 指令來尋找自己需要的 container 映像檔，例如搜尋 Ubuntu Linux 的 container 映像檔：

```bash
docker search ubuntu
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
ubuntu                            Ubuntu is a Debian-based Linux operating s...   5056      [OK]       
ubuntu-upstart                    Upstart is an event-based replacement for ...   68        [OK]       
rastasheep/ubuntu-sshd            Dockerized SSH service, built on top of of...   49                   [OK]
consol/ubuntu-xfce-vnc            Ubuntu container with "headless" VNC sessi...   29                   [OK]
torusware/speedus-ubuntu          Always updated official Ubuntu docker imag...   27                   [OK]
ubuntu-debootstrap                debootstrap --variant=minbase --components...   27        [OK]       
ioft/armhf-ubuntu                 [ABR] Ubuntu Docker images for the ARMv7(a...   19                   [OK]
nickistre/ubuntu-lamp             LAMP server on Ubuntu                           11                   [OK]
[略]
```

若要下載 container 映像檔，可以使用 `pull` 並且指定 container 的名稱：

```bash
docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu

aed15891ba52: Pull complete 
773ae8583d14: Pull complete 
d1d48771f782: Pull complete 
cd3d6cd6c0cf: Pull complete 
8ff6f8a9120c: Pull complete 
Digest: sha256:35bc48a1ca97c3971611dc4662d08d131869daa692acb281c7e9e052924e38b1
Status: Downloaded newer image for ubuntu:latest
```

`docker` 的 `images` 指令可以列出目前系統上所有的 container 映像檔：

```bash
docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              e4415b714b62        10 hours ago        128.1 MB
```

## Hello World

有個 container 映像檔之後，我們就可以在 container 中執行程式了，以下是在 container 中執行程式的 hello world 範例：

```bash
docker run ubuntu /bin/echo 'Hello world'
Hello world
```

這裡我們選擇 `ubuntu` 這個剛下載下來的 container 作為程式執行的環境，在這個環境中執行 `/bin/echo 'Hello world'` 這個指令，這樣就是 Docker 最簡單的使用方式。

若要進入 Docker container 中的執行環境，可以使用 `docker` 的 `run` 指令，加上 `-i` 與 `-t` 參數進入互動式的操作介面並且配置 tty：

```bash
docker run -i -t ubuntu
```

[![ubuntu-linux-install-docker-tutorial-20161117-2](https://blog.gtwang.org/wp-content/uploads/2016/11/ubuntu-linux-install-docker-tutorial-20161117-2.png)](https://blog.gtwang.org/wp-content/uploads/2016/11/ubuntu-linux-install-docker-tutorial-20161117-2.png)

進入 Docker container

若要離開 container 的互動式操作，則執行 `exit` 即可，使用上跟一般的 shell 差不多。

如果直接執行一個沒有下載的 container，Docker 也會自動從網路上下載映像檔來執行：

```bash
docker run -it centos
```

也就是說 `pull` 的步驟是可以省略的。

### Daemon

若要使用 Docker 來執行 daemon 類型的常駐程式，可以使用 `-d` 參數，讓整個 container 放在背景執行，例如：

```bash
docker run -d ubuntu /bin/sh -c "while true; do echo hello world; sleep 1; done"
```

執行之後，會輸出一串很長的字串：

```bash
8764388588e5be54e4d04ff622f25753cab14a56c561523cdd58b03021dd8c6c
```

這個就是 container 的 ID，可用於後續的 container 管理。

我們可以使用 `docker` 的 `ps` 指令來查看目前所有正在執行的 container：

```bash
docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
8764388588e5        ubuntu              "/bin/sh -c 'while tr"   3 minutes ago       Up 3 minutes                            boring_franklin
```

最後一個 `NAMES` 欄位是 container 執行個體的名稱，這個名稱是 Docker 自己隨機取的，在指令中若要對該 container 進行操作，可以用這個名稱來指定。

在 container 中的程式輸出訊息可以透過 Docker 的 `logs` 紀錄檔功能來查閱，例如查看 `boring_franklin` 這個 container 的記錄檔：

```bash
docker logs boring_franklin
hello world
hello world
hello world
hello world
hello world
[略]
```

若要終止正在執行中的 container，可以使用 `stop` 指令：

```bash
docker stop boring_franklin
boring_franklin
```

以上就是 Ubuntu Linux 中最基本的 Docker 安裝與操作。

參考資料：[Unixmen](https://www.unixmen.com/installing-docker-ubuntu-16-04-lts-mint-17-centos-7/)、[Docker](https://docs.docker.com/engine/installation/linux/ubuntulinux/)、[Docker 原理圖解及全環境安裝](https://joshhu.gitbooks.io/docker_theory_install/content/DockerBible/ubuntu_linuxdocker.html)、[Docker tutorials](https://docs.docker.com/engine/tutorials/dockerizing/)