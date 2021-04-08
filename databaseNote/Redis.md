# Redis

> 全名: REmote DIctionary Server (远程字典服务器)

> c语言编写的 高性能(key/value) 分布式内存数据库

----

## 常用命令
### db或通用命令

```
select 15   //选择第16个库,默认就16个

type key //返回类型

move key db //移动该键值对到其他库

EXPIRE key timeout //给存在的key设置 时间

persist key //移除key的时间,使其永久

ttl key //查看状态,-1 :永久,-2 不存在 
pttl key //毫秒级显示

exists key //1存在 ;0不存在

rename key newKeyName //重命名

del key //删除

FLUSHDB——清除当前库
FLUSHALL——清除所有库

info replication //显示主从信息
```

### string
```
set key value [ex secends//生存多少秒]

setex key timeout value

setnx key value // set if not exist

mset k1 v1 k2 v2 k3 v3 //批量

msetnx k3 v3 k4 v4 // 存在任意一个 ,则全部失败
```

```
get key
mget k1 k2 k3
```

```
incr key //自增
decr key //自减
incrby key number //加 number
decrby key number //减 number
```

### list

> **r** 开头的**list**操作,只有push,pop类型
>
> 双向链表 数据结构

```js
lpush k1 v1 v2 v3 v4 
rpush k1 v5 v6 v7 v8

//执行完可以参考如下
// v4 v3 v2 v1 v5 v6 v7 v8
// lpush : 先插v1 ,插完再在左边插入v2....
// rpush : 一直在右边依次插入

```

```js
lrange key startIndex endIndex  // 从左到右,读取范围
// 如执行完上边两条 push命令 之后
// 执行 lrange k1 0 -1 
// 结果 >> : v4 v3 v2 v1 v5 v6 v7 v8

lpop key //在最左边出栈.相当于查询最左边的值,且将其在数组中删除
rpop key //方向相反

lindex key index  // 如 list 的 get(index)

llen key //查询数组的长度

lrem key num value //从数组中移除 num 个 value

ltrim key startIndex endIndex //左->右截取 start-end的长度 其他舍弃

rpoplpush 源列表 目的列表  //将源列表的最右边值出栈,放入目的列表的最左边

lset key index value //相当于list的 set(index,value)

linsert key before/after v1 v2 // 在值v1的前边/后边插入v2
//若 存在多个v1, 则以 左>右的 第一个执行

```



### set

```js
sadd key v1 [ v2 v3] //如插入sadd k1 v1 v1 v2 ; 则仅有v1 v2
```

```js
smembers key //显示所有set中的数据

srandmember key number //随机获取number个元素,且不重复

spop key //随机出栈一个元素

smove key1 key2 key1的值  //作用是将key1中的某个值移动到key2中(剪切)

sismembers key value //set中是否存在value,存在1,不存在0

scard key //获取set中的元素个数
```

```js
srem key value //移除set中的value元素
```

#### 差集

```js
sdiff key1 key2 // key1中存在的,且过滤掉key2中也存在的
//有先后顺序 --- 是 k1 减 k2 的关系,不显示负
```

#### 交集

```js
sinter k1 k2 // 两个集合同时存在的
```

#### 并集

```js
sunion k1 k2 //合并显示,相同的显示一次
```

---

### hash

| 变量名  | 描述       |
| ------- | ---------- |
| mapName | map的key   |
| field   | value的key |
| value   | 值         |

```js
hset mapName field value [field value] 

hmset mapName field value [field value] //和上边没什么不同

hsetnx mapName field value //不存在则插

hincrby/hincrbyfloat mapName field num
```

```js
hget mapName field

hgetall mapName
//结果如下,以 键 值 的形式
1) "name"
2) "sunyuhong"
3) "age"
4) "26"
5) "sex"
6) "man"

//************
hkeys mapName //类似 java map.keySet();

hvalues mapName //map.values();
//************

hlen mapName //查询map中的 键值对个数

hexists mapName field //查询map中的field是否存在,1存在,0不存在
```

```js
hdel mapName field [field] //移除map中的某些元素
```

---

### zset

> 有序无重复集合

```js
zadd zsetName score value [score value ]
//score -- 排序的数
```

```js
zrange zsetName startIndex endIndex [withscores] //根据下标查询
//zrange zset01 0 -1

zrangebyscore zsetname [(]score1 [(]socre2 [limit index num]
//查询 socre1 <= score <= score2 范围的数据
//有 ( 表示:不等于,仅有大于小于
//[limit index num] : 从下标为index的地方开始,查询num条数据

zcard zsetName //统计value的个数,不含score

zcount zsetName score1 socre2 //统计1-2之间的value个数

zrank zsetName value //获取value下标

zscore zstname value //获取该值的 score

zrevrank zsetname value //逆序获取下标值

zrevrange zsetname 0 -1 //逆序根据下表查询

zrevrangebyscore zsetname max min //逆序根据score范围查询
```

```
zrem zsetname value
```



---

## redis配置文件

| 配置项           | 默认                | 生产常用                                      | 说明                                                         |
| ---------------- | ------------------- | --------------------------------------------- | ------------------------------------------------------------ |
| `daemonize`      | no                  | yes                                           | 是否以守护进程的方式运行                                     |
| pidfile          | /var/run/redis.pid  |                                               | 当 Redis 以守护进程方式运行时，Redis 默认会把 pid 写入 /var/run/redis.pid 文件，可以通过 pidfile 指定 |
| port             | 6379                |                                               | 监听端口                                                     |
| bind             | 127.0.0.1 -::1      |                                               | 绑定主机服务器                                               |
| `timeout`        | 0                   | 300                                           | 当客户端闲置多少**`秒`**后关闭连接，如果指定为 0 ，表示关闭该功能 |
| loglevel         | notice              | notice                                        | 默认是生产级别                                               |
| logfile          | ""                  | stdout                                        | 日志记录方式，默认为标准输出，如果配置 Redis 为守护进程方式运行，而这里又配置为""，则日志将会发送给 /dev/null |
| databases        | 16                  |                                               | 设置数据库的数量，登录后默认选择0号数据库，可以使用SELECT 命令在连接上指定数据库id |
| `save `          | ""                  | save 900 1  save 300 10         save 60 10000 | 默认不自动备份.需要开启下别的三个级别....分别表示 900 秒（15 分钟）内有 1 个更改，300 秒（5 分钟）内有 10 个更改以及 60 秒内有 10000 个更改。 |
| rdbcompression   | yes                 |                                               | 指定存储至本地数据库时是否压缩数据,Redis 采用 LZF 压缩，如果为了节省 CPU 时间，可以关闭该选项，但会导致数据库文件变的巨大 |
| dbfilename       | dump.rdb            |                                               | 指定本地数据库文件名                                         |
| dir              | ./                  |                                               | 指定本地数据库存放目录                                       |
| `maxclients`     | 10000               | 128                                           | 设置同一时间最大客户端连接数                                 |
| maxmemory        | 注释                |                                               | redis的最大内存                                              |
| maxmemory-policy | # noeviction        |                                               | # volatile-lru -> Evict using approximated LRU, only keys with an expire set.<br/># allkeys-lru -> Evict any key using approximated LRU.<br/># volatile-lfu -> Evict using approximated LFU, only keys with an expire set.<br/># allkeys-lfu -> Evict any key using approximated LFU.<br/># volatile-random -> Remove a random key having an expire set.<br/># allkeys-random -> Remove a random key, any key.<br/># volatile-ttl -> Remove the key with the nearest expire time (minor TTL)<br/># noeviction -> Don't evict anything, just return an error on write operations. |
| appendonly       | no                  | no                                            | 指定是否在每次更新操作后进行日志记录，Redis 在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis 本身同步数据文件是按上面 save 条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为 no |
| appendfsync      | everysec            | everysec                                      | 指定更新日志条件，共有 3 个可选值：no：表示等操作系统进行数据缓存同步到磁盘（快）always：表示每次更新操作后手动调用 fsync() 将数据写到磁盘（慢，安全）everysec：表示每秒同步一次（折中，默认值） |
| include          | /path/to/local.conf |                                               | 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件 |
|                  |                     |                                               |                                                              |

---

## 事务

> 一次执行多个命令,本质是一组命令的集合,一个事务中的所有命令都会序列化,按顺序串行化执行,而不会被其他命令插入

> 一个队列中,一次性,顺序性,排他性的执行一些列命令

> 不保证原子性,redis同一个事务一条命令失败,其他也能成功(添加watch解决),没有回滚

### 相关命令

```
multi //标记一个事务块的开始,相当于开启队列,之后得命令都会返回queued/error,哪怕是get

exec //执行所有事务块内的命令,

discard //取消事务

watch key [key...] //监视一个或多个key ,如果在事务执行之前, 这个或这些key 被其他命令所改动,那么事务将被打断

unwatch //取消 watch 命令对所有key 的监视
```

#### 正常执行

```
--->multi
ok
--->set k1 v1
queued
--->set k2 v2
queued
--->get k1
queued
--->exec
ok
ok
v1
```

#### 放弃事务

```
---> multi
ok
---> set k1 11
queued
---> set k2 22
queued
---> discard
ok
---> get k1
v1
```

**--数据无改动,k1仍为v1--**

#### 执行前报错--全部不执行

```
---> multi
ok
---> set kd1 v11
queued
---> set kd2 22
queued
---> setget fdsa //故意写错一条
error
---> get kd1
queued
---> exec
error
---> get kd1
nil //一条失败,全部失败
```

#### 执行中报错--仅报错的不执行

```
---> multi
ok
---> incr kd1
queued   //值为 v11 自增1在添加到事务队列中时不报错
---> set kd2 890
queued
---> exec
error
ok
---> get kd2
890 
//仅在执行自增时失败,其他正常执行
```

#### watch监控

> 不管成功还是失败,在执行 exec命令后,对key得监控都将被取消
>
> 监控多个,则比较多个,条件为    &&
>
> 基于乐观锁

##### 悲观锁

总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁（共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程）。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。Java中synchronized和ReentrantLock等独占锁就是悲观锁思想的实现

##### 乐观锁 

总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和CAS算法实现。乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库提供的类似于write_condition机制，其实都是提供的乐观锁。在Java中java.util.concurrent.atomic包下面的原子变量类就是使用了乐观锁的一种实现方式CAS实现的

> 策略:提交版本必须大于当前记录的版本才执行更新

##### CAS(Compare And Swap)

CAS 有 3 个操作数，内存值 V，旧的预期值 A，要修改的新值 B。
当且仅当预期值 A 和内存值 V相同时，将内存值 V 修改为 B，否则什么都不做

> 实现方式,原子类中的自旋锁

> 缺点:cpu开销很大,尤其多个线程同时改动一个值

> ABA问题: 线程1准备用cas将变量A替换为B,但读取旧的预期值后,且未执行交换前,
>
> 线程2将A替换为了C,又将C替换为了A.
>
> 这时线程1执行交换命令时,发现预期值与内存之相同.交换成了B
>
> ----可能存在潜藏的问题,,,将上例比方成 栈,
>
> --线程1 :A->B 要将A出栈,栈顶指向B
>
> --线程2 :A->B 要将A,B 都出栈,进栈C B A,此时:A->B->C
>
> ABA问题发生后: 栈顶--->B    B --->C   中间断了,线程2 的 B-->C 成了垃圾
>
> 解决:加上版本号
>
> 

----

##### 示例  

> balance=100 和 debt=0

````
---> watch balance
ok
-----------------------------另一个线程------------> set balance 800
-----------------------------另一个线程------------> ok
---> multi
ok
---> decrby balance 20
queued
---> incrby debt 20
queued
---> exec
nil //执行时比较且仅比较 balance ,观察的值与内存中的值不匹配,执行失败,
````

**一条失败全部失败,且在执行exec命令后,对字段得监控自动取消**

#### unwatch

> 取消对值得监控   

```
....之前一堆得操作
---> unwatch
ok
//没有key
```



## 主从复制

- 配从库不配主库
- 从库配置:slaveof 主库id 主库端口
  - 如    `slaveof 127.0.0.1 6379`
- 修改配置文件
  - 拷贝配置文件
  - daemonize  yes
  - Pid文件名称
  - 指定端口
  - log文件名称
  - Dump.rdb名

### 数据一致

> 从执行slave...命令后,从机数据和主机保持一致

- 主机原来的数据 也会存在从机上.

- 从机原有数据全部清掉.

- 从机仅有读权限,写报错

### 默认情况

#### 主机宕机

- 主机down掉,从机待命
- 主机复活,从机自动连接
- 

#### 从机宕机

- 从机宕机,主机无变化
- 从机复活.从机变为master主机.

### 常用配置

> 主机<----从机<----从机.........
>
> 主机当掉,最前边的`从机`自动升为`主机`

#### 一主二仆

#### 薪火相传

> 从机可以直接slaveof从机

#### 反客为主

- (手动) 从机执行  `slaveof no one`  ,反客为主
- (自动) 配置哨兵模式

- 一个从机升为master,其他从机跟随该新上任的master
- 主机复活,仍然是master的身份,但没有从机,从机仍然跟随新上任的新老大

### 哨兵模式

> 反客为主的自动版

- 新建配置文件`sentinel.conf`
  - sentinel monitor \<master-name> \<ip> \<redis-port> \<quorum>
    - 告诉哨兵监控 \<ip> \<redis-port>的master主机
    - 最后一个数字 quorum，指明当有多少个sentinel认为一个master失效时，master才算真正失效 
  - 一组sentinel 可以同时监控多个Master
-  哨兵会自己在从机中选择一个当主机
- 主机复活,哨兵会将起变为从机,跟随新上任的主机

---



## java调用

