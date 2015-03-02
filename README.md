# Scala ActiveRecord integration test

Required Docker version 1.5 or greater.

## Prepare

```
$ docker build -t scala-activerecord-app-base -f Dockerfile.base .
```

## MySQL 5.6

```sh
$ mkdir -p /tmp/mysql/data
$ docker run --name mysql-server -e MYSQL_ROOT_PASSWORD=password -v /tmp/mysql/data:/var/lib/mysql -d mysql
$ docker exec -it mysql-server bash -c "echo 'CREATE DATABASE sampledb DEFAULT CHARACTER SET utf8;' | mysql -uroot -ppassword"
$ docker run -it -p 9000:9000 --link mysql-server:db scala-activerecord-app-base bash -c \
  'cd /opt/apps/scala-activerecord-sample && \
   bin/sbt -Ddb.activerecord.url="jdbc:mysql://${DB_PORT_3306_TCP_ADDR}:${DB_PORT_3306_TCP_PORT}/sampledb" \
  -Ddb.activerecord.driver=com.mysql.jdbc.Driver \
  -Ddb.activerecord.user=root \
  -Ddb.activerecord.password=password \
  run'
```

## PostgreSQL 9.4

```sh
$ mkdir -p /tmp/postgresql/data
$ docker run --name postgresql-server -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=password -v /tmp/postgresql/data:/var/lib/postgresql/ -d postgres:9.4
$ docker exec -it postgresql-server bash -c "su - postgres -c 'createdb sampledb --locale=C --encoding=UTF8 --template=template0'"
$ docker run -it -p 9000:9000 --link postgresql-server:db scala-activerecord-app-base bash -c \
  'cd /opt/apps/scala-activerecord-sample && \
   bin/sbt -Ddb.activerecord.url="jdbc:postgresql://${DB_PORT_5432_TCP_ADDR}:${DB_PORT_5432_TCP_PORT}/sampledb" \
  -Ddb.activerecord.driver=org.postgresql.Driver \
  -Ddb.activerecord.user=postgres \
  -Ddb.activerecord.password=password \
  run'
```

## Oracle 11g Release 2 (11.2)

```sh
$ docker run --name oracle-server -d wnameless/oracle-xe-11g
$ docker run -it -p 9000:9000 --link oracle-server:db scala-activerecord-app-base bash -c \
  'cd /opt/apps/scala-activerecord-sample && \
   rm -rf play2x/conf/evolutions && \
   bin/sbt -Ddb.activerecord.url="jdbc:oracle:thin:@${DB_PORT_1521_TCP_ADDR}:${DB_PORT_1521_TCP_PORT}:XE" \
  -Ddb.activerecord.driver=oracle.jdbc.OracleDriver \
  -Ddb.activerecord.user=system \
  -Ddb.activerecord.password=oracle \
  run'
```
