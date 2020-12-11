# VPCエンドポイントを使ってEC2からS3にアクセス
## EC2を作成
### インスタンスを作成
- publicとprivateでインスタンスを2つ作成
- セキュリティグループを設定
  - public: httpとhttpsとsshをフルオープン
  - private: http、https、ssh、mysql、publicのプライベートipのみでオープン

### publicサーバ
- publicサーバにapachをインストールしてhtmlファイルを適当に配置
- ブラウザからipアドレスにアクセスして確認

### privateサーバ
- publicサーバから疎通確認
- ssh接続
- public側にNATgatewayを作成
- privateのルートテーブルにnatgatewayを定義
- privateサーバでyum updateしてmysqlをインストール

### ロールの作成
- S3fullアクセスのロールを作成
- privateサーバにアタッチ

### S3のバケット作成
- パブリックアクセスは全部閉じておく

### NATGatewayからS3へのアクセス
- privateサーバへssh(publicを通して)
- aws s3 ls でS3へのアクセス確認

### NATGatewayを外すと…
- NATGatewayをルートテーブルから外してみて、アクセスができないことを確認
- aws s3 lsが使えない(インターネット接続してないから！)

### VPCエンドポイントを作成
- VPCページからエンドポイントを作成
- 自分のリージョン内のS3gatewayを選択
- VPCに紐付け
- private側のルートテーブルに紐付け
- ルートテーブルに反映されていることを確認

### アクセス確認
- privateサーバからaws s3 ls --region ap-northeast-1 でバケット名を確認
