# redis

## 1. 认识redis

- 认识nosql 

  no only sql  不仅仅是sql  非关系数据库，没有表结构

- nosql和sql对比

  |          |                    sql                     |     nosql     |
  | :------: | :----------------------------------------: | :-----------: |
  | 数据结构 |                   结构化                   |   非结构化    |
  | 数据关联 |                   关联的                   |   非关联的    |
  | 查询方式 |                  sql查询                   |   非sql查询   |
  | 事务特性 |                    ACID                    | BASE 基本满足 |
  | 存储方式 |                    磁盘                    |     内存      |
  |  扩展性  |                    垂直                    |     水平      |
  | 应用场景 | 数据结构固定，对数据安全性和一致性要求较高 | 对性能要求高  |

  1. ACID 指的是：原子性、一致性、隔离性、持久性
  2. 垂直：提高机器硬件设置  水平：提供机器数量

- redis
  基于内存的键值型NoSQL数据库
  1. 键值（key-value）型，value支持多种不同数据结构，功能丰富
  2. 单线程，每个命令具备原子性
  3. 低延迟，速度快（基于内存、IO多路复用、良好的编码）。
  4. 支持数据持久化
  5. 支持主从集群、分片集群
  6. 支持多语言客户端



## 2. redis常见命令

Redis是一个key-value的数据库，key一般是String类型，不过value的类型多种多样

- 通用命令
  1. KEYS：查看符合模板的所有key，类型模糊查询数据量大时非常缓慢
     ![image-20231226174900154](assets/image-20231226174900154.png)
  2. DEL：删除一个指定的key
     ![image-20231226174935426](assets/image-20231226174935426.png)
  3. EXISTS：判断key是否存在
     ![image-20231226175343290](assets/image-20231226175343290.png)
  4. EXPIRE：给一个key设置有效期，有效期到期时该key会被自动删除  (已经存在的)
  5. TTL：查看一个KEY的剩余有效期
     ![image-20231226175901856](assets/image-20231226175901856.png)

- strings 常见命令

  reids key的格式  使用：表示层级结构 shop:user:1

  1. SET：添加或者修改已经存在的一个String类型的键值对

  2. GET：根据key获取String类型的value

     ![image-20231226181308832](assets/image-20231226181308832.png)
     

  3. MSET：批量添加多个String类型的键值对

  4. MGET：根据多个key获取多个String类型的value
     ![image-20231226181554796](assets/image-20231226181554796.png)

  5. INCR：让一个整型的key自增1

  6. INCRBY:让一个整型的key自增并指定步长，例如：incrby num 2 让num值自增2
     ![image-20231226181646149](assets/image-20231226181646149.png)

  7. INCRBYFLOAT：让一个浮点类型的数字自增并指定步长

  8. SETNX：添加一个String类型的键值对，前提是这个key不存在，否则不执行
     ![image-20231226181822961](assets/image-20231226181822961.png)

  9. SETEX：添加一个String类型的键值对，并且指定有效期
     ![image-20231226182312877](assets/image-20231226182312877.png)

​	

- Hash 类型 
  无序字典
  1. HSET key field value：添加或者修改hash类型key的field的值
     ![image-20231226183612838](assets/image-20231226183612838.png)
  2. HGET key field：获取一个hash类型key的field的值
     ![image-20231226183648164](assets/image-20231226183648164.png)
  3. HMSET：批量添加多个hash类型key的field的值
     ![image-20231226183802193](assets/image-20231226183802193.png)
  4. HMGET：批量获取多个hash类型key的field的值
     ![image-20231226183833901](assets/image-20231226183833901.png)
  5. HGETALL：获取一个hash类型的key中的所有的field和value
     ![image-20231226183857199](assets/image-20231226183857199.png)
  6. HKEYS：获取一个hash类型的key中的所有的field
  7. HVALS：获取一个hash类型的key中的所有的value
     ![image-20231226183951340](assets/image-20231226183951340.png)
  8. HINCRBY:让一个hash类型key的字段值自增并指定步长
     ![image-20231226184035971](assets/image-20231226184035971.png)
  9. HSETNX：添加一个hash类型的key的field值，前提是这个field不存在，否则不执行
     ![image-20231226184306775](assets/image-20231226184306775.png)



- List 类型
  双向链表结构
  1. LPUSH key element ... ：向列表左侧插入一个或多个元素
  2. RPUSH key element ... ：向列表右侧插入一个或多个元素
     ![image-20231226185454131](assets/image-20231226185454131.png)
     ![image-20231226185546141](assets/image-20231226185546141.png)
  3. LPOP key：移除并返回列表左侧的第一个元素，没有则返回nil
  4. RPOP key：移除并返回列表右侧的第一个元素
  5. LRANGE key star end：返回一段角标范围内的所有元素
     ![image-20231226185740409](assets/image-20231226185740409.png)
  6. BLPOP和BRPOP：与LPOP和RPOP类似，只不过在没有元素时等待指定时间，而不是直接返回nil
     ![image-20231226190110134](assets/image-20231226190110134.png)



- Set 类型

  无序，不重复

  1. SADD key member ... ：向set中添加一个或多个元素
  2. SREM key member ... : 移除set中的指定元素
     ![image-20231226190624775](assets/image-20231226190624775.png)
  3. SCARD key： 返回set中元素的个数
     ![image-20231226190651727](assets/image-20231226190651727.png)
  4. SISMEMBER key member：判断一个元素是否存在于set中
     ![image-20231226190802868](assets/image-20231226190802868.png)
  5. SMEMBERS：获取set中的所有元素
     ![image-20231226190844208](assets/image-20231226190844208.png)
  6. SINTER key1 key2 ... ：求key1与key2的交集
  7. SDIFF key1 key2 ... ：求key1与key2的差集
  8. SUNION key1 key2 ..：求key1和key2的并集

  ```text
  将下列数据用Redis的Set集合来存储：
      张三的好友有：李四、王五、赵六
      李四的好友有：王五、麻子、二狗
  利用Set的命令实现下列功能：
      计算张三的好友有几人
      计算张三和李四有哪些共同好友
      查询哪些人是张三的好友却不是李四的好友
      查询张三和李四的好友总共有哪些人
      判断李四是否是张三的好友
      判断张三是否是李四的好友
      将李四从张三的好友列表中移除
  ```

  ![image-20231226204015655](assets/image-20231226204015655.png)
  

- SortedSet 类型
  可排序的set类型

  1. ZADD key score member：添加一个或多个元素到sorted set ，如果已经存在则更新其score值
  2. ZREM key member：删除sorted set中的一个指定元素
  3. ZSCORE key member : 获取sorted set中的指定元素的score值
  4. ZRANK key member：获取sorted set 中的指定元素的排名
  5. ZCARD key：获取sorted set中的元素个数
  6. ZCOUNT key min max：统计score值在给定范围内的所有元素的个数
  7. ZINCRBY key increment member：让sorted set中的指定元素自增，步长为指定的increment值
  8. ZRANGE key min max：按照score排序后，获取指定排名范围内的元素
  9. ZRANGEBYSCORE key min max：按照score排序后，获取指定score范围内的元素
  10. ZDIFF、ZINTER、ZUNION：求差集、交集、并集

  注意：所有的排名默认都是升序，如果要降序则在命令的Z后面添加REV即可

  ```text
  将班级的下列学生得分存入Redis的SortedSet中：
  	Jack 85, Lucy 89, Rose 82, Tom 95, Jerry 78, Amy 92, Miles 76
  并实现下列功能：
      删除Tom同学
      获取Amy同学的分数
      获取Rose同学的排名
      查询80分以下有几个学生
      给Amy同学加2分
      查出成绩前3名的同学
      查出成绩80分以下的所有同学
  ```

  