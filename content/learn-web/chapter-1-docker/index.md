+++
title = "Dockerの導入"
date = "2022-02-25T09:08:28+09:00"
draft = false
+++

# docker、docker-composeを導入

ここでは、開発のために自分の手元のPCでサーバを立ち上げるために、`docker`と`docker-compose`というソフトウェアを導入します。

## 手順: Dockerの導入

- [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)を参考に、WSL2の上でコマンドを打ってdockerをインストールします。
- 確認のため、インストールを終えたら `sudo docker ps` とターミナルで打ってみてください。以下のように表示されたらうまくいっています。

```none
$ sudo docker ps
[sudo] password for uta8a: 
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## トラブルシューティング: もしエラーが起きたら？

- ここは非常にエラーが色々な種類で出るので、エラーメッセージで検索してみて、5分考えて分からなければサークルのdiscordで質問してみましょう。

## 手順: docker-composeの導入

- [docker/compose](https://github.com/docker/compose)からReleasesの`docker-compose-linux-x86_64`を取ってきます。アドレスをコピーしてWSL側でダウンロードしましょう。

```none
# URLは最新のものにする
wget https://github.com/docker/compose/releases/download/v2.1.1/docker-compose-linux-x86_64
sudo cp ./docker-compose-linux-x86_64 /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

- versionコマンドで確認をします。以下のように表示されれば成功です。

```none
$ sudo docker-compose -v
Docker Compose version v2.0.1
```

#### feedback

フィードバックのリンクは [こちら](https://forms.gle/Yd7nN2FK5tTYP6Xp8)

