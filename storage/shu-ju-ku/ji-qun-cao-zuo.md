# 集群操作

```sql
select * from system.clusters;

CREATE DATABASE
```

```sql
SELECT * FROM system.zookeeper WHERE path = '/clickhouse'
SELECT * FROM system.zookeeper WHERE path = '/clickhouse'
```

```sql
SELECT table, create_time, command, is_done FROM system.mutations ORDER BY create_time DESC
```

```text
./zkCli.sh -server 127.0.0.1:2181
./zkCli.sh -server 127.0.0.1:2181 connect


```

update 限制

[https://www.altinity.com/blog/2018/10/16/updates-in-clickhouse](https://www.altinity.com/blog/2018/10/16/updates-in-clickhouse)

