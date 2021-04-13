# Docker-compose-laravel-alpine-for-mac-m1
A pretty simplified Docker Compose workflow that sets up a LNMP network of containers for local Laravel development on Mac M1.

- `docker_php 130MB`
- `redis 32.3MB`
- `nginx 20.5MB`
- `mysql/mysql-server 436MB`

## Guide

To get started, make sure you have [Docker installed](https://docs.docker.com/docker-for-mac/install/) on your system, and then clone this repository.

Next, navigate in your terminal to the directory you cloned this, and spin up the containers for the web server by running :
- `docker-compose up -d --build`

Without buiding,just run:
- `docker-compose up -d`

Other useful command:
- `docker-compose down`
- `docker-compose stop php`
- `docker-compose start php`
- `docker exec -it php sh`

After that completes, follow the steps from the [src/README.md](src/README.md) file to get your Laravel project added in (or create a new blank one).

The following are built for our web server, with their exposed ports detailed:

- **nginx:stable-alpine** - `:80`
- **mysql/mysql-server:latest-aarch64** - `:3306`
- **php:7.4.16-fpm-alpine3.13** - `:9000`
- **redis:alpine** - `:6379`

## PHP 

- `docker exec -w /var/www/your-site-root php composer install`
- `docker exec -w /var/www/your-site-root php php artisan serve`
- `docker exec -w /var/www/your-site-root php artisan migrate` 
  
#### Extensions

- `bcmath`
- `Core`
- `ctype`
- `curl`
- `date`
- `dom`
- `fileinfo`
- `filter`
- `ftp`
- `gd`
- `hash`
- `iconv`
- `imap`
- `intl`
- `json`
- `libxml`
- `mbstring`
- `memcached`
- `mysqlnd`
- `openssl`
- `pcntl`
- `pcre`
- `PDO`
- `pdo_mysql`
- `pdo_sqlite`
- `Phar`
- `posix`
- `readline`
- `redis`
- `Reflection`
- `session`
- `SimpleXML`
- `sodium`
- `SPL`
- `sqlite3`
- `standard`
- `tidy`
- `tokenizer`
- `xml`
- `xmlreader`
- `xmlwriter`
- `Zend OPcache`
- `zip`
- `zlib`

## MySQL
Mysql Client: Default password :12345678 
```
docker exec -it mysql mysql -uroot -p
```
Mysql Data Storage:
```
volumes:
  - mysqldata:/var/lib/mysql     
```

Starting with MySQL 8 you no longer can (implicitly) create a user using the GRANT command. Use CREATE USER instead, followed by the GRANT statement:

```
mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'PASSWORD';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;
```
**Caution:** about the security risks about WITH GRANT OPTION, see:
[Grant all privileges on database](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_all)

Now, you can use any MySQL Client program to connect with MySQL Server,running on the container.
```
HOST:127.0.0.1
PORT:3306
USER:root
PASSWORD:12345678 
```

## Redis
Redis Client: Default password :(empty) 
```
docker exec -it redis redis-cli
```
Redis Data Storage
```
volumes:
  - redisdata:/var/lib/mysql     
```

