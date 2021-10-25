## 安裝 Docker CE

由於 Docker 後來分為 Docker EE（Enterprise Edition）與 Docker CE（Community Edition）兩種版本，新的套件名稱與舊的不同，所以在安裝 Docker 之前，要先移除 CentOS Linux 系統上舊版的 Docker 套件：

```bash
sudo yum remove docker \
  docker-common \
  container-selinux \
  docker-selinux \
  docker-engine \
  docker-engine-selinux
```

安裝一些必要套件：

```bash
sudo yum install -y yum-utils \
  device-mapper-persistent-data lvm2
```

新增 Docker 官方的 stable 套件庫（repository），縱使您想要安裝 edge 版本的 Docker，也要先新增這一個套件庫：

```bash
sudo yum-config-manager \
  --add-repo \
  https://download.docker.com/linux/centos/docker-ce.repo
```

`docker.repo` 中也同時包含 edge 版本的 Docker 套件庫，不過預設是被停用的，想安裝 edge 版本的話，就要先啟用 edge 版本的套件庫（如果只是要安裝 stable 版本的人，就可以省略這一步）：

```bash
sudo yum-config-manager --enable docker-ce-edge
```

更新 yum 的套件索引：

```bash
sudo yum makecache fast
```

安裝 Docker CE 版：

```bash
sudo yum install docker-ce
```

第一次安裝 Docker 的時候，會需要匯入 GPG 的金鑰，Docker CE 版的金鑰指紋是 `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`，確認無誤就選擇 `y` 匯入。

[![匯入 GPG 金鑰](https://blog.gtwang.org/wp-content/uploads/2017/06/centos-linux-7-install-docker-tutorial-20170623-1.png)](https://blog.gtwang.org/wp-content/uploads/2017/06/centos-linux-7-install-docker-tutorial-20170623-1.png)

匯入 GPG 金鑰

安裝好之後，啟動系統的 Docker 服務：

```bash
sudo systemctl start docker
```

執行 hello world 程式測試：

```bash
sudo docker run hello-world
```

正常來說，輸出會像這樣：

[![Docker 的 Hello World 測試](https://blog.gtwang.org/wp-content/uploads/2017/06/centos-linux-7-install-docker-tutorial-20170623-2.png)](https://blog.gtwang.org/wp-content/uploads/2017/06/centos-linux-7-install-docker-tutorial-20170623-2.png)

Docker 的 Hello World 測試

這樣就完成 CentOS Linux 7 上面的 Docker 環境安裝了。

如果想要使用一般使用者帳號來執行 Docker，要先將該帳號加入 `docker` 群組：

```bash
# 將帳號加入 docker 群組
sudo usermod -aG docker USERNAME
```

參考資料：[Docker](https://docs.docker.com/engine/installation/linux/centos/)