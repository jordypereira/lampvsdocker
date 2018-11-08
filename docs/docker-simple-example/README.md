# Simple site in Docker

## Install php in docker

So we will create a docker service that runs a php image. We can choose an image from the [dockerhub](https://hub.docker.com/_/php/).

Create a project folder and create a docker-compose.yml file inside it.

```yaml
version: '3'
services:
  php-apache:
    image: php:7.2.1-apache
    ports:
      - 80:80
    volumes:
      - ./DocumentRoot:/var/www/html
    links:
      - 'mariadb'
```

This file will be used by docker-compose to start our service. The file we will be showing is a simple `<?php phpinfo(); ?>` in `project/DocumentRoot/index.php`

Ofcourse we need a database and the links tag ensures that it won't start before 'mariadb' has started.

## PHP-extensions

We need to provide these ourselves. Thus we will be making a Dockerfile.

in `php-apache/Dockerfile` we insert the following:

```dockerfile
FROM php:7.2.1-apache
MAINTAINER egidio docile
RUN docker-php-ext-install pdo pdo_mysql mysqli
```

This takes the base image and runs a command fron the image, installing our extensions. Now we set up our docker-compose file to use this instead of the image.

So replace `image` with

```yaml
build:
  context: ./php-apache
```

## The Database service

We add another service to our docker-compose file:

```yaml
mariadb:
  image: mariadb:10.1
  volumes:
    - mariadb:/var/lib/mysql
  environment:
    TZ: 'Europe/Brussels'
    MYSQL_ALLOW_EMPTY_PASSWORD: 'no'
    MYSQL_ROOT_PASSWORD: 'rootpwd'
    MYSQL_USER: 'testuser'
    MYSQL_PASSWORD: 'testpassword'
    MYSQL_DATABASE: 'testdb'
```

The environment variables available are listed [here](https://hub.docker.com/_/mariadb/)

Next to services we have to specify a volume:

```yaml
volumes: mariadb:
```

To ensure that the data persists.

## Run our container

`docker-compose up`
When we navigate to localhost (Or the IP of your server) we can see the phpinfo file we made earlier.

## So what if we want to host multiple sites?

Reverse-proxy container.. Or let someone else do the hard work and use [devilbox](../)
