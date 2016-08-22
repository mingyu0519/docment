# Redis 数据库学习

## 安装(mac)

- 使用homebrew安装，命令是：`brew install redis`。
- 安装完成后启动命令：`brew services start redis`。
- 使用命令`redis-cli`进入redis进行下一步的操作。

## Redis 的数据类型
### String 类型
string是redis最基本的数据类型，一个key对应一个value，一个key最大能存储512m。
```
127.0.0.1:6379> SET name "xiaoming"
OK
127.0.0.1:6379> GET name
"xiaoming"
127.0.0.1:6379>
```

### Hash
redis的hash是键值对集合。
```
127.0.0.1:6379> HMSET user:1 username xiaohong password xiaohong sex gril
OK
127.0.0.1:6379> HGETALL user
(empty list or set)
127.0.0.1:6379> HGETALL user:1
 1) "username"
 2) "xiaohong"
 3) "passworld"
 4) "w3cshool.cc"
 5) "points"
 6) "200"
 7) "password"
 8) "xiaohong"
 9) "sex"
10) "gril"
```

### List 
redis是简单的字符串列表，按照插入的顺序排序。
```
127.0.0.1:6379> lpush xiaoming class
(integer) 1
127.0.0.1:6379> lpush xiaoming class1
(integer) 2
127.0.0.1:6379> lpush xiaoming class2
(integer) 3
127.0.0.1:6379> lpush xiaoming class2
(integer) 4
127.0.0.1:6379> lpush xiaoming class2
(integer) 5
127.0.0.1:6379> LRANGE xiaoming 0 1
1) "class2"
2) "class2"
127.0.0.1:6379> LRANGE xiaoming 0 10
1) "class2"
2) "class2"
3) "class2"
4) "class1"
5) "class"
```

### Set 集合
Redis的set是string的无序集合。
#### sadd命令
添加一个string元素到key对应的set集合中去，成功则返回1，元素存在返回0，key对应的set不存在则返回错误。
```
127.0.0.1:6379> sadd xiaoming class
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> sadd xiaoming class1
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> sadd xiaoming1 class
(integer) 1
127.0.0.1:6379> sadd xiaoming1 class1
(integer) 1
127.0.0.1:6379> sadd xiaoming1 class2
(integer) 1
127.0.0.1:6379> sadd xiaoming1 class2
(integer) 0
127.0.0.1:6379> SMEMBERS xiaoming
(error) WRONGTYPE Operation against a key holding the wrong kind of value
127.0.0.1:6379> SMEMBERS xiaoming1
1) "class1"
2) "class"
3) "class2"
```

### zset 有序集合
zset和set一样，也是string类型的元素集合，并且是不允许重复的成员。不同的是zset会为每一个成员设置一个score分数，zset集合的顺序也是通过这个score进行排序的。zset元素是唯一的，但是score是可以重复的。
```
127.0.0.1:6379> ZADD xiaoming2 0.1 class1
(integer) 1
127.0.0.1:6379> ZADD xiaoming2 0.2 class2
(integer) 1
127.0.0.1:6379> ZADD xiaoming2 0.09 class3
(integer) 1
127.0.0.1:6379> ZADD xiaoming2 0.09 class3
(integer) 0
127.0.0.1:6379> ZRANGEBYSCORE xiaoming2 0 100
1) "class3"
2) "class1"
3) "class2"
```

## Redis 命令
Redis是通过bash命令后管理的，基本命令是`redis-cli`.

在远程服务上登录redis，命令是`redis-cli -h host -p port -a password`

## Redis 事务
Redis事务可以一次执行多个命令，并且带有以下两个重要保证：
1. 事务是一个单独的隔离操作：事务中的所有命令都会序列化，按顺序的执行。事务在执行过程中，不会被其他的客户端发送的命令请求所打断。
2. 事务是一个原子操作：要么都执行，要么就都不执行。

```
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> SET book "nodejs 21 days"
QUEUED
127.0.0.1:6379> GET book
QUEUED
127.0.0.1:6379> SADD tag "nodejs" "21"
QUEUED
127.0.0.1:6379> SMEMBERS tag
QUEUED
127.0.0.1:6379> EXEC
1) OK
2) "nodejs 21 days"
3) (integer) 2
4) 1) "21"
   2) "nodejs"
```





















