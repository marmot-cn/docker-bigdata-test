# docker-bigdata-test

## Install

```
docker-compose up -d
```

## Testing

```
docker-compose exec hive-server bash
/opt/hive/bin/beeline -u jdbc:hive2://localhost:10000
CREATE TABLE pokes (foo INT, bar STRING);
LOAD DATA LOCAL INPATH '/opt/hive/examples/files/kv1.txt' OVERWRITE INTO TABLE pokes;

select * from pokes;
```

## Hive 导入数据到MySQL

我们把`MySQL`的`help_keyword`导入到`hive`的`sqoop_test`库

#### 测试连通

```
sqoop list-databases \
--connect jdbc:mysql://数据库地址:3306/ \
--username root \
--password xxxx
```

#### hive 创建测试库`sqoop_test`

```
/opt/hive/bin/beeline -u jdbc:hive2://localhost:10000

hive>  SHOW DATABASES;
hive>  CREATE DATABASE sqoop_test;
```

#### 导入

```
sqoop import --connect jdbc:mysql://数据库地址:3306/mysql --username 账户 --password 密码 --table help_keyword --delete-target-dir --target-dir /sqoop_hive  --hive-database sqoop_test --hive-import --hive-overwrite -m 3
```