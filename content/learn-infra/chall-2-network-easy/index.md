+++
title = "問題2 docker network"
date = "2022-03-10T20:13:18+09:00"
draft = false
+++

# 問題2 docker network

# 問題概要

Dockerコンテナからpingができないので、それを直す問題。

Dockerのネットワークの3種類について理解しているかを問う。

# 解答

主に [Docker公式 Disable networking for a container](https://docs.docker.com/network/none/) が参考になる。

## 調査段階

```none
$ sudo docker inspect easy-net | grep Net
            "NetworkMode": "none",
```

から、NetworkMode noneだとわかるので、bridgeに直せばよい。

## 修正段階

```none
$ sudo docker network disconnect none easy-net
$ sudo docker network connect bridge easy-net
$ sudo docker exec easy-net ping -3 example.com
PING example.com (x.x.x.x): 56 data bytes
64 bytes from x.x.x.x: seq=0 ttl=54 time=y ms
64 bytes from x.x.x.x: seq=1 ttl=54 time=y ms
```

# 問題環境の解説

DockerでIPv6 Only networkを作ろうとして、作れなかったので生まれた問題。

## none + (host | bridge)

```none
$ sudo docker network connect bridge no-net-alpine
Error response from daemon: container cannot be connected to multiple networks with one of the networks in private (none) mode
$ sudo docker network connect host no-net-alpine
Error response from daemon: container cannot be disconnected from host network or connected to host network
```

怒られるので、一度disconnectする。
