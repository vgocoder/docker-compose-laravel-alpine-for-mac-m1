# Docker-compose-laravel-alpine-for-mac-m1
A pretty simplified Docker Compose workflow that sets up a LNMP network of containers for local Laravel development on Mac M1.

> You can modify the version of the images in the file "docker-compose.yml" or the Dockfile for the specific needs.

The following are built for our web server, with their exposed ports detailed:
- **nginx:stable-alpine** - `:80`
- **mysql/mysql-server:latest-aarch64** - `:3306`
- **php:7.4.16-fpm-alpine3.13** - `:9000`
- **redis:alpine** - `:6379`

The images are minimized as much as possible:

- `docker_php 130MB`
- `redis 32.3MB`
- `nginx 20.5MB`
- `mysql/mysql-server 436MB`

## **Prerequisites**

To get started, make sure you have [Docker installed](https://docs.docker.com/docker-for-mac/install/) on your system, and then clone this repository.

For the user of Mac M1, please follow [ Docker Desktop for Apple Silicon](https://docs.docker.com/docker-for-mac/apple-m1/)

## **Quick start**

### Step 1 - Spin up the containers for the web server

> Navigate in your terminal to the directory you cloned this, and spin up the containers for the web server by running :

- `docker-compose up -d --build`

#### **Optional:**
> up without building,just run:
- `docker-compose up -d`
> Stops containers and removes containers, networks, volumes, and images created by up . By default, the only things removed are: Containers for services defined in the Compose file.
- `docker-compose down`
> Stop the container,"php" is the name of the container
- `docker-compose stop php`
> Start the containers,"php" is the name of the container
- `docker-compose start php`


### **Step 2 - Get your Laravel project added in**
follow the steps from the [wwwroot/README.md](wwwroot/README.md) file to get your Laravel project added in (or create a new blank one).

## **The usage of the project**
### **PHP Service (composer included)**
- `Accessing the Container Shell`
    
  ```
  docker exec -it php sh
  ```
- `The mapped directory  the service container for php`
  ```
    volumes:
      - ./wwwroot:/var/www
      - ./php/php.ini:/usr/local/etc/php/php.ini
  ```
- `Composer install`
  ```
  docker exec -w /var/www/your-site-root php composer install
  ```
- `Laravel: php artisan`
  ```
  docker exec -w /var/www/your-site-root php php artisan serve
  docker exec -w /var/www/your-site-root php artisan migrate
  ```
  
#### **The list of php extensions in the project**
- `bcmath`
- `gd`
- `imap`
- `intl`
- `memcached`
- `mysqlnd`
- `pcntl`
- `pdo_mysql`
- `redis`
- `tidy`
- `Zend OPcache`
- `zip`
- `zlib`

### **Nginx Service**
- `Accessing the Container Shell`
  ```
    docker exec -it nginx  sh
  ```
- `The mapped directory  the service container for nginx`
  ```
    volumes:
      - ./wwwroot:/var/www
      - ./nginx/conf.d/:/etc/nginx/conf.d/
      - ./nginx/log:/var/log/nginx
  ```

### **MySQL Service**
- `Mysql Client: Default password :12345678`
  ```
  docker exec -it mysql mysql -uroot -p
  ```
- `The mapped directory  the service container for mysql`
  ```
  volumes:
      - mysqldata:/var/lib/mysql      
      - ./mysql/my.cnf:/etc/mysql/my.cnf 
  ```
- `Starting with MySQL 8 you no longer can (implicitly) create a user using the GRANT command. Use CREATE USER instead, followed by the GRANT statement`

  ```
  mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'PASSWORD';
  mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
  mysql> FLUSH PRIVILEGES;
  ```
  **Caution:** about the security risks about WITH GRANT OPTION, see:
[Grant all privileges on database](https://dev.mysql.com/doc/refman/8.0/en/privileges-provided.html#priv_all)

- `Now, you can use any MySQL Client program to connect with MySQL Server,running on the container`
  ```
  HOST:127.0.0.1
  PORT:3306
  USER:root
  PASSWORD:12345678 
  ```

### **Redis Service**
- `Redis Client: Default password :(empty) `
  ```
  docker exec -it redis redis-cli
  ```
- `The mapped directory  the service container for redis`
  ```
  volumes:
    - redisdata:/data
    - ./redis/conf:/usr/local/etc/redis
    - ./redis/log:/log   
  ```

