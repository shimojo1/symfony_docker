# clone
git clone https://github.com/shimojo1/symfony_docker.git

# 移動
cd symfony_docker

# Docker構築
docker compose build --no-cache
docker compose up -d

# ここから
# プロジェクト作成
cd src
docker exec -it php-symfony composer create-project symfony/website-skeleton symfony 4.3.x
なんか聞かれたらy とEnterを入力(2回くらい)

# 所有ユーザを変更する(ローカルで編集するため)
sudo chown ubuntu:ubuntu -R symfony


# symfonyディレクトリに移動して.envファイルをコピー
cd symfony
cp .env .env.local

# DB接続情報を記述する
vi .env.local

# 以下部分をこのように書き換える
DATABASE_URL="mysql:#root:root@db-symfony:3306/symfony?serverVersion=16&charset=utf8"

http://localhost
これで開いて画面が表示されていればOK

# バージョン確認
docker exec --workdir /var/www/html/symfony -it php-symfony php bin/console --version



参考:【初心者向け】初めてのSymfony4
https:#tech.quartetcom.co.jp/2019/12/18/hello-symfony/

ここからはSymfonyのメモ

# コントローラ作成
docker exec --workdir /var/www/html/symfony -it php-symfony bin/console make:controller FormController

# composer.jsonのmaker-bundleのバージョンを修正(なんかmake:entityでエラー出るので)
        "symfony/maker-bundle": "1.33.*",

# 終わったらcomposerをアップデート
docker exec --workdir /var/www/html/symfony -it php-symfony composer update

# データベース作成
docker exec --workdir /var/www/html/symfony -it php-symfony php bin/console doctrine:database:create;
# エンティティ作成（対話型コマンド）
docker exec --workdir /var/www/html/symfony -it php-symfony php bin/console make:entity

# マイグレーションファイル作成
docker exec --workdir /var/www/html/symfony -it php-symfony php bin/console make:migration
# マイグレーションファイル実行
docker exec --workdir /var/www/html/symfony -it php-symfony php bin/console doctrine:migrations:migrate
