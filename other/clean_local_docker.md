# ローカルのdocker環境の掃除
https://docs.docker.jp/config/pruning.html
## 注意
- `docker container prune` は使わない
  * 使用されていないコンテンが削除されてしまうため
- `docker system prune` も同様

## 全体の手順
1. 不要なコンテナを削除
  * docker-compose run や up でできた不要なコンテナや、使わないプロジェクト
2. コンテナに紐付かない不要なイメージを削除
3. コンテナに紐付かない不要なボリュームを削除

## コンテナ
- `docker ps -a`
  * コンテナの一覧を確認
- `docker container prune --filter "xxxxxx"`
  * fileterオプションでよしなに消せそう(未検証)
- だが、コンテナを個別で削除するのが安全
  * docker rm [コンテナID or コンテナ名]

## イメージ
- `docker images`
  * イメージの一覧を確認
- どのコンテナーからも参照されていないイメージを含めてすべてを削除する
  * `docker image prune -a`
  * rubyやnodeなどアプリケーションのコンテナを作る元になるイメージも削除される
  * コンテナが紐付いていればimagesは消えない
  * docker-compose up の時に自動的にbuildされる
- イメージを個別で削除する
  * docker rmi [イメージID]

## ボリューム
- `docker volume ls`
  * ボリュームの一覧を確認
- どのコンテナーからも参照されていないイメージを含めてすべてを削除する
  * `docker volume prune`

## ネットワーク
- `docker network ls`
  * ネットワークの一覧を確認
- `docker network prune`
  * 必要なネットワークも消えた
- ディスクスペースを取るものではないらしい
- その他は未調査