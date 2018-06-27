
## redis 各个数据类型常用操作
 
===

##### 1、KEY 相关的常用操作

 * del 删除key
 * exists  检查key是否存在，存在返回1，不存在返回0
 * expire  给key设定生存时间，当key过期时会被自动删除
 * expireat 设定生存时间，设定的值为时间戳，以秒为单位
 * pexpireat  以毫秒为单位设置key的生存时间
 * keys   匹配符合条件的key
 * persist  移除key的生存时间
 * sort   返回给的的列表，集合，有序集合 key 中经过排序的元素，排序默认以数字作为对象，值被解释为双精度浮点数，然后进行比较。
		  sort key  从小到大排序
		  sort key desc 从大到小排序
		  limt 0 5 支持分页
		  by  根据字段来排序
* type  返回key的类型，string、list、set、zset、hash

##### 2、String 类型 相关的常用操作

_String 数据结构是简单的 key-value 类型，value 不仅可以是 String，也可以是数字（当数字类型用 Long 可以表示的时候            encoding 就是整型，其他都存储在 sdshdr 当做字符串）。使用 Strings 类型，可以完全实现目前 Memcached 的功能，并且效率更高。还可以享受 Redis 的定时持久化（可以选择 RDB 模式或者 AOF 模式），操作日志及 Replication 等功能。除了提供与 Memcached 一样的 get、set、incr、decr 等操作_

 * set key value [EX seconds]  可以时间过期时间单位秒  
 * get key
 * append key value  追加到已存在的值的末尾
 * decr   key   将 key 中存储的数值减一，如果key不存在则先把key初始化为0，在执行decr操作，返回自减后的数值
 * decrby key value  将key的值减去value，如果key不存在先把key初始化为0在执行减value的操作，返回自减后的数值
 * incr key  自增
 * incrby key  value 自增value值
 * mget 获取多个key
 * mset 设置多个key 
 * strlen key  返回值 value 的长度

##### 3、Hash 哈希类型 相关操作
  Redis hash 是一个string类型的field和value的映射表，hash特别适合用于存储对象
  例如：key 为 user 的 hash 对象值是一个｛'name' => 'xxxx', 'age' => 'xx', 'address' => 'xxxxx'｝这样的键值对集合

在 Memcached 中，我们经常将一些结构化的信息打包成 hashmap，在客户端序列化后存储为一个字符串的值（一般是 JSON 格式），比如用户的昵称、年龄、性别、积分等。这时候在需要修改其中某一项时，通常需要将字符串（JSON）取出来，然后进行反序列化，修改某一项的值，再序列化成字符串（JSON）存储回去。简单修改一个属性就干这么多事情，消耗必定是很大的，也不适用于一些可能并发操作的场合（比如两个并发的操作都需要修改积分）。而 Redis 的 Hash 结构可以使你像在数据库中 Update 一个属性一样只修改某一项属性值

  * hset key field value  设置hash中的一个值，一般会通过循环操作给一个hash对象赋值
  * hgetall key 获取一个hash 对象中的所有域的值，返回一个数组
  * hget key field  返回hash表 key  中指定的 域值
  * hexists  key field 检查hash表 key中的field字段是否存在
  * hdel key field  删掉hash表中key的指定field
  * hincrby  key field value  为hash表中key的指定field的值增加value
  * hkeys  key  返回hash表key的所有域名（即 key）
  * hvals key 返回hash表中key的所有field对应的值
  * hlen  key 返回hash表中key的域的数量
  * hmset  key  field value , field value ..... 同时设置多个域
  * hmget  key   field,field...... 同时获取多个域

##### 4、LIST 类型相关操作

  Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）
      一个列表最多可以包含 2的32次方 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

  使用 List 结构，我们可以轻松地实现最新消息排行等功能（比如新浪微博的 TimeLine ）。List 的另一个应用就是消息队列，可以利用 List 的 LPUSH 操作，将任务存在 List 中，然后工作线程再用 RPOP 操作将任务取出进行执行。Redis 还提供了操作 List 中某一段元素的 API，你可以直接查询，删除 List 中某一段的元素。

  * lpush key val .... 将一个值或多个值插入list的表头，对空列表 mylist 执行命令 LPUSH mylist a b c ，列表的值将是 c b a
  * lset  key   index  value  将列表 key 下标为 index 的元素的值设置为 value，index 为 索引数字0，1，2。。。。
  * lpop key 移出list 表头第一个元素
  * rpop key 移出list 表尾第一个元素
  * rpush key value 将一个或多个值 value 插入到列表 key 的表尾(最右边)
  * lrange key start top  返回列表 key 中指定区间内的元素，区间以偏移量 start 和 stop 指定。下标(index)参数 start 和 stop 都                                            以 0 为底，也就是说，以 0 表示列表的第一个元素，以 1 表示列表的第二个元素，以此类推。

##### 5、set 集合类型相关操作
Set 就是一个集合，集合的概念就是一堆不重复值的组合。利用 Redis 提供的 Set 数据结构，可以存储一些集合性的数据。比如在微博应用中，可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。因为 Redis 非常人性化的为集合提供了求交集、并集、差集等操作，那么就可以非常方便的实现如共同关注、共同喜好、二度好友等功能，对上面的所有集合操作，你还可以使用不同的命令选择将结果返回给客户端还是存集到一个新的集合中。
1.共同好友、二度好友
2.利用唯一性，可以统计访问网站的所有独立 IP
3.好友推荐的时候，根据 tag 求交集，大于某个 threshold 就可以推荐
 * sadd  key value  把元素添加到集合中
 * smembers key   获取集合中的所有元素
 * scard key  返回集合中元素的个数
 * sdiff  key1  key2  返回只在key1集合中，不在key2集合中的元素
 * sdiffstore  destination  key  key  这个命令的作用和 SDIFF 类似，但它将结果保存到 destination 集合，而不是简单地返回结果集。如果 destination 集合已经存在，则将其覆盖。destination 可以是 key 本身。
 * sinter  key1 key2  返回两个集合的交集，即两个集合中都存在的元素
 * sinterstore destination  key  key  这个命令类似于 SINTER 命令，但它将结果保存到 destination 集合，而不是简单地返回结果集。如果 destination 集合已经存在，则将其覆盖
 * sismember key member  判断 member 元素是否集合 key 的成员。
 * smove source destination member  将member元素由source移动到destination集合中，该操作为原子操作
 * srem key member  移除key中的某个元素
 * sunion key1  key2  返回所有给定集合的并集
 * sunionstore destination key1 key2 返回集合的并集，存入destination集合中



##### 6、Sorted Set——有序集合  相关操作

和Sets相比，Sorted Sets是将 Set 中的元素增加了一个权重参数 score，使得集合中的元素能够按 score 进行有序排列，比如一个存储全班同学成绩的 Sorted Sets，其集合 value 可以是同学的学号，而 score 就可以是其考试得分，这样在数据插入集合的时候，就已经进行了天然的排序。另外还可以用 Sorted Sets 来做带权重的队列，比如普通消息的 score 为1，重要消息的 score 为2，然后工作线程可以选择按 score 的倒序来获取工作任务。让重要的任务优先执行。
1.带有权重的元素，比如一个游戏的用户得分排行榜
2.比较复杂的数据结构，一般用到的场景不算太多

Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。有序集合的成员是唯一的,但分数(score)却可以重复

* zadd key score member  将一个或多个 member 元素及其 score 值加入到有序集 key 当中。如果某个 member 已经是有序集的成员，那么更新这个 member 的 score 值，并通过重新插入这个 member 元素，来保证该 member 在正确的位置上。
* zcard key  返回有序集 key 的基数
* zcount  key  min   max  返回有序集合key中score值在min和max（默认包括 score 值等于 min 或 max）之间的元素数量
* zincrby key  increment member  为集合key 的成员 member的score增加增量increment
* zrange key  start  stop [withscores]  返回有序集合key中 下标为于 start和stop之间的 成员，默认按照score值递增
* zrangebyscore key min max [withscores] [limit offset count]  返回有序集 key 中，所有 score 值介于 min 和 max 之间(包括等于 min 或 max )的成员。有序集成员按 score 值递增(从小到大)次序排列。
* zrevrangebyscore key  min max [withscores] [limit offset count]  用法基本同上，只是排序是按照score值递减来排序
* zrank key member  返回有序集合key中member成员的排名，按照score由小到大排序，排名从0开始
* zrevrank key member  返回有序集合key中member成员的排名，按照score由大到小排序
* zrem key member  移除有序集合key中的一个或多个成员
* zremrangebyscore  key  min  max  移除有序集合key中，score值介于min和 max之间的成员
* zscore key  member 返回有序集合key中元素member的score值
* zunionstore destination numkeys  key key ....  计算给定的多个有序集合的并集，numkeys 为有序集合的数量，destination为合并后的结果集合，如果各个集合中有相同的元素，那么结果集中该元素的score为各个集合中的和
* zinterstore destination numkeys key key....  计算多个集合的交集，其他适用方式同上
     