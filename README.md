# Ansibleコンテナ

## 管理対象コンテナを起動
```sh
$ cd ansible-linux-docker
$ docker compose up -d target
$ docker compose ps
```

## コンテナ内のAnsibleを実行する
docker comose run --rm ansible <Ansibleのコマンド>　でタスク実行をする。

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

### プレイブックを適用する


#### 1.[/controller/ansible/playbook/roles](/controller/ansible/playbook/roles)配下に<Role名>/tasks/main.ymlでRoleを作成

#### 2.[site.yml](controller/ansible/playbook/site.yml)を編集しRoleを読み込む

```yml
- hosts: target
  roles:
    - your_role_name # ここに追加
```

#### 3.ansible-playbookコマンドを実行
```sh
$ docker compose run --rm ansible ansible-playbook /ansible/playbook/site.yml -i /ansible/hosts.yml -u root
```

実行結果例
```
PLAY [target] *****************************************************************************************************************************
ok: [target]

TASK [test : echo test] *******************************************************************************************************************
changed: [target]

TASK [yum : install apache] ***************************************************************************************************************
changed: [target]

PLAY RECAP ********************************************************************************************************************************
target                     : ok=3    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```


