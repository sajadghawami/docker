# Official phpMyAdmin Docker image

Run phpMyAdmin with Alpine, supervisor, nginx and PHP FPM.

[![Build Status](https://travis-ci.org/phpmyadmin/docker.svg?branch=master)](https://travis-ci.org/phpmyadmin/docker)
[![Docker Pulls](https://img.shields.io/docker/pulls/phpmyadmin/phpmyadmin.svg)][hub]
[![Docker Stars](https://img.shields.io/docker/stars/phpmyadmin/phpmyadmin.svg)][hub]
[![Docker Layers](https://images.microbadger.com/badges/image/phpmyadmin/phpmyadmin.svg)](https://microbadger.com/images/phpmyadmin/phpmyadmin "Get your own image badge on microbadger.com")
[![Docker Version](https://images.microbadger.com/badges/version/phpmyadmin/phpmyadmin.svg)](https://microbadger.com/images/phpmyadmin/phpmyadmin "Get your own version badge on microbadger.com")


All following examples will bring you phpMyAdmin on `http://localhost:8080`
where you can enjoy your happy MySQL administration.

## Credentials

phpMyAdmin does use MySQL server credential, please check the corresponding
server image for information how it is setup.

The official MySQL and MariaDB use following environment variables to define these:

* `MYSQL_ROOT_PASSWORD` - This variable is mandatory and specifies the password that will be set for the `root` superuser account.
* `MYSQL_USER`, `MYSQL_PASSWORD` - These variables are optional, used in conjunction to create a new user and to set that user's password.

## Docker hub tags

You can use following tags on Docker hub:

* `latest` - latest stable release
* `4.8` - latest stable release for the 4.8 version
* `4.8.5` - specific patch release for the 4.8 version

For each tag the following variants are provided:

### apache

This starts an Apache webserver with PHP, so you can use phpMyAdmin out of the box.

### fpm-alpine

This image has a very small footprint. It is based on Alpine Linux and starts only a PHP FPM process. Use this variant if you already have a seperate webserver. If you need more tools, that are not available on Alpine Linux, use the fpm image instead.

### fpm

This image starts only a PHP FPM container. Use this variant if you already have a seperate webserver.

## Usage with linked server

First you need to run MySQL or MariaDB server in Docker, and this image need
link a running mysql instance container:

```
docker run --name myadmin -d --link mysql_db_server:db -p 8080:80 phpmyadmin/phpmyadmin
```

## Usage with external server

You can specify MySQL host in the `PMA_HOST` environment variable. You can also
use `PMA_PORT` to specify port of the server in case it's not the default one:

```
docker run --name myadmin -d -e PMA_HOST=dbhost -p 8080:80 phpmyadmin/phpmyadmin
```

## Usage with arbitrary server

You can use arbitrary servers by adding ENV variable `PMA_ARBITRARY=1` to the startup command:

```
docker run --name myadmin -d -e PMA_ARBITRARY=1 -p 8080:80 phpmyadmin/phpmyadmin
```

## Usage with docker-compose and arbitrary server

This will run phpMyAdmin with arbitrary server - allowing you to specify MySQL/MariaDB
server on login page.

Using the docker-compose.yml from https://github.com/phpmyadmin/docker

```
docker-compose up -d
```

## Run the E2E tests with docker-compose

You can run the E2E tests with the local test environment by running MariaDB/MySQL databases. Adding ENV variable `PHPMYADMIN_RUN_TEST=true` already added on docker-compose file. Simply run:

Using the docker-compose.testing.yml from https://github.com/phpmyadmin/docker

```
docker-compose -f docker-compose.testing.yml up phpmyadmin
```

## Adding Custom Configuration
### For phpmyadmin

You can add your own custom config.inc.php settings (such as Configuration Storage setup) 
by creating a file named "config.user.inc.php" with the various user defined settings
in it, and then linking it into the container using:

```
-v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php
```
On the "docker run" line like this:
``` 
docker run --name myadmin -d --link mysql_db_server:db -p 8080:80 -v /some/local/directory/config.user.inc.php:/etc/phpmyadmin/config.user.inc.php phpmyadmin/phpmyadmin
```

See the following links for config file information.
https://docs.phpmyadmin.net/en/latest/config.html#config
https://docs.phpmyadmin.net/en/latest/setup.html

### For PHP

You can add your own custom php.ini settings (such as max_upload_size)
by creating a file named "php.ini" with the various user defined settings
in it, and then linking it into the container using:

```
-v /some/local/directory/php.ini:/usr/local/etc/php/conf.d/php.ini
```
On the "docker run" line like this:
``` 
docker run --name myadmin -d --link mysql_db_server:db -p 8080:80 -v /some/local/directory/php.ini:/usr/local/etc/php/conf.d/php.ini phpmyadmin/phpmyadmin
```

## Usage behind reverse proxys

Set the variable ``PMA_ABSOLUTE_URI`` to the fully-qualified path (``https://pma.example.net/``) where the reverse proxy makes phpMyAdmin available.

## Environment variables summary

* ``PMA_ARBITRARY`` - when set to 1 connection to the arbitrary server will be allowed
* ``PMA_HOST`` - define address/host name of the MySQL server
* ``PMA_VERBOSE`` - define verbose name of the MySQL server
* ``PMA_PORT`` - define port of the MySQL server
* ``PMA_HOSTS`` - define comma separated list of address/host names of the MySQL servers
* ``PMA_VERBOSES`` - define comma separated list of verbose names of the MySQL servers
* ``PMA_PORTS`` -  define comma separated list of ports of the MySQL servers
* ``PMA_USER`` and ``PMA_PASSWORD`` - define username to use for config authentication method
* ``PMA_ABSOLUTE_URI`` - define user-facing URI

For usage with Docker secrets, appending ``_FILE`` to any environment variable is allowed:
```
docker run --name myadmin -d -e PMA_PASSWORD_FILE=/run/secrets/db_password.txt -p 8080:80 phpmyadmin/phpmyadmin
```

For more detailed documentation see https://docs.phpmyadmin.net/en/latest/setup.html#installing-using-docker

[hub]: https://hub.docker.com/r/phpmyadmin/phpmyadmin/

Please report any issues with the Docker container to https://github.com/phpmyadmin/docker/issues
