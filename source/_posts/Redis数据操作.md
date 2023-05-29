---
# sticky: 2
title: Redis数据操作
date: 2021-12-09 00:08:00
description: 'Redis之数据结构与数据操作'
tag: Redis
cover: https://s3.bmp.ovh/imgs/2021/12/2d56e9883b9c2bd9.jpg
---

![Redis](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fstaticdata.yuanshihui.com%2Fdata%2FM00%2F6B%2F48%2FCIECAFtXNqyAJ117AABgsxRD-vs598.png&refer=http%3A%2F%2Fstaticdata.yuanshihui.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1641607439&t=3fe57771c8cbb3351a2c376bba6f5c00)

## 数据结构

Redis 数据类型分为：**字符串类型**、**散列类型**、**列表类型**、**集合类型**、**有序集合类型**。

Redis 这么火，它运行有多块？一台普通的笔记本电脑，可以在 1 秒钟内完成十万次的读写操作。

Redis 是 `key-value` 的数据结构，每条数据都是⼀个键值对键的类型是字符串，键的类型是字符串

原子操作：最小的操作单位，不能继续拆分。即最小的执行单位，不会被其他命令插入。高并发下不存在竞态条件。

KEY 的命名：一个良好的建议是 article:1:title 来存储 ID 为 1 的文章的标题。

**注意：键不能重复**

![](http://www.freeoa.net/images/unixsupt/db/2019/redis_dt_obj.png)

### keys 基本命令

-   `Keys *`

    该命令用于查看所有键。

-   `Keys 'xx'`

    该命令用于查看名称中包含 xx 的键

-   `type key`

    该命令用于查看键对应的 value 的类型

-   `del key`
    该命令用于在 key 存在时删除 key。

-   `dump key`
    序列化给定 key ，并返回被序列化的值。

-   `exists key`
    检查给定 key 是否存在。

-   `expire key seconds`
    为给定 key 设置过期时间，以秒计。

-   `expireat key timestamp`
    expireat 的作用和 expire 类似，都用于为 key 设置过期时间。

    不同在于 expireat 命令接受的时间参数是 UNIX 时间戳(unix timestamp)

-   `pexpire key milliseconds`
    设置 key 的过期时间以毫秒计。

-   `expirtae  key milliseconds-timestamp`
    设置 key 过期时间的时间戳(unix timestamp) 以毫秒计。

-   `key pattern`
    查找所有符合给定模式( pattern)的 key 。

-   `move key db`
    将当前数据库的 key 移动到给定的数据库 db 当中。

-   `persist key`
    移除 key 的过期时间，key 将持久保持。

-   `pttl key`
    以毫秒为单位返回 key 的剩余的过期时间。

### String 类型

字符串是 Redis 中的最基础的数据结构，我们保存到 Redis 中的 key，也就是键，就是字符串结构的。除此之外，Redis 中其它数据结构也是在字符串的基础上设计的，可见字符串结构对于 Redis 是多么重要。

Redis 中的字符串结构可以保存多种数据类型。如：简单的字符串、JSON、XML、二进制等，

但有一点要特别注意：**在 Redis 中字符串类型的值最大只能保存 512 MB。**

![String图解](http://www.freeoa.net/images/unixsupt/db/2019/redis_dt_str.png)

#### 保存

语法： `set key value [EX seconds] [PX milliseconds] [NX|XX]`

set 命令有几个非必须的选项，下面我们看一下它们的具体说明：

> ​ EX seconds：为键设置秒级过期时间
> ​ PX milliseconds：为键设置毫秒级过期时间
> ​ NX：键必须不存在，才可以设置成功，用于添加
> ​ XX：键必须存在，才可以设置成功，用于更新

-   **设置键值**

    语法：`set key value`

    _示例：_

    ```python
    # 如设置键为name 值为 alex
    set name alex
    ```

-   **设置键值及过期时间，以秒为单位**

    语法：`setex key seconds value`

    _示例：_

    ```python
    # 如设置键为eat 值为 apple 过期时间为5秒的数据
    set eat 5 apple
    ```

    > 在时间存在中可持续获取，过期后值为**（nil）**

-   **设置多个键值**

    语法：`mset key1 value1 key2 value2 ...`

    _示例：_

    ```python
    # 如设置键多个键值
    mset a 10 b 20 c 30
    ```

-   **追加值**

    语法：`append key value`

    _示例：_

    ```python
    # 向键为name 追加值 'Big'
    append name Big
    ```

#### 获取

-   根据键获取值，如果不存在此键则返回 nil

    语法：`get key `

    _示例：_

    ```python
    # 获取name的值
    get name
    --alex
    ```

-   根据多个键获取多个值

    语法：`mget key1 key2... `

    _示例：_

    ```python
    # 获取a1,a2,a3的值
    get a1 a2 a3
    --10 20 30
    ```

### **哈希类型**

**Redis 中哈希类型都是键值对结构的，所以要特别注意这里的 value 并不是指 Redis 中 key 的 value，而是哈希类型中的 field 所对应的 value。**

> hash ⽤于存储对象，对象的结构为属性、值

`值的类型为string`

![](http://www.freeoa.net/images/unixsupt/db/2019/redis_dt_hash.png)

#### 增加修改

-   设置单个属性

    语法：`hset key field value `

    _示例：_

    ```python
    # 设置键 user的属性 name 为 Shrimps
    hset user name Shrimps
    --(inteqer) 1
    ```

-   设置多个属性

    语法：`hmset key field1 value1 field2 value2 ... `

    _示例：_

    ```python
    # 设置键 test 的属性 name 为 mono、 属性 age 为 6
    hmset test name mono age 6
    -OK
    ```

#### 获取

-   获取指定键所有的属性

    语法：`hkeys key `

    _示例：_

    ```python
    # 获取键 test 的所有属性
    hkeys test
    --1) 'name'
    --2) 'age'
    ```

-   获取⼀个属性的值

    语法：`hget key field `

    _示例：_

    ```python
    # ：获取键 test 属性 'name' 的值
    hget test name
    -'mono'
    ```

-   获取多个属性的值

    语法：`hmget key field1 field2 ... `

    _示例：_

    ```python
    # 获取键 test 属性 'name' 与 'age' 的值
    hmget tset name age
    --1) 'mono'
    --2) '6'
    ```

-   获取所有属性的值

    语法：`hvals key `

    _示例：_

    ```python
    # ：获取键 test 属性 'name' 的值
    hvals test
    --1) 'mono'
    --2) '6'
    ```

#### 删除

-   删除整个 hash 键及值，使⽤ del 命令

    **删除属性，属性对应的值会被⼀起删除**

    语法：`hdel key field1 field2 ... `

    _示例：_

    ```python
    # 删除键 test 的属性'age'
    hdel test age
    --(inteqer) 1

    # 再次查询键test所有属性

    hkeys test
    --1) 'name'
    ```

### list 类型

**Redis 列表类型的特点如下：**
列表中所有的元素都是有序的，所以它们是可以通过索引获取的，也就是上图中的 lindex 命令。并且在 Redis 中列表类型的索引是从 0 开始的。

> 列表中的元素是可以重复的，也就是说在 Redis 列表类型中，可以保存同名元素

`列表的元素类型为string`

按照插⼊顺序排序

**如下图所示：**
![img](http://www.freeoa.net/images/unixsupt/db/2019/redis_dt_list3.png)

**Redis 中列表类型的插入和弹出操作：**

![](http://www.freeoa.net/images/unixsupt/db/2019/redis_dt_list1.png)

**Redis 中列表类型的获取与删除操作：**

![img](http://www.freeoa.net/images/unixsupt/db/2019/redis_dt_list2.png)

#### 增加

-   在左侧插⼊数据

    语法：`lpush key value1 value2 ... `

    _示例：_

    ```python
    # 从键为 strs 的列表左侧加⼊数据 a, b, c
    lpush strs a b c
    -(integer) 3

    '''获取查看 '''
    lrange test 0 3
    --1) 'c'
    --2) 'b'
    --3) 'a'
    ```

-   在右侧插⼊数据

    语法：`rpush key value1 value2 ... `

    _示例：_

    ```python
    # ：从键为 strs 的列表右侧加⼊数据 0 1
    rpush strs 0 1
    -(integer) 5

    '''获取查看 '''
    lrange test 0 5
    --1) 'c'
    --2) 'b'
    --3) 'a'
    --4) '0'
    --5) '1'
    ```

-   在指定元素的前或后插⼊新元素

    语法：`linsert key before或after 现有元素 新元素 `

    _示例：_

    ```python
    # 在键为 test 的列表中元素 'a' 前加⼊ 'bb'
    linsert test before b aa
    --(integer) 5

    '''获取查看 '''
    lrange test 0 5
    --1) 'c'
    --2) 'b'
    --3) 'bb'
    --4) 'a'
    --5) '0'
    --6) '1'
    ```

#### 获取

-   返回列表⾥指定范围内的元素

    > start、stop 为元素的下标索引
    >
    > 索引从左侧开始，第⼀个元素为 0
    >
    > 索引可以是负数，表示从尾部开始计数，如-1 表示最后⼀个元素

    语法：`lrange key start stop `

    _示例：_

    ```python
    # 获取键 test 的列表所有元素
    lrange test 0 -1
    --1) 'name'
    --2) 'age'
    ```

-   设置指定索引位置的元素值

    > 索引从左侧开始，第⼀个元素为 0
    >
    > 索引可以是负数，表示尾部开始计数，如-1 表示最后⼀个元素

    语法：`lset key index value `

    _示例：_

    ```python
    # 修改键为 test 的列表中下标为1的元素值为'z'
    lset a 1 z
    -OK
    ```

#### 删除

-   删除指定元素

    > 将列表中前 count 次出现的值为 value 的元素移除
    >
    > **count > 0:** 从头往尾移除
    >
    > **count < 0:** 从尾往头移除
    >
    > **count = 0:** 移除所有

    语法：`lrem key count value `

    _示例：_

    ```python
    # bbc -> 'a'、'b'、'a'、'b'、'a'、'b'
    # 从 bbc 列表右侧开始删除2个'b'
    lrem bbc -2 b
    # 从 bbc 列表左侧开始删除2个'a'
    lrem bbc 2 a
    --1) 'b'
    --2) 'a'
    ```

### set 类型

Redis 中的集合类型，也就是 set。在 Redis 中 set 也是可以保存多个字符串的，经常有人会分不清 list 与 set，下面我们重点介绍一下它们之间的不同：

-   set 中的元素是不可以重复的，而 list 是可以保存重复元素的。
-   set 中的元素是无序的，而 list 中的元素是有序的。
-   set 中的元素不能通过索引下标获取元素，而 list 中的元素则可以通过索引下标获取元素。
-   除此之外 set 还支持更高级的功能，例如多个 set 取交集、并集、差集等。

`⽆序集合 元素为string类型 元素具有唯⼀性，不重复`

**说明：对于集合没有修改操作**

#### 增加

-   添加元素

    语法：`sadd key member1 member2 ... `

    _示例：_

    ```python
    # 向键 set1 的集合中添加元素 'cat','dog','bunny'
    sadd set1 cat dog bunny
    --(inteqer) 3
    ```

#### 获取

-   返回所有的元素

    语法：`smerbers key `

    _示例：_

    ```python
    # 获取键 set1 的集合中所有元素
    smerbers set1
    --1) 'cat'
    --2) 'dog'
    --3) 'bunny'
    ```

#### 删除

-   删除指定元素

    语法：`srem key `

    _示例：_

    ```python
    # ：删除键 set1 的集合中元素 'bunny'
    srem set1 bunny
    --(inteqer) 1
    ```

### zset 类型

Redis 有序集合和集合一样也是 string 类型元素的集合,且不允许重复的成员。

sorted set，有序集合 `元素为string类型` `元素具有唯⼀性，不重复`

> 不同的是每个元素都会关联一个 double 类型的分数。redis 正是通过分数来为集合中的成员进行从小到大的排序。
>
> 有序集合的成员是唯一的,但分数(score)却可以重复。
>
> 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

**每个元素都会关联⼀个 double 类型的 score，表示权重，通过权重将元素从⼩到⼤排序**

**说明：没有修改操作**

#### 增加

-   添加

    语法：`zadd key score1 member1 score2 member2 ... `

    _示例：_

    ```python
    # 向键 zset1 的集合中添加元素 'kos','mogo','tre' 权重分别为 3,4,2
    zadd zset1 2 kos 4 mogo 3 tre
    --(inteqer) 1
    ```

#### 获取

**返回指定范围内的元素**

> start、stop 为元素的下标索引
>
> 索引从左侧开始，第⼀个元素为 0
>
> 索引可以是负数，表示从尾部开始计数，如-1 表示最后⼀个元素

-   添加元素

    语法：`zrange key start stop`

    _示例：_

    ```python
     # 获取键 zset1 的集合中所有元素
    zrange zset1 0 -1
    --1) 'tre'
    --2) 'kos'
    --3) 'mogo'
    ```

-   返回 score 值在 min 和 max 之间的成员

    语法：`zrangebyscore key min max`

    _示例：_

    ```python
     # 获取键 zset1 的集合中权限值在5和6之间的成员
    zrangebyscore zset1 3 4
    --1) 'kos'
    --2) 'mogo'
    ```

-   返回成员 member 的 score 值

    语法：`zscore key member`

    _示例：_

    ```python
     # 获取键 zset1 的集合中元素 'kos' 的权重
    zscore zset1 kos
    --'3'
    ```

#### 删除

-   删除指定元素

    语法：`zrem key member1 member2 ...`

    _示例：_

    ```python
     # 删除集合 zset1 中元素 'tre'
    zrem zset1 tre
     --(inteqer) 1
    ```

-   删除权重在指定范围的元素

    语法：`zremrangebyscore key min max`

    _示例：_

    ```python
     # 删除集合 zset1 中权重为2至3之间的元素
    zremrangebyscore zset1 2 3
     --(integer) 1
    ```

参考文章：[Redis 主要的五种数据结构](http://www.freeoa.net/osuport/db/redis-data-structure_3278.html) [菜鸟 Redis 数据结构](https://www.runoob.com/w3cnote/redis-intro-data-structure.html)
