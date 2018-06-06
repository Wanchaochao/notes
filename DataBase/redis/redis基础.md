#### 密码

```php
链接redis:
redis-cli
设置密码：
config set requirepass 密码
查看是否生效:
ping # (error) NOAUTH Authentication required.
使用密码验证:
auth 密码 # OK
再次验证:
ping # pong

清空密码:
config set requirepass ""
```


#### Clients 客户端

```php
获取client名称
client getname
(nil) #默认是没有name的

设置名字
client setname my-connection
OKz
client getname
my-connection

清除名字
client setname ""

clients列表:
clients list

杀死client:
kill ip:port  # 例 kill 127.0.0.1:43051

```

#### config

config get parameter
```php

config get save
1) "save"
2) "900 1 300 10 60 10000" #900s内一次，300秒内10次，60秒内10000次就会触发save

config get s* #s开头的配置参数和配置值
config get * # 所有的

config resetstat #重置info命令中的某些统计数据

CONFIG SET parameter value #可以动态地调整 Redis 服务器的配置(configuration)而无须重启。

config rewrite #将服务器当前所使用的配置记录到 redis.conf 文件中


```

#### debug

```php

DEBUG OBJECT mykey #DEBUG OBJECT 是一个调试命令，它不应被客户端所使用
Value at:0x7fd892d03420 refcount:2147483647 encoding:int serializedlength:2 lru:1460396 lru_seconds_idle:70212


DEBUG SEGFAULT
执行一个不合法的内存访问从而让 Redis 崩溃，仅在开发时用于 BUG 模拟。

```

#### FLUSHALL

```php
清空整个 Redis 服务器的数据(删除所有数据库的所有 key )。
此命令从不失败。
总是返回 OK 。

redis> DBSIZE            # 0 号数据库的 key 数量
(integer) 9

redis> SELECT 1          # 切换到 1 号数据库
OK

redis[1]> DBSIZE         # 1 号数据库的 key 数量
(integer) 6

redis[1]> flushall       # 清空所有数据库的所有 key
OK

redis[1]> DBSIZE         # 不但 1 号数据库被清空了
(integer) 0

redis[1]> SELECT 0       # 0 号数据库(以及其他所有数据库)也一样
OK

redis> DBSIZE
(integer) 0


```

#### FLUSHDB

```php
清空当前数据库中的所有 key。
redis> DBSIZE    # 清空前的 key 数量
(integer) 4

redis> FLUSHDB
OK

redis> DBSIZE    # 清空后的 key 数量
(integer) 0
```

#### MONITOR

实时监听当前redis接收到的命令

#### SHUTDOWN

SHUTDOWN 命令执行以下操作

* 停止所有客户端
* 如果有至少一个保存点在等待，执行 SAVE 命令
* 如果 AOF 选项被打开，更新 AOF 文件
* 关闭 redis 服务器(server)

如果持久化被打开的话， SHUTDOWN 命令会保证服务器正常关闭而不丢失任何数据。

另一方面，假如只是单纯地执行 SAVE 命令，然后再执行 QUIT 命令，则没有这一保证 —— 因为在执行 SAVE 之后、执行 QUIT 之前的这段时间中间，其他客户端可能正在和服务器进行通讯，这时如果执行 QUIT 就会造成数据丢失。

##### SAVE 和 NOSAVE 修饰符

通过使用可选的修饰符，可以修改 SHUTDOWN 命令的表现。比如说：

* 执行 SHUTDOWN SAVE 会强制让数据库执行保存操作，即使没有设定(configure)保存点
* 执行 SHUTDOWN NOSAVE 会阻止数据库执行保存操作，即使已经设定有一个或多个保存点(你可以将这一用法看作是强制停止服务器的一个假想的 ABORT 命令)

#### SLAVEOF

** SLAVEOF host port **

SLAVEOF 命令用于在 Redis 运行时**动态**地修改复制(replication)功能的行为。

通过执行 SLAVEOF host port 命令，可以将当前服务器转变为指定服务器的从属服务器(slave server)。

如果当前服务器已经是某个主服务器(master server)的从属服务器，那么执行 SLAVEOF host port 将使当前服务器停止对旧主服务器的同步，丢弃旧数据集，转而开始对新主服务器进行同步。

另外，对一个从属服务器执行命令 SLAVEOF NO ONE 将使得这个从属服务器关闭复制功能，并从从属服务器转变回主服务器，原来同步所得的数据集**不会被丢弃**。

利用『 SLAVEOF NO ONE **不会丢弃同步所得数据集**』这个特性，可以在主服务器失败的时候，将从属服务器用作新的主服务器，从而实现无间断运行。


#### SLOWLOG

Slow log 是 Redis 用来记录查询执行时间的日志系统。

查询执行时间指的是不包括像客户端响应(talking)、发送回复等 IO 操作，而单单是执行一个查询命令所耗费的时间。

另外，slow log 保存在内存里面，读写速度非常快，因此你可以放心地使用它，不必担心因为开启 slow log 而损害 Redis 的速度。


##### 设置 SLOWLOG

Slow log 的行为由两个配置参数(configuration parameter)指定，可以通过改写 redis.conf 文件或者用 CONFIG GET 和 CONFIG SET 命令对它们动态地进行修改。

第一个选项是 slowlog-log-slower-than ，它决定要对执行时间大于多少微秒(microsecond，1秒 = 1,000,000 微秒)的查询进行记录。

比如执行以下命令将让 slow log 记录所有查询时间大于等于 100 微秒的查询：

CONFIG SET slowlog-log-slower-than 100

另一个选项是 slowlog-max-len ，它决定 slow log 最多能保存多少条日志， slow log 本身是一个 FIFO 队列，当队列大小超过 slowlog-max-len 时，最旧的一条日志将被删除，而最新的一条日志加入到 slow log ，以此类推。

以下命令让 slow log 最多保存 1000 条日志：

CONFIG SET slowlog-max-len 1000

使用 CONFIG GET 命令可以查询两个选项的当前值：

slowlog get  #查看所有慢日志

```php
redis> SLOWLOG GET
1) 1) (integer) 12                      # 唯一性(unique)的日志标识符
   2) (integer) 1324097834              # 被记录命令的执行时间点，以 UNIX 时间戳格式表示
   3) (integer) 16                      # 查询执行时间，以微秒为单位
   4) 1) "CONFIG"                       # 执行的命令，以数组的形式排列
      2) "GET"                          # 这里完整的命令是 CONFIG GET slowlog-log-slower-than
      3) "slowlog-log-slower-than"

```
slowlog len 查看慢日志长度

```php
127.0.0.1:6379> SLOWLOG len
(integer) 9

slowlog reset #清空慢日志

127.0.0.1:6379> SLOWLOG reset
OK
127.0.0.1:6379> SLOWLOG len
(integer) 1

```
























