# alpineproject
This repository contains a Docker Compose configuration for a simple setup with Alpine, MySQL, and Redis.

## Alpine
*Alpine Linux is a Linux distribution built around musl libc and BusyBox. The image is only 5 MB in size and has access to a package repository that is much more complete than other BusyBox based images. This makes Alpine Linux a great image base for utilities and even production applications. Read more about Alpine Linux here and you can see how their mantra fits in right at home with Docker images.*

[Docker hub link for alpine](https://hub.docker.com/_/alpine)

## Mysql
MySQL is the world's most popular open source database. With its proven performance, reliability and ease-of-use, MySQL has become the leading database choice for web-based applications, covering the entire range from personal projects and websites, via e-commerce and information services, all the way to high profile web properties including Facebook, Twitter, YouTube, Yahoo! and many more.

For more information and related downloads for MySQL Server and other MySQL products, please visit:

[Docker hub link for mysql](www.mysql.com)

## Redis
Redis is an open-source, networked, in-memory, key-value data store with optional durability. It is written in ANSI C. The development of Redis is sponsored by Redis Labs today; before that, it was sponsored by Pivotal and VMware. According to the monthly ranking by DB-Engines.com, Redis is the most popular key-value store. The name Redis means REmote DIctionary Server.

[Docker hub link for redis](wikipedia.org/wiki/Redis)

### Docker Compose Details

The alpine service is:
```
alpine:
    image: alpine
    container_name: alpine
    volumes:
      - dbdata:/home/marina/db
    user: root 
    command: sh -c "mkdir -p /home/marina/db && tail -f /dev/null"
    networks:
      - vlab
    depends_on:
      - redis
      - db
    deploy:
      resources:
        limits:
          memory: 512M
```
The mysql service is:
```
db:
    image: mysql
    container_name: db
    environment:
      - MYSQL_ROOT_PASSWORD=secret
      - MYSQL_DATABASE=mydb
      - MYSQL_USER=myuser
      - MYSQL_PASSWORD=mypassword
      - REDIS_HOST=db
      - REDIS_PORT=6379
      - REDIS_PASSWORD=eYVX7EwVmmxKPCDmwMtyKVge8oLd2t81 
    volumes:
      - dbdata:/var/lib/mysql
    networks:
      - vlab
    depends_on:
      - redis
    deploy:
      resources:
        limits:
          memory: 1G
```
The redis service is:
```

          
