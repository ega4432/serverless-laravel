# Serverless Laravel

This is a sample repository of Serverless LAMP environment.

## Setup

Build the Laravel environment on docker in our local machine with sail.

```sh
$ curl -s "https://laravel.build/sail-sample" | bash

# If you have a component you want, add queries like this:
# curl -s "https://laravel.build/sail-sample?with=mysql,redis" | bash
```

## Build containers

```sh
$ ./vendor/bin/sail up -d
```

## Setup serverless

```sh
# Add libraries
$ ./vendor/bin/sail composer require bref/bref bref/laravel-bridge

# Copy yaml of serverless.
$ ./vendor/bin/sail artisan vendor:publish --tag=serverless-config
```


## Local development

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

Bia severless framework CLI to AWS Cloud.

```sh
$ seleverless deploy -v
# or
$ sls deploy -v
```

### Static assets

```sh
$ ./vendor/bin/sail npm run prod

$ aws s3 sync public/ s3://<bucket-name>/ --delete --exclude index.php
```
