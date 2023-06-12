# alpineproject
This repository contains a Docker Compose configuration for a simple setup with Alpine, MySQL, and Redis.

## Alpine
*Alpine Linux is a Linux distribution built around musl libc and BusyBox. The image is only 5 MB in size and has access to a package repository that is much more complete than other BusyBox based images. This makes Alpine Linux a great image base for utilities and even production applications. Read more about Alpine Linux here and you can see how their mantra fits in right at home with Docker images.*

[Docker hub link for alpine](https://hub.docker.com/_/alpine)

## Mysql
*MySQL is the world's most popular open source database. With its proven performance, reliability and ease-of-use, MySQL has become the leading database choice for web-based applications, covering the entire range from personal projects and websites, via e-commerce and information services, all the way to high profile web properties including Facebook, Twitter, YouTube, Yahoo! and many more.*

*For more information and related downloads for MySQL Server and other MySQL products, please visit:*

[Docker hub link for mysql](www.mysql.com)

## Redis
*Redis is an open-source, networked, in-memory, key-value data store with optional durability. It is written in ANSI C. The development of Redis is sponsored by Redis Labs today; before that, it was sponsored by Pivotal and VMware. According to the monthly ranking by DB-Engines.com, Redis is the most popular key-value store. The name Redis means REmote DIctionary Server.*

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
*This service uses the Alpine Linux base image (alpine) and creates a container named alpine. It has a single volume (dbdata) mounted at /home/marina/db inside the container. The user is set to root, and the command is set to sh -c "mkdir -p /home/marina/db && tail -f /dev/null", which creates the directory /home/marina/db and keeps the container running. It depends on the redis and db services and is connected to the vlab network. The container is deployed with a memory limit of 512 MB.*

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
*This service uses the MySQL image (mysql) and creates a container named db. It sets several environment variables to configure the MySQL instance, including MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, and MYSQL_PASSWORD. It also sets REDIS_HOST, REDIS_PORT, and REDIS_PASSWORD to configure the connection to the redis service. The container has a volume (dbdata) mounted at /var/lib/mysql to persist the MySQL data. It depends on the redis service and is connected to the vlab network. The container is deployed with a memory limit of 1 GB.*

The redis service is:
```
redis:
    image: redis
    restart: always
    ports:
      - 6379:6379
    command: sh -c "redis-server --save 20 1 --loglevel warning --requirepass eYVX7EwVmmxKPCDmwMtyKVge8oLd2t81"
    volumes: 
      - cache:/data
    networks:
      - vlab
    deploy:
      resources:
        limits:
          memory: 256M
```
*This service uses the Redis image (redis) and creates a container named redis. It specifies restart: always to ensure the container is always restarted if it fails. It publishes port 6379 on the host, mapping it to the same port in the container. The command runs redis-server with various options, including setting a password with --requirepass and configuring the save interval and policy. It has a volume (cache) mounted at /data to persist the Redis data. The container is connected to the vlab network and is deployed with a memory limit of 256 MB.*

To create volume dbdata,cache and vlab network:
```
networks:
  vlab:

volumes:
  dbdata:
  cache:
    driver: local
```
Networks:
The project defines a single network named vlab, which is used by all services.

Volumes:
The project defines two volumes: dbdata and cache. dbdata is used by the alpine and db services, while cache is used by the redis service. Both volumes are set to use the local driver.

### Instalation guide
See `INSTALL.md` file
