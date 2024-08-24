# 使用技術
![Laravel](https://img.shields.io/badge/Laravel-10-brightgreen.svg)
![PHP](https://img.shields.io/badge/PHP-8-blue.svg)
![MariaDB](https://img.shields.io/badge/MariaDB-10.4-blue.svg)
![nginx](https://img.shields.io/badge/nginx-1.18-blue.svg)
![Docker](https://img.shields.io/badge/Docker-20.10-blue.svg)
![docker-compose](https://img.shields.io/badge/docker--compose-1.29-blue.svg)


# Laravelプロジェクトの作成方法

1. **cloneする。**  
   プロジェクトのコピーを自分のコンピュータにダウンロードします。
   ```
   git clone https://github.com/arlo-engineer/php81-nginx-mariadb.git <ディレクトリ名>
   ```

2. **docker composeで立ち上げる。**  
   ダウンロードしたプロジェクトを使って、必要なプログラム（コンテナと呼ばれる）を自動的に起動します。
   ```
   cd php81-nginx-mariadb
   docker compose up -d
   ```

3. **phpコンテナに入る**  
   起動したプログラムの中の一つ、PHPを使う部分にアクセスします。
   ```
   docker exec -it myapp-php bash
   ```

4. **laravelをインストール**  
   PHPを使って、Laravelというツールをセットアップ（インストール）します。
   ```
   composer create-project --prefer-dist laravel/laravel my-app
   ```

5. **phpコンテナから出る**  
   Laravelのセットアップが終わったら、PHPの部分を終了します。
   ```
   exit
   ```

6. **docker-compose.ymlを編集する**  
   設定ファイル（docker-compose.yml）を変更して、プロジェクトの設定を更新します。以下のように`volumes`セクションを編集してください。
   ```diff
     web: 
    
       volumes:
   -      - .:/var/www/
   +      - ./my-app:/var/www/

     nginx: 
    
       volumes:
   -      - .:/var/www/
   +      - ./my-app:/var/www/
    
   ```

7. **再度docker composeで立ち上げる**  
   更新した設定で、もう一度プログラムを起動します。
   ```
   docker compose up -d
   ```
# DBとの接続方法
1. Laravelプロジェクト内の.envファイルを以下のように変更する(変更する値はLaravelプロジェクト外の.envファイルを参照する)
   ```:.env
   DB_CONNECTION=mysql
   DB_HOST=[docker-compose.ymlのmariadbコンテナ名]
   DB_PORT=3306
   DB_DATABASE=development
   DB_USERNAME=mysql
   DB_PASSWORD=mysql
   ```
3. 以下を実行し、DBとの接続をする
   ```
   php artisan migrate
   ```
# リポジトリを変更してpushする方法
以下記事を参考にすることで、GitHubリポジトリを変更してpushすることができる

https://k-koh.hatenablog.com/entry/2020/10/09/154644
