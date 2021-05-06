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
```

*Tips*

Setup alias

```sh
# Add to your shell. ( e.g. ~/.bashrc )
alias sail="./vendor/bin/sail"
```

Customize your container settings.

```sh
$ ./vendor/bin/sail artisan sail:publish
Copied Directory [/vendor/laravel/sail/runtimes] To [/docker]
Publishing complete.
```
