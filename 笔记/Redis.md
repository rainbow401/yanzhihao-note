# Redis

## 常见命令

### String

``` java
SET key value                   						//设置key=value
GET key                         						//获得键key对应的值
GETRANGE key start end          						//得到字符串的子字符串存放在一个键
GETSET key value                						//设置键的字符串值，并返回旧值
GETBIT key offset               						//返回存储在键位值的字符串值的偏移
MGET key1 [key2..]              						//得到所有的给定键的值
SETBIT key offset value         						//设置或清除该位在存储在键的字符串值偏移
SETEX key seconds value         						//键到期时设置值
SETNX key value                 						//设置键的值，只有当该键不存在
SETRANGE key offset value       						//覆盖字符串的一部分从指定键的偏移
STRLEN key                      						//得到存储在键的值的长度
MSET key value [key value...]   						//设置多个键和多个值
MSETNX key value [key value...] 						//设置多个键多个值，只有在当没有按键的存在时
PSETEX key milliseconds value   						//设置键的毫秒值和到期时间
INCR key                        						//增加键的整数值一次
INCRBY key increment            						//由给定的数量递增键的整数值
INCRBYFLOAT key increment       						//由给定的数量递增键的浮点值
DECR key                        						//递减键一次的整数值
DECRBY key decrement            						//由给定数目递减键的整数值
APPEND key value                						//追加值到一个键
DEL key                         						//如果存在删除键
DUMP key                        						//返回存储在指定键的值的序列化版本
EXISTS key                      						//此命令检查该键是否存在
EXPIRE key seconds              						//指定键的过期时间
EXPIREAT key timestamp          						//指定的键过期时间。在这里，时间是在Unix时间戳格式
PEXPIRE key milliseconds        						//设置键以毫秒为单位到期
PEXPIREAT key milliseconds-timestamp        //设置键在Unix时间戳指定为毫秒到期
KEYS pattern                    						//查找与指定模式匹配的所有键
MOVE key db                     						//移动键到另一个数据库
PERSIST key                     						//移除过期的键
PTTL key                        						//以毫秒为单位获取剩余时间的到期键。
TTL key                         						//获取键到期的剩余时间。
RANDOMKEY                       						//从Redis返回随机键
RENAME key newkey               						//更改键的名称
RENAMENX key newkey             						//重命名键，如果新的键不存在
TYPE key                        						//返回存储在键的数据类型的值。
```

### List

``` java
BLPOP key1 [key2 ] timeout 						//取出并获取列表中的第一个元素，或阻塞，直到有可用
BRPOP key1 [key2 ] timeout 						//取出并获取列表中的最后一个元素，或阻塞，直到有可用
BRPOPLPUSH source destination timeout //从列表中弹出一个值，它推到另一个列表并返回它;或阻塞，直到有可用
LINDEX key index 											//从一个列表其索引获取对应的元素
LINSERT key BEFORE|AFTER pivot value 	//在列表中的其他元素之后或之前插入一个元素
LLEN key 															//获取列表的长度
LPOP key 															//获取并取出列表中的第一个元素
LPUSH key value1 [value2] 						//在前面加上一个或多个值的列表
LPUSHX key value 											//在前面加上一个值列表，仅当列表中存在
LRANGE key start stop 								//从一个列表获取各种元素
LREM key count value 									//从列表中删除元素
LSET key index value 									//在列表中的索引设置一个元素的值
LTRIM key start stop 									//修剪列表到指定的范围内
RPOP key 															//取出并获取列表中的最后一个元素
RPOPLPUSH source destination 					//删除最后一个元素的列表，将其附加到另一个列表并返回它
RPUSH key value1 [value2] 						//添加一个或多个值到列表
RPUSHX key value 											//添加一个值列表，仅当列表中存在
```

### Hash

``` java
HDEL key field[field...] 												//删除对象的一个或几个属性域，不存在的属性将被忽略
HEXISTS key field 															//查看对象是否存在该属性域
HGET key field 																	//获取对象中该field属性域的值
HGETALL key 																		//获取对象的所有属性域和值
HINCRBY key field value 												//将该对象中指定域的值增加给定的value，原子自增操作，只																							能是integer的属性值可以使用
HINCRBYFLOAT key field increment 								//将该对象中指定域的值增加给定的浮点数
HKEYS key 																			//获取对象的所有属性字段
HVALS key 																			//获取对象的所有属性值
HLEN key 																				//获取对象的所有属性字段的总数
HMGET key field[field...] 											//获取对象的一个或多个指定字段的值
HSET key field value 														//设置对象指定字段的值
HMSET key field value [field value ...] 				//同时设置对象中一个或多个字段的值
HSETNX key field value 													//只在对象不存在指定的字段时才设置字段的值
HSTRLEN key field 															//返回对象指定field的value的字符串长度，如果该对象																										或者field不存在，返回0
HSCAN key cursor [MATCH pattern] [COUNT count]	//类似SCAN命令
```

### Set

```java
SADD key member [member ...] 												//添加一个或者多个元素到集合 (set)里
SCARD key 																					//获取集合里面的元素数量
SDIFF key [key ...] 																//获得队列不存在的元素
SDIFFSTORE destination key [key ...] 								//获得队列不存在的元素，并存储在一个关键的结果集
SINTER key [key ...] 																//获得两个集合的交集
SINTERSTORE destination key [key ...] 							//获得两个集合的交集，并存储在一个集合中
SISMEMBER key member 																//确定一个给定的值是一个集合的成员
SMEMBERS key 																				//获取集合里面的所有key
SMOVE source destination member 										//移动集合里面的一个key到另一个集合
SPOP key [count] 																		//获取并删除一个集合里面的元素
SRANDMEMBER key [count] 														//从集合里面随机获取一个元素
SREM key member [member ...] 												//从集合里删除一个或多个元素，不存在的元素会被忽																											 略
SUNION key [key ...] 																//添加多个set元素
SUNIONSTORE destination key [key ...] 							//合并set元素，并将结果存入新的set里面
SSCAN key cursor [MATCH pattern] [COUNT count] 			//迭代set里面的元素
```

### ZSet

```java
ZADD key score1 member1 [score2 member2] 								//添加一个或多个成员到有序集合，或者如果它已																													 经存在更新其分数
ZCARD key 																							//得到的有序集合成员的数量
ZCOUNT key min max 																			//计算一个有序集合成员与给定值范围内的分数
ZINCRBY key increment member 														//在有序集合增加成员的分数
ZINTERSTORE destination numkeys key [key ...] 					//多重交叉排序集合，并存储生成一个新的键有序																													 集合。
ZLEXCOUNT key min max 																	//计算一个给定的字典范围之间的有序集合成员的																													 数量
ZRANGE key start stop [WITHSCORES] 											//由索引返回一个成员范围的有序集合（从低到																													高）
ZRANGEBYLEX key min max [LIMIT offset count] 						//返回一个成员范围的有序集合（由字典范围）
ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT] 					//返回有序集key中，所有 score 值介于 min 																													 和 max 之间(包括等于 min 或 max )的成																												  员，有序集成员按 score 值递增(从小到大)																													次序排列
ZRANK key member 																				//确定成员的索引中有序集合
ZREM key member [member ...] 														//从有序集合中删除一个或多个成员，不存在的成																													 员将被忽略
ZREMRANGEBYLEX key min max 															//删除所有成员在给定的字典范围之间的有序集合
ZREMRANGEBYRANK key start stop 													//在给定的索引之内删除所有成员的有序集合
ZREMRANGEBYSCORE key min max 														//在给定的分数之内删除所有成员的有序集合
ZREVRANGE key start stop [WITHSCORES] 									//返回一个成员范围的有序集合，通过索引，以分																													 数排序，从高分到低分
ZREVRANGEBYSCORE key max min [WITHSCORES] 							//返回一个成员范围的有序集合，以socre排序从																													 高到低
ZREVRANK key member 																		//确定一个有序集合成员的索引，以分数排序，从																													 高分到低分
ZSCORE key member 																			//获取给定成员相关联的分数在一个有序集合
ZUNIONSTORE destination numkeys key [key ...] 					//添加多个集排序，所得排序集合存储在一个新的																												 	 键
ZSCAN key cursor [MATCH pattern] [COUNT count] 					//增量迭代排序元素集和相关的分数
```

