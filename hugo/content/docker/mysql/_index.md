+++
title = "安装mysql"
description = "_index.md"
weight = 4
+++

### 已创建docker-net
### docker-compose.yml
{{< code >}}
version: '3'
services:
  mysql:
    restart: always
    image: mysql:5.7.29
    container_name: mysql
    ports:
      - "3306:3306"
    environment:
      - "TZ=Asia/Shanghai"
    volumes:
      - /opt/mysql:/var/lib/mysql
      - /opt/mysql/my.cnf:/etc/my.cnf
      - /opt/mysql/log/mysqld.log:/var/log/mysqld.log
    networks:
      - docker-net

networks:
  docker-net:
    external: true
{{< /code >}}

### my.cnf
{{< code >}}
[mysqld]
datadir =/var/lib/mysql
port = 3306
sync_binlog	= 0
max_binlog_size=524288000

character_set_server=UTF8MB4
sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION'

max_connections=2000
max_connect_errors=100
max_length_for_sort_data=2048

tmp_table_size=256M
max_heap_table_size=256M
max_allowed_packet=128M

innodb_buffer_pool_instances=1
innodb_buffer_pool_chunk_size=128M
innodb_buffer_pool_size=256M
innodb_log_buffer_size=256M
innodb_log_file_size=256M
innodb_io_capacity=20000
innodb_io_capacity_max=40000

read_rnd_buffer_size=8M
read_buffer_size=8M
sort_buffer_size=8M
join_buffer_size=8M
key_buffer_size=8M
innodb_sort_buffer_size=8M
myisam_sort_buffer_size=8M
bulk_insert_buffer_size=8M

[mysql]
default-character-set=UTF8MB4

[client]
default-character-set=UTF8MB4
{{< /code >}}