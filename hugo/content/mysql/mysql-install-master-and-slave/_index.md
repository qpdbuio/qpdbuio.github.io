+++
title = "MySQL主从"
description = "_index.md"
weight = 1
+++

### 下载安装包
{{< code >}}
mysql-5.7.38-1.el7.x86_64.rpm-bundle.tar
ncurses-c++-libs-6.1-9.20180224.el8.x86_64.rpm
ncurses-compat-libs-6.1-9.20180224.el8.x86_64.rpm
mysql-community-libs-5.7.38-1.el7.x86_64.rpm
mysql-community-common-5.7.38-1.el7.x86_64.rpm
mysql-community-client-5.7.38-1.el7.x86_64.rpm
mysql-community-server-5.7.38-1.el7.x86_64.rpm
{{< /code >}}

### master配置: my.cnf
{{< code >}}
server_id = 1
log-bin = mysql-bin
binlog_format = row
binlog-ignore-db=mysql
binlog-ignore-db=sys
binlog-ignore-db=information_schema
binlog-ignore-db=performance_schema
#binlog-do-db = esteel
auto_increment_offset = 1
auto_increment_increment = 1
log_bin_trust_function_creators = 1

max_connections = 2000

#innodb_buffer_pool_size = 128M
#join_buffer_size = 128M
#sort_buffer_size = 2M
#read_rnd_buffer_size = 2M

lower_case_table_names=1

innodb_buffer_pool_size = 5G
innodb_buffer_pool_instances = 8
innodb_buffer_pool_load_at_startup = 1
innodb_buffer_pool_dump_at_shutdown = 1
innodb_lru_scan_depth = 2000
innodb_io_capacity = 4000
innodb_io_capacity_max = 8000
innodb_print_all_deadlocks = 1
innodb_sort_buffer_size = 16M


character_set_server=utf8mb4
#skip-grant-tables

#是否开启慢查询日志，1表示开启，0表示关闭。
slow-query-log = 1

#MySQL数据库慢查询日志存储路径
#slow-query-log-file = var/lib/mysql/localhost-slow.log

#慢查询阈值，当查询时间多于设定的阈值时，记录日志。单位:秒
long_query_time = 1

gtid_mode = on
enforce_gtid_consistency = 1
log_slave_updates = 1
relay_log_recovery = 1
skip_slave_start = 1
sync_binlog=1

#rpl_semi_sync_master_enabled=1
#rpl_semi_sync_master_timeout=10000  # 10秒（默认）

relay_log_purge=0

expire_logs_days = 15
{{< /code >}}

### slave配置: my.cnf
{{< code >}}
server_id = 2
log-bin = mysql-bin
binlog_format = row
relay-log = relay-log
read-only = 1

max_connections = 2000

#innodb_buffer_pool_size = 128M
#join_buffer_size = 128M
#sort_buffer_size = 2M
#read_rnd_buffer_size = 2M

lower_case_table_names=1

innodb_buffer_pool_size = 5G
innodb_buffer_pool_instances = 8
innodb_buffer_pool_load_at_startup = 1
innodb_buffer_pool_dump_at_shutdown = 1
innodb_lru_scan_depth = 2000
innodb_io_capacity = 4000
innodb_io_capacity_max = 8000
innodb_print_all_deadlocks = 1
innodb_sort_buffer_size = 16M


character_set_server=utf8mb4
#skip-grant-tables

#是否开启慢查询日志，1表示开启，0表示关闭。
slow-query-log = 1

#MySQL数据库慢查询日志存储路径
#slow-query-log-file = var/lib/mysql/localhost-slow.log

#慢查询阈值，当查询时间多于设定的阈值时，记录日志。单位:秒
long_query_time = 1

gtid_mode = on
enforce_gtid_consistency = 1
log_slave_updates = 1
relay_log_recovery = 1
skip_slave_start = 1
sync_binlog=1

#rpl_semi_sync_slave_enabled=1

relay_log_purge=0

expire_logs_days = 15
{{< /code >}}

### master执行
{{< code >}}
SET PASSWORD = PASSWORD('Q!w2e3r4');
grant all privileges on *.* to 'root'@'%' identified by 'Q!w2e3r4' with grant option;
GRANT REPLICATION SLAVE ON *.* TO 'root'@'%' identified by 'Q!w2e3r4';
FLUSH PRIVILEGES;
{{< /code >}}

### slave执行
{{< code >}}
CHANGE MASTER TO
MASTER_HOST='10.11.90.124',
MASTER_PORT=3306,
MASTER_USER='root',
MASTER_PASSWORD='Q!w2e3r4',
MASTER_LOG_FILE='mysql-bin.000007',
MASTER_LOG_POS=623;
{{< /code >}}

### 查看状态
{{< code >}}
show master status;
show slave status;
start master;
start slave;
{{< /code >}}
