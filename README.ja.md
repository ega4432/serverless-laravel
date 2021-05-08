# Serverless Laravel

[![automatic release](https://github.com/ysmtegsr/serverless-laravel/actions/workflows/release.yml/badge.svg)](https://github.com/ysmtegsr/serverless-laravel/actions/workflows/release.yml)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/ysmtegsr/serverless-laravel)
![GitHub](https://img.shields.io/github/license/ysmtegsr/serverless-laravel)

[ [English](https://github.com/ysmtegsr/serverless-laravel) | Japanese ]

サーバーレス LAMP スタックのサンプルリポジトリです。

サーバーレス LAMP スタックについて詳しく知りたいという方は下記の AWS ブログの記事を参照ください。

[新しいサーバーレス LAMP スタック – Part 1: 概要紹介 \| Amazon Web Services ブログ](https://aws.amazon.com/jp/blogs/news/introducing-the-new-serverless-lamp-stack/)

## セットアップ

[Laravel Sail](https://readouble.com/laravel/8.x/ja/sail.html) を使って環境構築を行います。Sail は、ローカルマシンで Laravel アプリケーションを開発するための Docker 環境を簡単に作ることができます。

```sh
$ curl -s "https://laravel.build/serverless-laravel" | bash

# If you have a component you want, add queries like this:
# curl -s "https://laravel.build/serverless-laravel?with=mysql,redis" | bash
```

## コンテナのビルド

```sh
$ ./vendor/bin/sail up -d
```

## サーバーレスのセットアップ

サーバーレス Laravel を構築する上で Bref ライブラリをインストールします。

Bref というのは、Laravel アプリケーションを AWS Lambda 上で実行する環境を簡単に作流ことができる優れものです。

[Serverless Laravel applications \- Bref](https://bref.sh/docs/frameworks/laravel.html)

```sh
# Add libraries
$ ./vendor/bin/sail composer require bref/bref bref/laravel-bridge

# Copy yaml of serverless.
$ ./vendor/bin/sail artisan vendor:publish --tag=serverless-config
```


## ローカルでの開発

よく使うであろうコマンドの実行方法を記載しました。

```sh
# migrate
$ ./vendor/bin/sail artisan migrate

# seed
$ ./vendor/bin/sail artisan db:seed

# install npm packages
$ ./vendor/bin/sail npm i
```

環境変数用の yml ファイルを serverless framework のスタック名でコピーします。

```sh
$ cp ./serverless-config/env/stack.yml.example ./serverless-config/env/<your stack>.yml
```

Docker コンテナをカスタマイズしたくなったら、下記のように publish して Dockerfile を編集します。

```sh
$ ./vendor/bin/sail artisan sail:publish
Copied Directory [/vendor/laravel/sail/runtimes] To [/docker]
Publishing complete.
```

ローカル DB を直接 SQL を実行したい場合は、コンテナに入って確認することができます。（もちろん tinker を使う方法もあります。）

```sh
# Set variables
$ MYSQL_USER=<user_name>
$ MYSQL_PASSWORD=<password>
$ MYSQL_DATABASE=<database_name>

$ docker-compose exec mysql bash -c 'mysql -u${MYSQL_USER} -p${MYSQL_PASSWORD} ${MYSQL_DATABASE}'
```

*Tips*

何度も使うコマンドは、エイリアスを設定しておくと良いでしょう。

```sh
# sail
alias sail="./vendor/bin/sail"

# bref
alias bref="./vendor/bin/bref"
```

## デプロイ

### 動的アプリケーション

[Serverless Framework CLI](https://www.serverless.com/framework/docs/providers/aws/cli-reference/) を使ってデプロイします。

マイグレーションやシードを行いたい場合は、[Bref CLI](https://bref.sh/docs/runtimes/console.html) を使って行ます。

```sh
$ seleverless deploy -v
# or
$ sls deploy -v

# migrate
$ ./vendor/bin/bref cli <artisan function> --region <region> -- migrate

# seed
$ ./vendor/bin/bref cli <artisan function> --region <region> -- db:seed
```

### 静的アセット

[AWS CLI](https://aws.amazon.com/jp/cli/) を使って、S3 バケットにデプロイします。

```sh
$ ./vendor/bin/sail npm run prod

$ aws s3 sync public/ s3://<bucket-name>/ --delete --exclude index.php
```
