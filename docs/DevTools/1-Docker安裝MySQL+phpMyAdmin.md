# 1- Docker 安裝MariaDB + phpMyAdmin





#### Step 1: 创建网络

```bash
docker network create \
  --driver=bridge \
  --subnet=10.10.10.0/24 \
  my-network
```



#### Step 2:  my.cnf

```bash
[mysql]
#设置mysql客户端默认字符集
default-character-set=utf8mb4
socket=/var/run/mysql/mysql.sock

[mysqld]
# skip-grant-tables
#mysql5.7以后的不兼容问题处理
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
datadir=/var/lib/mysql
socket=/var/run/mysql/mysql.sock

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemdd
#允许最大连接数
max_connections=200
#服务端使用的字符集默认为utf8mb4
character-set-server=utf8mb4
collation-server = utf8mb4_general_ci
#创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
lower_case_table_names=1
max_allowed_packet=16M
#设置时区
default-time_zone='+8:00'

[mysqld_safe]
log-error=/var/log/mariadb/mariadb.log
pid-file=/var/run/mariadb/mariadb.pid
```



#### Step 3: 



```
  db:
    image: mysql:5.7
    container_name: db
    privileged: true
    restart: always
    ports:
      - "3306:3306"
    expose:
      - "3306"
    environment:
      MYSQL_ROOT_PASSWORD: "root" #输入自己的密码
      MYSQL_DATABASE: "lyadmin_db"
      TZ: Asia/Shanghai
    command:
      --wait_timeout=31536000
      --interactive_timeout=31536000
      --max_connections=1000
      --default-authentication-plugin=mysql_native_password
    volumes:
      - ./docker_env/mysql/data:/var/lib/mysql
      - ./docker_env/mysql/conf.d:/etc/mysql/conf.d"
      - ./docker_env/mysql/logs:/logs
    networks:
      network:
        ipv4_address: 10.10.10.13
```





```
sudo docker run -p 3306:3306 \
--name db \
--restart always \
--privileged \
-v ~/mysql/conf:/etc/mysql/conf.d \
-v ~/mysql/logs:/var/log \
-v ~/mysql/data:/var/lib/mysql \
-e MARIADB_USER=example-user \
-e MARIADB_PASSWORD=my_cool_secret \
-e MARIADB_ROOT_PASSWORD=my-secret-pw \
-e TZ:Asia/Shanghai \
-d mariadb:latest



sudo docker run --name phpmyadmin -d \
--restart always \
--link db \
-p 80:80 \
phpmyadmin/phpmyadmin




# --network my-network \
# --ip="10.10.10.13" \
# --expose 3306 \
-e MYSQL_DATABASE: "lyadmin_db" \
-v ~/mysql/conf:/etc/mysql/conf.d \

```





```

/Users/qzheng/Documents/mysql_db



sudo 

docker run -p 3306:3306 \
--name db \
--restart always \
--privileged \
-v /Users/qzheng/Documents/mysql_db/conf:/etc/mysql/conf.d \
-v /Users/qzheng/Documents/mysql_db/logs:/var/log \
-v /Users/qzheng/Documents/mysql_db/data:/var/lib/mysql \
-e MARIADB_USER=example-user \
-e MARIADB_PASSWORD=my_cool_secret \
-e MARIADB_ROOT_PASSWORD=my-secret-pw \
-e TZ:Asia/Shanghai \
-d mariadb:latest



docker run -p 6379:6379 \
--name redis \
--restart always \
--privileged \
-v /Users/qzheng/Documents/redis_db/redis/data:/data \
-v /Users/qzheng/Documents/redis_db/redis.conf:/etc/redis/redis.conf \
-d redis:latest

```











```
sudo docker run -p 3306:3306 \
--name db \
--restart always \
--privileged \
-v /home/mysql/conf:/etc/mysql/conf.d \
-v /home/mysql/logs:/var/log \
-v /home/mysql/data:/var/lib/mysql \
-e MARIADB_USER=example-user \
-e MARIADB_PASSWORD=my_cool_secret \
-e MARIADB_ROOT_PASSWORD=my-secret-pw \
-e TZ:Asia/Shanghai \
-d mariadb:latest
```



