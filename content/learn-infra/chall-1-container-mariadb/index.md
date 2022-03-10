+++
title = "問題1 コンテナとMariaDB"
date = "2022-03-10T13:42:31+09:00"
draft = false
+++

# 問題1 コンテナとMariaDB

[問題リンク](https://github.com/uta8a/learn-infra.code/tree/main/container-mariadb)

# 問題概要

Dockerコンテナが落ちるので、それを直す問題。

コンテナはDBなので、volumeにも気を付けてデータが見えるようにする必要がある。

# 解答

主に [ICTSC公式の解説](https://blog.icttoracon.net/2020/03/01/%E7%94%9F%E3%81%8D%E8%BF%94%E3%82%8Cmariadb/) が参考になる。

## 調査段階

```none
$ sudo docker ps -a
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS                     PORTS     NAMES
dbcda45e71c7   mariadb:latest   "docker-entrypoint.s…"   8 seconds ago   Exited (1) 7 seconds ago             mariaDB
$ sudo docker logs mariaDB
2022-03-10 04:47:56+00:00 [Note] [Entrypoint]: Entrypoint script for MariaDB Server 1:10.7.3+maria~focal started.
2022-03-10 04:47:56+00:00 [ERROR] [Entrypoint]: mariadbd failed while attempting to check config
	command was: mariadbd --verbose --help --log-bin-index=/tmp/tmp.3aDO1OstcQ
$ sudo docker inspect mariaDB
[
    {
        "Id": "dbcda45e71c7ef4739c5a4a04d5665d89c29e2cf872dabea3fa1cc7f762c19d6",
        "Created": "2022-03-10T04:47:56.503193766Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "mariadbd"
        ],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": true,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 1,
            "Error": "",
            "StartedAt": "2022-03-10T04:47:56.789855967Z",
            "FinishedAt": "2022-03-10T04:47:56.834720686Z"
        },
...
```

このあたりはまずみる。exitedなので落ちている。DB立ち上げただけで落ちるのは何かおかしいし `exec ... bash` もできないので詰んでいる。メモリ不足のようなパワー不足を疑う。

inspectでDockerの初期設定自体を調べる。

```none
"Memory": 6291456,
```

6Mのメモリ。これは少ないのでこれではないかと当たりをつける。

## 修正段階

```none
$ sudo docker update --memory 1G mariaDB
mariaDB
Your kernel does not support swap limit capabilities or the cgroup is not mounted. Memory limited without swap.
$ sudo docker start mariaDB
mariaDB
$ sudo docker exec -it mariaDB bash
### コンテナの中に入った
root@dbcda45e71c7:/# mysql -u root -pMariaPass
### MariaDBの中に入った
MariaDB [(none)]> use chall;
MariaDB [chall]> show tables;
+-------------------+
| Tables_in_chall   |
+-------------------+
| container_mariadb |
+-------------------+
MariaDB [chall]> select * from container_mariadb;
+----+------------+-----------+-----+-------+
| ID | First_Name | Last_Name | Age | Sex   |
+----+------------+-----------+-----+-------+
|  1 | Tarou      | Yamada    |  23 | MAN   |
|  2 | Emi        | Uchiyama  |  23 | WOMAN |
|  3 | Ryo        | Sato      |  25 | MAN   |
|  4 | Yuki       | Tayama    |  22 | WOMAN |
|  5 | Yuto       | Takahashi |  21 | MAN   |
+----+------------+-----------+-----+-------+
5 rows in set (0.001 sec)
```

port 3306が空いていることを確認する。

```none
### VMから
$ mysql -h 127.0.0.1 -u root -pMariaPass
### MariaDBの中に入った
MariaDB [(none)]> use chall;
MariaDB [chall]> select * from container_mariadb;
+----+------------+-----------+-----+-------+
| ID | First_Name | Last_Name | Age | Sex   |
+----+------------+-----------+-----+-------+
|  1 | Tarou      | Yamada    |  23 | MAN   |
|  2 | Emi        | Uchiyama  |  23 | WOMAN |
|  3 | Ryo        | Sato      |  25 | MAN   |
|  4 | Yuki       | Tayama    |  22 | WOMAN |
|  5 | Yuto       | Takahashi |  21 | MAN   |
+----+------------+-----------+-----+-------+
```

これで確認できた。完了。

# 問題環境の解説

VMはmultipassを使って立てています。disk容量を多めに取っておくといい。

環境構築はansibleを用いています。

## mariaVOLはどこにあるのか？

パス指定しない、即ち dot `.` やslash `/` から始まらないパスはNamed Volumeとして扱われ、 `/var/lib/docker/volumes/<name>/` に実体が置かれる。

## Named Volume + `../../../` はどうなる？

```none
$ sudo docker run  -v mariaVOL/../../../../../../../../hoge:/var/lib/mysql -d --name mariaDB2 -e MYSQL_ROOT_PASSWORD=MariaPass -p 3306:3306 -d mariadb:latest
docker: Error response from daemon: create mariaVOL/../../../../../../../../hoge: "mariaVOL/../../../../../../../../hoge" includes invalid characters for a local volume name, only "[a-zA-Z0-9][a-zA-Z0-9_.-]" are allowed. If you intended to pass a host directory, use absolute path.
See 'docker run --help'.
```

きっちり怒られる

## AnsibleでなぜかDocker上のMariaDBに繋がらない

未解決

手元から以下のようにつなげるが、ansibleで同じことをしようとすると無理。(`ERROR 2013 (HY000): Lost connection to MySQL server at 'handshake: reading initial communication packet', system error: 11` と言われる。TCPなどの他の接続方法を試したりmy.cnfを書き換えてもダメだった)

```
$ mysql -h 127.0.0.1 -u root -pMariaPass
```

結局、Dockerコンテナに `docker cp` でsqlファイルを移動させ、コンテナ内で実行した


## AnsibleでDocker内でmysqlコマンドが使えない

`docker exec ... mysql ...` のようにコンテナ内で実行しようとしても、socketにつなげないというエラーが出る。

これはdb waitをすることで解決した。具体的には、シェルスクリプトで30secタイムアウトでnetcatで接続試行を行い、接続できたらmysqlコマンドを発行する。
