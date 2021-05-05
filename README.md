# Serverless Laravel

This is a sample repository of Serverless LAMP environment.


## Setup

Build the Laravel environment on Docker in our local machine with sail.

```sh
$ curl -s "https://laravel.build/sail-sample" | bash

# If you have a component you want, add queries like this:
# curl -s "https://laravel.build/sail-sample?with=mysql,redis" | bash
```

## Build containers

```sh
$ ./vendor/bin/sail up -d
```


*Tips*

```sh
# Setup alias
alias sail="./vendor/bin/sail"
```

