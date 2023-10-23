# docker composeの使い方

## 管理対象コンテナを起動
```sh
$ cd ansible-linux-docker/docker
$ docker compose up -d target
$ docker compose ps
NAME              IMAGE           COMMAND                       SERVICE   CREATED          STATUS          PORTS
docker-target-1   docker-target   "/bin/sh -c \"/sbin/init\""   target    26 seconds ago   Up 25 seconds   22/tcp
```

## コンテナのAnsibleを実行する
基本的にdocker comose run --rm でタスク実行をする。

### インベントリ設定の確認

○インベントリファイルのフォーマット確認
```sh
$ docker compose run --rm ansible ansible-inventory -i /ansible/hosts.yml --list --yaml
```

○インベントリで定義されているの管理対象ホストの確認
```sh
$ docker compose run --rm ansible ansible target -i /ansible/hosts.yml -m debug -a "msg=test"
```

○Ansibleホスト->管理対象ホストの疎通確認
```sh
$ docker compose run --rm ansible ansible target -i /ansible/hosts.yml -m ping -u root
```



