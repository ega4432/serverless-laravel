# Serverless Laravel

[ English | [Japanese](https://github.com/ysmtegsr/serverless-laravel/blob/main/README.ja.md) ]

This is a sample repository of Serverless LAMP stack.

Learn more about what a serverless lamp stack is, see this AWS blog.

[Introducing the new Serverless LAMP stack \| AWS Compute Blog](https://aws.amazon.com/jp/blogs/compute/introducing-the-new-serverless-lamp-stack/)

## Setup

Build the Laravel environment on docker in our local machine with [Laravel Sail](https://readouble.com/laravel/8.x/ja/sail.html).

```sh
$ curl -s "https://laravel.build/serverless-laravel" | bash

# If you have a component you want, add queries like this:
# curl -s "https://laravel.build/serverless-laravel?with=mysql,redis" | bash
```

## Build containers

```sh
$ ./vendor/bin/sail up -d
```

## Setup serverless

Install the Bref libraries to build serverless Laravel.

Bref is a great way to easily create an environment where you can run your Laravel application on AWS Lambda.

[Serverless Laravel applications \- Bref](https://bref.sh/docs/frameworks/laravel.html)

```sh
# Add libraries
$ ./vendor/bin/sail composer require bref/bref bref/laravel-bridge

# Copy yaml of serverless.
$ ./vendor/bin/sail artisan vendor:publish --tag=serverless-config
```


## Local development

Described how to execute commands that you will often use.

```sh
# migrate
$ ./vendor/bin/sail artisan migrate

# seed
$ ./vendor/bin/sail artisan db:seed

# install npm packages
$ ./vendor/bin/sail npm i
```

Customize your container settings.

```sh
$ ./vendor/bin/sail artisan sail:publish
Copied Directory [/vendor/laravel/sail/runtimes] To [/docker]
Publishing complete.
```

Connect your local database.

```sh
# Set variables
$ MYSQL_USER=<user_name>
$ MYSQL_PASSWORD=<password>
$ MYSQL_DATABASE=<database_name>

$ docker-compose exec mysql bash -c 'mysql -u${MYSQL_USER} -p${MYSQL_PASSWORD} ${MYSQL_DATABASE}'
```

*Tips*

Add aliases to your shell. ( e.g. ~/.bashrc )

```sh
# sail
alias sail="./vendor/bin/sail"

# bref
alias bref="./vendor/bin/bref"
```

## Deployment

### Dynamic application

Bia [severless framework CLI](https://www.serverless.com/framework/docs/providers/aws/cli-reference/) to AWS Cloud.
If you want to migrate or seed, use the [Bref CLI](https://bref.sh/docs/runtimes/console.html).

```sh
$ seleverless deploy -v
# or
$ sls deploy -v

# migrate
$ ./vendor/bin/bref cli <artisan function> --region <region> -- migrate

# seed
$ ./vendor/bin/bref cli <artisan function> --region <region> -- db:seed
```

### Static assets

Bia [AWS CLI](https://aws.amazon.com/cli/) to S3 bucket.

```sh
$ ./vendor/bin/sail npm run prod

$ aws s3 sync public/ s3://<bucket-name>/ --delete --exclude index.php
```
