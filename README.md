# 使用技術
![Laravel](https://img.shields.io/badge/Laravel-11-brightgreen.svg)
![PHP](https://img.shields.io/badge/PHP-8.3-blue.svg)
![MariaDB](https://img.shields.io/badge/MySQL-8.0.33-blue.svg)
![nginx](https://img.shields.io/badge/nginx-1.27-blue.svg)
![Docker](https://img.shields.io/badge/Docker-26.1.4-blue.svg)
![docker-compose](https://img.shields.io/badge/docker--compose-2.27.1-blue.svg)


# Laravelプロジェクトの作成方法

1. **cloneする**  
   プロジェクトのコピーを自分のコンピュータにダウンロードします。
   ```
   git clone git@github.com:arlo-engineer/php83-nginx-mysql.git <ディレクトリ名>
   ```

2. **docker composeで立ち上げる**  
   ダウンロードしたプロジェクトを使って、必要なプログラム（コンテナと呼ばれる）を自動的に起動します。
   ```
   cd <ディレクトリ名>
   docker compose up -d
   ```

3. **phpコンテナに入る**  
   起動したプログラムの中の一つ、PHPを使う部分にアクセスします。
   ```
   docker exec -it myapp-php bash
   ```

4. **サブディレクトリにlaravelをインストールする**  
   PHPを使って、Laravelをインストールします。
   ```
   composer create-project "laravel/laravel=11.*" myapp --prefer-dist
   ```
   プロジェクトフォルダの中身を移動し、一時ディレクトリを削除
   ```
   mv myapp/* ./
   mv myapp/.* ./
   rm -r myapp
   ```
5. **実行テストする**
   ブラウザで http://localhost にアクセスし、Laravelのwelcomeページが表示されることを確認する。

# DBとの接続方法
1. Laravelプロジェクト内の.envファイルを以下のように変更する(変更する値はLaravelプロジェクト外の.envファイルを参照する)
   ```:.env
   DB_CONNECTION=mysql
   DB_HOST=db # docker-compose.ymlのmysqlのコンテナ名
   DB_PORT=3306
   DB_DATABASE=developmen
   DB_ROOT_PASSWORD=root
   DB_USERNAME=mysql
   DB_PASSWORD=mysql
   ```
3. 以下を実行し、DBとの接続をする
   ```
   php artisan migrate
   ```
# リポジトリを変更してpushする方法
以下記事を参考にすることで、GitHubリポジトリを変更してpushすることができる

[GitHubからクローンしたリポジトリを別リポジトリにプッシュしたい](https://k-koh.hatenablog.com/entry/2020/10/09/154644)

# 参考文献
- [Laravel10の開発環境をdockerで実現する方法](https://qiita.com/hitotch/items/869070c3a9f474a358ea)
- [【最新保存版】Laravel入門基礎マスター講座【初心者もゼロから学習】](https://youtu.be/SXjrlVs5Tnk?si=Dmr5qMVMMF33_ejB)

