# 概览

> key:value型的数据库
>
> (Remote Dictionary Server)

## 优点

1）速度快 不需要等待磁盘的IO，在内存之间进行的数据存储和查询，速度非常快。当然，缓存的数据总量不能太大，因为受到物理内存空间大小的限制。

（2）丰富的数据结构 除了string之外，还有list、hash、set、sortedset，一共五种类型。

（3）单线程，避免了线程切换和锁机制的性能消耗。

（4）可持久化 支持RDB与AOF两种方式，将内存中的数据写入外部的物理存储设备。

（5）支持发布/订阅。

（6）支持Lua脚本。

（7）支持分布式锁 在分布式系统中，如果不同的节点需要访同到一个资源，往往需要通过互斥机制来防止彼此干扰，并且保证数据的一致性。在这种情况下，需要使用到分布式锁。分布式锁和Java的锁用于实现不同线程之间的同步访问，原理上是类似的。

（8）支持原子操作和事务Redis事务是一组命令的集合。一个事务中的命令要么都执行，要么都不执行。如果命令在运行期间出现错误，不会自动回滚。

（9）支持主-从（Master-Slave）复制与高可用（Redis Sentinel）集群（3.0版本以上）

（10）支持管道Redis管道是指客户端可以将多个命令一次性发送到服务器，然后由服务器一次性返回所有结果。管道技术的优点是：在批量执行命令的应用场景中，可以大大减少网络传输的开销，提高性能。

# 命令配置

1. 获取配置信息

   `CONFIG GET CONFIG_SETTING_NAME`

   **例子**

   ```bash
   CONFIG get loglevel #获取日志等级
   config get *        #获取全部日志信息
   ```
2. 更改配置信息
	`CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE`
	**例子**
	```bash
	CONFIG SET loglevel "notice"
	```
	

# 使用

## 启动

` redis-cli -h host -p port -a password`

```bash
redis-cli -h 127.0.0.1 -p 6379 -a "mypass"
```

## 连接

### 实例

以下实例演示了客户端如何通过密码验证连接到 redis 服务，并检测服务是否在运行：

```bash
redis 127.0.0.1:6379> AUTH "password"# 验证密码是否正确,而不是为了登录
OK
redis 127.0.0.1:6379> PING#测试是否连通
PONG
```

## 键

Redis 键命令的基本语法如下：

```bash
 COMMAND KEY_NAME
```

**实例**

```bash
SCAN cursor [MATCH pattern\] [COUNT count] #迭代数据库中的数据库键。
SCAN命令的返回值 是一个包含两个元素的数组
	第一个数组元素是用于进行下一次迭代的新游标
	第二个数组元素则又是一个数组， 这个数组中包含了所有被迭代的元素。
	返回0才证明迭代结束
keys * #查询全部key,但有性能隐患
```

## 数据的存储与使用

### 字符串

#### 语法

```
 COMMAND KEY_NAME
```

#### 实例

```bash
SET hello redis #设置键hello,值为Redis
OK
GET hello		#得到hello这个键所对应的值	
"redis"

```

### Hash

> Redis hash 是一个 string 类型的 field（字段） 和 value（值） 的映射表，hash 特别适合用于存储对象。
>
> Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）

command key_name

**实例**

```bash
hmset student id 1234 name zqq age 18
```

### 列表(List)

> Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）
>
> 一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)

### 集合(Set)

> Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。
>
> Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。
>
> 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

### 有序集合(sorted set)

> Redis 有序集合和集合一样也是 string 类型元素的集合,且不允许重复的成员。
>
> 不同的是每个元素都会关联一个 double 类型的分数。redis 正是通过分数来为集合中的成员进行从小到大的排序。
>
> 有序集合的成员是唯一的,但分数(score)却可以重复。
>
> 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

### HyperLogLog

> Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。
>
> 在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。
>
> 但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素

#### 什么是基数?

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。

## 发布订阅

> Redis 发布订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。
>
> Redis 客户端可以订阅任意数量的频道。
>
> ![image-20201113235236195](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201113235236195.png)
>
> 当有新消息通过 PUBLISH 命令发送给频道 channel1 时， 这个消息就会被发送给订阅它的三个客户端：
>
> ![image-20201113235329372](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201113235329372.png)



## 事务

> Redis 事务可以一次执行多个命令， 并且带有以下三个重要的保证：
>
> - 批量操作在发送 EXEC 命令前被放入队列缓存。
> - 收到 EXEC 命令后进入事务执行，==事务中任意命令执行失败，其余的命令依然被执行==。
> - 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。
>
> 一个事务从开始到执行会经历以下三个阶段：
>
> - 开始事务。
> - 命令入队。
> - 执行事务。
>
> **单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。**
>
> **事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。**

疑惑

Redis事务到底具不具备原子性

## 脚本

> 使用 Lua 解释器来执行脚本

### 语法

Eval 命令的基本语法如下：

```
redis 127.0.0.1:6379> EVAL script numkeys key [key ...] arg [arg ...]
```

### 实例

以下实例演示了 redis 脚本工作过程：

```
redis 127.0.0.1:6379> EVAL "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}" 2 key1 key2 first second

1) "key1"
2) "key2"
3) "first"
4) "second"
```

## 数据备份与恢复

### 语法

redis Save 命令基本语法如下：

```
redis 127.0.0.1:6379> SAVE 
```

### 实例

```
redis 127.0.0.1:6379> SAVE 
OK
```

该命令将在 redis 安装目录中创建dump.rdb文件。

------

### 恢复数据

如果需要恢复数据，只需将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可。获取 redis 目录可以使用 **CONFIG** 命令，如下所示：

```
redis 127.0.0.1:6379> CONFIG GET dir
1) "dir"
2) "/usr/local/redis/bin"
```

也可以使用bgsave在后台备份数据

## 安全

设置密码

```bash
 CONFIG set requirepass
```

获取密码

```bash
 CONFIG get requirepass
```

验证密码

```
auth password
```

## 管道技术

Redis是一种基于客户端-服务端模型以及请求/响应协议的TCP服务。这意味着通常情况下一个请求会遵循以下步骤：

- 客户端向服务端发送一个查询请求，并监听Socket返回，通常是以阻塞模式，等待服务端响应。
- 服务端处理命令，并将结果返回给客户端。

Redis 管道技术可以在服务端未响应时，客户端可以继续向服务端发送请求，并最终一次性读取所有服务端的响应。

### 实例

查看 redis 管道，只需要启动 redis 实例并输入以下命令：

```bash
$(echo -en "PING\r\n SET runoobkey redis\r\nGET runoobkey\r\nINCR visitor\r\nINCR visitor\r\nINCR visitor\r\n"; sleep 10) | nc localhost 6379

+PONG
+OK
redis
:1
:2
:3
```

# Java使用Redis

## 连接到 redis 服务

### 实例

```java
import redis.clients.jedis.Jedis;  
public class RedisJava {    
    public static void main(String[] args) {        
        //连接本地的 Redis 服务        
        Jedis jedis = new Jedis("localhost");        
        // 如果 Redis 服务设置来密码，需要下面这行，没有就不需要        
        // jedis.auth("123456");         
        System.out.println("连接成功");        
        //查看服务是否运行        
        System.out.println("服务正在运行: "+jedis.ping());    } }
```





编译以上 Java 程序，确保驱动包的路径是正确的。

```
连接成功
服务正在运行: PONG
```

------

## Redis Java String(字符串) 实例

### 实例

```java
import redis.clients.jedis.Jedis;  
public class RedisStringJava {    
    public static void main(String[] args) {        
        //连接本地的 Redis 服务        
        Jedis jedis = new Jedis("localhost");       
        System.out.println("连接成功");        
        //设置 redis 字符串数据        
        jedis.set("runoobkey", "www.runoob.com");        
        // 获取存储的数据并输出        
        System.out.println("redis 存储的字符串为: "+ jedis.get("runoobkey"));    } }
```





编译以上程序。

```
连接成功
redis 存储的字符串为: www.runoob.com
```

------

## Redis Java List(列表) 实例

#### 实例

```java
import java.util.List;
import redis.clients.jedis.Jedis;
public class RedisListJava {   
    public static void main(String[] args) 
    {        //连接本地的 Redis 服务        
        Jedis jedis = new Jedis("localhost");       
        System.out.println("连接成功");        
        //存储数据到列表中        
        jedis.lpush("site-list", "Runoob");        
        jedis.lpush("site-list", "Google");        
        jedis.lpush("site-list", "Taobao");        
        // 获取存储的数据并输出        
        List<String> list = jedis.lrange("site-list", 0 ,2);       
        for(int i=0; i<list.size(); i++) {           
            System.out.println("列表项为: "+list.get(i));
        }   
    } 
}
```



编译以上程序。

```
连接成功
列表项为: Taobao
列表项为: Google
列表项为: Runoob
```

------

## Redis Java Keys 实例

### 实例

```java
import java.util.Iterator; 
import java.util.Set; 
import redis.clients.jedis.Jedis;  
public class RedisKeyJava {    
    public static void main(String[] args) {        
        //连接本地的 Redis 服务        
        Jedis jedis = new Jedis("localhost");        
        System.out.println("连接成功");         
        // 获取数据并输出        
        Set<String> keys = jedis.keys("*");         
        Iterator<String> it=keys.iterator() ;           
        while(it.hasNext()){               
            String key = it.next();               
            System.out.println(key);           
        }    
    } 
}
```





# 附录

## redis.conf 配置项：

| 序号 | 配置项                                                       | 说明                                                         |
| :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 1    | `daemonize no`                                               | Redis 默认不是以守护进程的方式运行，可以通过该配置项修改，使用 yes 启用守护进程（Windows 不支持守护线程的配置为 no ） |
| 2    | `pidfile /var/run/redis.pid`                                 | 当 Redis 以守护进程方式运行时，Redis 默认会把 pid 写入 /var/run/redis.pid 文件，可以通过 pidfile 指定 |
| 3    | `port 6379`                                                  | 指定 Redis 监听端口，默认端口为 6379，作者在自己的一篇博文中解释了为什么选用 6379 作为默认端口，因为 6379 在手机按键上 MERZ 对应的号码，而 MERZ 取自意大利歌女 Alessia Merz 的名字 |
| 4    | `bind 127.0.0.1`                                             | 绑定的主机地址                                               |
| 5    | `timeout 300`                                                | 当客户端闲置多长秒后关闭连接，如果指定为 0 ，表示关闭该功能  |
| 6    | `loglevel notice`                                            | 指定日志记录级别，Redis 总共支持四个级别：debug、verbose、notice、warning，默认为 notice |
| 7    | `logfile stdout`                                             | 日志记录方式，默认为标准输出，如果配置 Redis 为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给 /dev/null |
| 8    | `databases 16`                                               | 设置数据库的数量，默认数据库为0，可以使用SELECT 命令在连接上指定数据库id |
| 9    | `save <seconds> <changes>`Redis 默认配置文件中提供了三个条件：**save 900 1****save 300 10****save 60 10000**分别表示 900 秒（15 分钟）内有 1 个更改，300 秒（5 分钟）内有 10 个更改以及 60 秒内有 10000 个更改。 | 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合 |
| 10   | `rdbcompression yes`                                         | 指定存储至本地数据库时是否压缩数据，默认为 yes，Redis 采用 LZF 压缩，如果为了节省 CPU 时间，可以关闭该选项，但会导致数据库文件变的巨大 |
| 11   | `dbfilename dump.rdb`                                        | 指定本地数据库文件名，默认值为 dump.rdb                      |
| 12   | `dir ./`                                                     | 指定本地数据库存放目录                                       |
| 13   | `slaveof <masterip> <masterport>`                            | 设置当本机为 slave 服务时，设置 master 服务的 IP 地址及端口，在 Redis 启动时，它会自动从 master 进行数据同步 |
| 14   | `masterauth <master-password>`                               | 当 master 服务设置了密码保护时，slav 服务连接 master 的密码  |
| 15   | `requirepass foobared`                                       | 设置 Redis 连接密码，如果配置了连接密码，客户端在连接 Redis 时需要通过 AUTH <password> 命令提供密码，默认关闭 |
| 16   | ` maxclients 128`                                            | 设置同一时间最大客户端连接数，默认无限制，Redis 可以同时打开的客户端连接数为 Redis 进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis 会关闭新的连接并向客户端返回 max number of clients reached 错误信息 |
| 17   | `maxmemory <bytes>`                                          | 指定 Redis 最大内存限制，Redis 在启动时会把数据加载到内存中，达到最大内存后，Redis 会先尝试清除已到期或即将到期的 Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis 新的 vm 机制，会把 Key 存放内存，Value 会存放在 swap 区 |
| 18   | `appendonly no`                                              | 指定是否在每次更新操作后进行日志记录，Redis 在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis 本身同步数据文件是按上面 save 条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为 no |
| 19   | `appendfilename appendonly.aof`                              | 指定更新日志文件名，默认为 appendonly.aof                    |
| 20   | `appendfsync everysec`                                       | 指定更新日志条件，共有 3 个可选值：**no**：表示等操作系统进行数据缓存同步到磁盘（快）**always**：表示每次更新操作后手动调用 fsync() 将数据写到磁盘（慢，安全）**everysec**：表示每秒同步一次（折中，默认值） |
| 21   | `vm-enabled no`                                              | 指定是否启用虚拟内存机制，默认值为 no，简单的介绍一下，VM 机制将数据分页存放，由 Redis 将访问量较少的页即冷数据 swap 到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析 Redis 的 VM 机制） |
| 22   | `vm-swap-file /tmp/redis.swap`                               | 虚拟内存文件路径，默认值为 /tmp/redis.swap，不可多个 Redis 实例共享 |
| 23   | `vm-max-memory 0`                                            | 将所有大于 vm-max-memory 的数据存入虚拟内存，无论 vm-max-memory 设置多小，所有索引数据都是内存存储的(Redis 的索引数据 就是 keys)，也就是说，当 vm-max-memory 设置为 0 的时候，其实是所有 value 都存在于磁盘。默认值为 0 |
| 24   | `vm-page-size 32`                                            | Redis swap 文件分成了很多的 page，一个对象可以保存在多个 page 上面，但一个 page 上不能被多个对象共享，vm-page-size 是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page 大小最好设置为 32 或者 64bytes；如果存储很大大对象，则可以使用更大的 page，如果不确定，就使用默认值 |
| 25   | `vm-pages 134217728`                                         | 设置 swap 文件中的 page 数量，由于页表（一种表示页面空闲或使用的 bitmap）是在放在内存中的，，在磁盘上每 8 个 pages 将消耗 1byte 的内存。 |
| 26   | `vm-max-threads 4`                                           | 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4 |
| 27   | `glueoutputbuf yes`                                          | 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启 |
| 28   | `hash-max-zipmap-entries 64 hash-max-zipmap-value 512`       | 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法 |
| 29   | `activerehashing yes`                                        | 指定是否激活重置哈希，默认为开启（后面在介绍 Redis 的哈希算法时具体介绍） |
| 30   | `include /path/to/local.conf`                                | 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件 |

## Redis keys 命令

> 增删改查以及持久化

 键相关的基本命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **[DEL key](https://www.runoob.com/redis/keys-del.html) 该命令用于在 key 存在时删除 key。** |
| 2    | [DUMP key](https://www.runoob.com/redis/keys-dump.html) 序列化给定 key ，并返回被序列化的值。 |
| 3    | **[EXISTS key](https://www.runoob.com/redis/keys-exists.html) 检查给定 key 是否存在。** |
| 4    | **[EXPIRE key](https://www.runoob.com/redis/keys-expire.html) seconds 为给定 key 设置过期时间，以秒计**。 |
| 5    | [EXPIREAT key timestamp](https://www.runoob.com/redis/keys-expireat.html) EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。 |
| 6    | [PEXPIRE key milliseconds](https://www.runoob.com/redis/keys-pexpire.html) 设置 key 的过期时间以毫秒计。 |
| 7    | [PEXPIREAT key milliseconds-timestamp](https://www.runoob.com/redis/keys-pexpireat.html) 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计 |
| 8    | **[KEYS pattern](https://www.runoob.com/redis/keys-keys.html) 查找所有符合给定模式( pattern)的 key 。** |
| 9    | **[MOVE key db](https://www.runoob.com/redis/keys-move.html) 将当前数据库的 key 移动到给定的数据库 db 当中。** |
| 10   | **[PERSIST key](https://www.runoob.com/redis/keys-persist.html) 移除 key 的过期时间，key 将持久保持。** |
| 11   | PTTL key以毫秒为单位返回 key 的剩余的过期时间。              |
| 12   | [TTL key](https://www.runoob.com/redis/keys-ttl.html) 以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。 |
| 13   | **[RANDOMKEY](https://www.runoob.com/redis/keys-randomkey.html) 从当前数据库中随机返回一个 key** 。 |
| 14   | **[RENAME key newkey](https://www.runoob.com/redis/keys-rename.html) 修改 key 的名称** |
| 15   | **[RENAMENX key newkey](https://www.runoob.com/redis/keys-renamenx.html) 仅当 newkey 不存在时，将 key 改名为 newkey 。** |
| 16   | **[SCAN cursor [MATCH pattern\] [COUNT count]](https://www.runoob.com/redis/keys-scan.html) 迭代数据库中的数据库键。** |
| 17   | **[TYPE key](https://www.runoob.com/redis/keys-type.html) 返回 key 所储存的值的类型。** |

## Redis 字符串命令

下表列出了常用的 redis 字符串命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [SET key value](https://www.runoob.com/redis/strings-set.html) 设置指定 key 的值 |
| 2    | [GET key](https://www.runoob.com/redis/strings-get.html) 获取指定 key 的值。 |
| 3    | [GETRANGE key start end](https://www.runoob.com/redis/strings-getrange.html) 返回 key 中字符串值的子字符 |
| 4    | [GETSET key value](https://www.runoob.com/redis/strings-getset.html) 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。 |
| 5    | [GETBIT key offset](https://www.runoob.com/redis/strings-getbit.html) 对 key 所储存的字符串值，获取指定偏移量上的位(bit)。 |
| 6    | [MGET key1 [key2..\]](https://www.runoob.com/redis/strings-mget.html) 获取所有(一个或多个)给定 key 的值。 |
| 7    | [SETBIT key offset value](https://www.runoob.com/redis/strings-setbit.html) 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。 |
| 8    | [SETEX key seconds value](https://www.runoob.com/redis/strings-setex.html) 将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。 |
| 9    | [SETNX key value](https://www.runoob.com/redis/strings-setnx.html) 只有在 key 不存在时设置 key 的值。 |
| 10   | [SETRANGE key offset value](https://www.runoob.com/redis/strings-setrange.html) 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。 |
| 11   | [STRLEN key](https://www.runoob.com/redis/strings-strlen.html) 返回 key 所储存的字符串值的长度。 |
| 12   | [MSET key value [key value ...\]](https://www.runoob.com/redis/strings-mset.html) 同时设置一个或多个 key-value 对。 |
| 13   | [MSETNX key value [key value ...\]](https://www.runoob.com/redis/strings-msetnx.html) 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。 |
| 14   | [PSETEX key milliseconds value](https://www.runoob.com/redis/strings-psetex.html) 这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。 |
| 15   | [INCR key](https://www.runoob.com/redis/strings-incr.html) 将 key 中储存的数字值增一。 |
| 16   | [INCRBY key increment](https://www.runoob.com/redis/strings-incrby.html) 将 key 所储存的值加上给定的增量值（increment） 。 |
| 17   | [INCRBYFLOAT key increment](https://www.runoob.com/redis/strings-incrbyfloat.html) 将 key 所储存的值加上给定的浮点增量值（increment） 。 |
| 18   | [DECR key](https://www.runoob.com/redis/strings-decr.html) 将 key 中储存的数字值减一。 |
| 19   | [DECRBY key decrement](https://www.runoob.com/redis/strings-decrby.html) key 所储存的值减去给定的减量值（decrement） 。 |
| 20   | [APPEND key value](https://www.runoob.com/redis/strings-append.html) 如果 key 已经存在并且是一个字符串， APPEND 命令将指定的 value 追加到该 key 原来值（value）的末尾。 |

## Redis hash 命令

下表列出了 redis hash 基本的相关命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [HDEL key field1 [field2\]](https://www.runoob.com/redis/hashes-hdel.html) 删除一个或多个哈希表字段 |
| 2    | [HEXISTS key field](https://www.runoob.com/redis/hashes-hexists.html) 查看哈希表 key 中，指定的字段是否存在。 |
| 3    | [HGET key field](https://www.runoob.com/redis/hashes-hget.html) 获取存储在哈希表中指定字段的值。 |
| 4    | [HGETALL key](https://www.runoob.com/redis/hashes-hgetall.html) 获取在哈希表中指定 key 的所有字段和值 |
| 5    | [HINCRBY key field increment](https://www.runoob.com/redis/hashes-hincrby.html) 为哈希表 key 中的指定字段的整数值加上增量 increment 。 |
| 6    | [HINCRBYFLOAT key field increment](https://www.runoob.com/redis/hashes-hincrbyfloat.html) 为哈希表 key 中的指定字段的浮点数值加上增量 increment 。 |
| 7    | [HKEYS key](https://www.runoob.com/redis/hashes-hkeys.html) 获取所有哈希表中的字段 |
| 8    | [HLEN key](https://www.runoob.com/redis/hashes-hlen.html) 获取哈希表中字段的数量 |
| 9    | [HMGET key field1 [field2\]](https://www.runoob.com/redis/hashes-hmget.html) 获取所有给定字段的值 |
| 10   | [HMSET key field1 value1 [field2 value2 \]](https://www.runoob.com/redis/hashes-hmset.html) 同时将多个 field-value (域-值)对设置到哈希表 key 中。 |
| 11   | [HSET key field value](https://www.runoob.com/redis/hashes-hset.html) 将哈希表 key 中的字段 field 的值设为 value 。 |
| 12   | [HSETNX key field value](https://www.runoob.com/redis/hashes-hsetnx.html) 只有在字段 field 不存在时，设置哈希表字段的值。 |
| 13   | [HVALS key](https://www.runoob.com/redis/hashes-hvals.html) 获取哈希表中所有值。 |
| 14   | [HSCAN key cursor [MATCH pattern\] [COUNT count]](https://www.runoob.com/redis/hashes-hscan.html) 迭代哈希表中的键值对。 |

## Redis 列表命令

下表列出了列表相关的基本命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [BLPOP key1 [key2 \] timeout](https://www.runoob.com/redis/lists-blpop.html) 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 2    | [BRPOP key1 [key2 \] timeout](https://www.runoob.com/redis/lists-brpop.html) 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 3    | [BRPOPLPUSH source destination timeout](https://www.runoob.com/redis/lists-brpoplpush.html) 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 4    | [LINDEX key index](https://www.runoob.com/redis/lists-lindex.html) 通过索引获取列表中的元素 |
| 5    | [LINSERT key BEFORE\|AFTER pivot value](https://www.runoob.com/redis/lists-linsert.html) 在列表的元素前或者后插入元素 |
| 6    | [LLEN key](https://www.runoob.com/redis/lists-llen.html) 获取列表长度 |
| 7    | [LPOP key](https://www.runoob.com/redis/lists-lpop.html) 移出并获取列表的第一个元素 |
| 8    | [LPUSH key value1 [value2\]](https://www.runoob.com/redis/lists-lpush.html) 将一个或多个值插入到列表头部 |
| 9    | [LPUSHX key value](https://www.runoob.com/redis/lists-lpushx.html) 将一个值插入到已存在的列表头部 |
| 10   | [LRANGE key start stop](https://www.runoob.com/redis/lists-lrange.html) 获取列表指定范围内的元素 |
| 11   | [LREM key count value](https://www.runoob.com/redis/lists-lrem.html) 移除列表元素 |
| 12   | [LSET key index value](https://www.runoob.com/redis/lists-lset.html) 通过索引设置列表元素的值 |
| 13   | [LTRIM key start stop](https://www.runoob.com/redis/lists-ltrim.html) 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。 |
| 14   | [RPOP key](https://www.runoob.com/redis/lists-rpop.html) 移除列表的最后一个元素，返回值为移除的元素。 |
| 15   | [RPOPLPUSH source destination](https://www.runoob.com/redis/lists-rpoplpush.html) 移除列表的最后一个元素，并将该元素添加到另一个列表并返回 |
| 16   | [RPUSH key value1 [value2\]](https://www.runoob.com/redis/lists-rpush.html) 在列表中添加一个或多个值 |
| 17   | [RPUSHX key value](https://www.runoob.com/redis/lists-rpushx.html) 为已存在的列表添加值 |

## Redis 集合命令

下表列出了 Redis 集合基本命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [SADD key member1 [member2\]](https://www.runoob.com/redis/sets-sadd.html) 向集合添加一个或多个成员 |
| 2    | [SCARD key](https://www.runoob.com/redis/sets-scard.html) 获取集合的成员数 |
| 3    | [SDIFF key1 [key2\]](https://www.runoob.com/redis/sets-sdiff.html) 返回第一个集合与其他集合之间的差异。 |
| 4    | [SDIFFSTORE destination key1 [key2\]](https://www.runoob.com/redis/sets-sdiffstore.html) 返回给定所有集合的差集并存储在 destination 中 |
| 5    | [SINTER key1 [key2\]](https://www.runoob.com/redis/sets-sinter.html) 返回给定所有集合的交集 |
| 6    | [SINTERSTORE destination key1 [key2\]](https://www.runoob.com/redis/sets-sinterstore.html) 返回给定所有集合的交集并存储在 destination 中 |
| 7    | [SISMEMBER key member](https://www.runoob.com/redis/sets-sismember.html) 判断 member 元素是否是集合 key 的成员 |
| 8    | [SMEMBERS key](https://www.runoob.com/redis/sets-smembers.html) 返回集合中的所有成员 |
| 9    | [SMOVE source destination member](https://www.runoob.com/redis/sets-smove.html) 将 member 元素从 source 集合移动到 destination 集合 |
| 10   | [SPOP key](https://www.runoob.com/redis/sets-spop.html) 移除并返回集合中的一个随机元素 |
| 11   | [SRANDMEMBER key [count\]](https://www.runoob.com/redis/sets-srandmember.html) 返回集合中一个或多个随机数 |
| 12   | [SREM key member1 [member2\]](https://www.runoob.com/redis/sets-srem.html) 移除集合中一个或多个成员 |
| 13   | [SUNION key1 [key2\]](https://www.runoob.com/redis/sets-sunion.html) 返回所有给定集合的并集 |
| 14   | [SUNIONSTORE destination key1 [key2\]](https://www.runoob.com/redis/sets-sunionstore.html) 所有给定集合的并集存储在 destination 集合中 |
| 15   | [SSCAN key cursor [MATCH pattern\] [COUNT count]](https://www.runoob.com/redis/sets-sscan.html) 迭代集合中的元素 |

## Redis 有序集合命令

下表列出了 redis 有序集合的基本命令:

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [ZADD key score1 member1 [score2 member2\]](https://www.runoob.com/redis/sorted-sets-zadd.html) 向有序集合添加一个或多个成员，或者更新已存在成员的分数 |
| 2    | [ZCARD key](https://www.runoob.com/redis/sorted-sets-zcard.html) 获取有序集合的成员数 |
| 3    | [ZCOUNT key min max](https://www.runoob.com/redis/sorted-sets-zcount.html) 计算在有序集合中指定区间分数的成员数 |
| 4    | [ZINCRBY key increment member](https://www.runoob.com/redis/sorted-sets-zincrby.html) 有序集合中对指定成员的分数加上增量 increment |
| 5    | [ZINTERSTORE destination numkeys key [key ...\]](https://www.runoob.com/redis/sorted-sets-zinterstore.html) 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 destination 中 |
| 6    | [ZLEXCOUNT key min max](https://www.runoob.com/redis/sorted-sets-zlexcount.html) 在有序集合中计算指定字典区间内成员数量 |
| 7    | [ZRANGE key start stop [WITHSCORES\]](https://www.runoob.com/redis/sorted-sets-zrange.html) 通过索引区间返回有序集合指定区间内的成员 |
| 8    | [ZRANGEBYLEX key min max [LIMIT offset count\]](https://www.runoob.com/redis/sorted-sets-zrangebylex.html) 通过字典区间返回有序集合的成员 |
| 9    | [ZRANGEBYSCORE key min max [WITHSCORES\] [LIMIT]](https://www.runoob.com/redis/sorted-sets-zrangebyscore.html) 通过分数返回有序集合指定区间内的成员 |
| 10   | [ZRANK key member](https://www.runoob.com/redis/sorted-sets-zrank.html) 返回有序集合中指定成员的索引 |
| 11   | [ZREM key member [member ...\]](https://www.runoob.com/redis/sorted-sets-zrem.html) 移除有序集合中的一个或多个成员 |
| 12   | [ZREMRANGEBYLEX key min max](https://www.runoob.com/redis/sorted-sets-zremrangebylex.html) 移除有序集合中给定的字典区间的所有成员 |
| 13   | [ZREMRANGEBYRANK key start stop](https://www.runoob.com/redis/sorted-sets-zremrangebyrank.html) 移除有序集合中给定的排名区间的所有成员 |
| 14   | [ZREMRANGEBYSCORE key min max](https://www.runoob.com/redis/sorted-sets-zremrangebyscore.html) 移除有序集合中给定的分数区间的所有成员 |
| 15   | [ZREVRANGE key start stop [WITHSCORES\]](https://www.runoob.com/redis/sorted-sets-zrevrange.html) 返回有序集中指定区间内的成员，通过索引，分数从高到低 |
| 16   | [ZREVRANGEBYSCORE key max min [WITHSCORES\]](https://www.runoob.com/redis/sorted-sets-zrevrangebyscore.html) 返回有序集中指定分数区间内的成员，分数从高到低排序 |
| 17   | [ZREVRANK key member](https://www.runoob.com/redis/sorted-sets-zrevrank.html) 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 |
| 18   | [ZSCORE key member](https://www.runoob.com/redis/sorted-sets-zscore.html) 返回有序集中，成员的分数值 |
| 19   | [ZUNIONSTORE destination numkeys key [key ...\]](https://www.runoob.com/redis/sorted-sets-zunionstore.html) 计算给定的一个或多个有序集的并集，并存储在新的 key 中 |
| 20   | [ZSCAN key cursor [MATCH pattern\] [COUNT count]](https://www.runoob.com/redis/sorted-sets-zscan.html) 迭代有序集合中的元素（包括元素成员和元素分值） |

## Redis HyperLogLog 命令

下表列出了 redis HyperLogLog 的基本命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [PFADD key element [element ...\]](https://www.runoob.com/redis/hyperloglog-pfadd.html) 添加指定元素到 HyperLogLog 中。 |
| 2    | [PFCOUNT key [key ...\]](https://www.runoob.com/redis/hyperloglog-pfcount.html) 返回给定 HyperLogLog 的基数估算值。 |
| 3    | [PFMERGE destkey sourcekey [sourcekey ...\]](https://www.runoob.com/redis/hyperloglog-pfmerge.html) 将多个 HyperLogLog 合并为一个 HyperLogLog |

## Redis 发布订阅命令

下表列出了 redis 发布订阅常用命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [PSUBSCRIBE pattern [pattern ...\]](https://www.runoob.com/redis/pub-sub-psubscribe.html) 订阅一个或多个符合给定模式的频道。 |
| 2    | [PUBSUB subcommand [argument [argument ...\]]](https://www.runoob.com/redis/pub-sub-pubsub.html) 查看订阅与发布系统状态。 |
| 3    | [PUBLISH channel message](https://www.runoob.com/redis/pub-sub-publish.html) 将信息发送到指定的频道。 |
| 4    | [PUNSUBSCRIBE [pattern [pattern ...\]]](https://www.runoob.com/redis/pub-sub-punsubscribe.html) 退订所有给定模式的频道。 |
| 5    | [SUBSCRIBE channel [channel ...\]](https://www.runoob.com/redis/pub-sub-subscribe.html) 订阅给定的一个或多个频道的信息。 |
| 6    | [UNSUBSCRIBE [channel [channel ...\]]](https://www.runoob.com/redis/pub-sub-unsubscribe.html) 指退订给定的频道。 |

## Redis 事务命令

下表列出了 redis 事务的相关命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [DISCARD](https://www.runoob.com/redis/transactions-discard.html) 取消事务，放弃执行事务块内的所有命令。 |
| 2    | [EXEC](https://www.runoob.com/redis/transactions-exec.html) 执行所有事务块内的命令。 |
| 3    | [MULTI](https://www.runoob.com/redis/transactions-multi.html) 标记一个事务块的开始。 |
| 4    | [UNWATCH](https://www.runoob.com/redis/transactions-unwatch.html) 取消 WATCH 命令对所有 key 的监视。 |
| 5    | [WATCH key [key ...\]](https://www.runoob.com/redis/transactions-watch.html) 监视一个(或多个) key ，如果在事务执行之前这个(或这些) key 被其他命令所改动，那么事务将被打断。 |

## Redis 服务器命令

下表列出了 redis 服务器的相关命令:

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [BGREWRITEAOF](https://www.runoob.com/redis/server-bgrewriteaof.html) 异步执行一个 AOF（AppendOnly File） 文件重写操作 |
| 2    | [BGSAVE](https://www.runoob.com/redis/server-bgsave.html) 在后台异步保存当前数据库的数据到磁盘 |
| 3    | [CLIENT KILL [ip:port\] [ID client-id]](https://www.runoob.com/redis/server-client-kill.html) 关闭客户端连接 |
| 4    | [CLIENT LIST](https://www.runoob.com/redis/server-client-list.html) 获取连接到服务器的客户端连接列表 |
| 5    | [CLIENT GETNAME](https://www.runoob.com/redis/server-client-getname.html) 获取连接的名称 |
| 6    | [CLIENT PAUSE timeout](https://www.runoob.com/redis/server-client-pause.html) 在指定时间内终止运行来自客户端的命令 |
| 7    | [CLIENT SETNAME connection-name](https://www.runoob.com/redis/server-client-setname.html) 设置当前连接的名称 |
| 8    | [CLUSTER SLOTS](https://www.runoob.com/redis/server-cluster-slots.html) 获取集群节点的映射数组 |
| 9    | [COMMAND](https://www.runoob.com/redis/server-command.html) 获取 Redis 命令详情数组 |
| 10   | [COMMAND COUNT](https://www.runoob.com/redis/server-command-count.html) 获取 Redis 命令总数 |
| 11   | [COMMAND GETKEYS](https://www.runoob.com/redis/server-command-getkeys.html) 获取给定命令的所有键 |
| 12   | [TIME](https://www.runoob.com/redis/server-time.html) 返回当前服务器时间 |
| 13   | [COMMAND INFO command-name [command-name ...\]](https://www.runoob.com/redis/server-command-info.html) 获取指定 Redis 命令描述的数组 |
| 14   | [CONFIG GET parameter](https://www.runoob.com/redis/server-config-get.html) 获取指定配置参数的值 |
| 15   | [CONFIG REWRITE](https://www.runoob.com/redis/server-config-rewrite.html) 对启动 Redis 服务器时所指定的 redis.conf 配置文件进行改写 |
| 16   | [CONFIG SET parameter value](https://www.runoob.com/redis/server-config-set.html) 修改 redis 配置参数，无需重启 |
| 17   | [CONFIG RESETSTAT](https://www.runoob.com/redis/server-config-resetstat.html) 重置 INFO 命令中的某些统计数据 |
| 18   | [DBSIZE](https://www.runoob.com/redis/server-dbsize.html) 返回当前数据库的 key 的数量 |
| 19   | [DEBUG OBJECT key](https://www.runoob.com/redis/server-debug-object.html) 获取 key 的调试信息 |
| 20   | [DEBUG SEGFAULT](https://www.runoob.com/redis/server-debug-segfault.html) 让 Redis 服务崩溃 |
| 21   | [FLUSHALL](https://www.runoob.com/redis/server-flushall.html) 删除所有数据库的所有key |
| 22   | [FLUSHDB](https://www.runoob.com/redis/server-flushdb.html) 删除当前数据库的所有key |
| 23   | [INFO [section\]](https://www.runoob.com/redis/server-info.html) 获取 Redis 服务器的各种信息和统计数值 |
| 24   | [LASTSAVE](https://www.runoob.com/redis/server-lastsave.html) 返回最近一次 Redis 成功将数据保存到磁盘上的时间，以 UNIX 时间戳格式表示 |
| 25   | [MONITOR](https://www.runoob.com/redis/server-monitor.html) 实时打印出 Redis 服务器接收到的命令，调试用 |
| 26   | [ROLE](https://www.runoob.com/redis/server-role.html) 返回主从实例所属的角色 |
| 27   | [SAVE](https://www.runoob.com/redis/server-save.html) 同步保存数据到硬盘 |
| 28   | [SHUTDOWN [NOSAVE\] [SAVE]](https://www.runoob.com/redis/server-shutdown.html) 异步保存数据到硬盘，并关闭服务器 |
| 29   | [SLAVEOF host port](https://www.runoob.com/redis/server-slaveof.html) 将当前服务器转变为指定服务器的从属服务器(slave server) |
| 30   | [SLOWLOG subcommand [argument\]](https://www.runoob.com/redis/server-showlog.html) 管理 redis 的慢日志 |
| 31   | [SYNC](https://www.runoob.com/redis/server-sync.html) 用于复制功能(replication)的内部命令 |

## Redis 连接命令

下表列出了 redis 连接的基本命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [AUTH password](https://www.runoob.com/redis/connection-auth.html) 验证密码是否正确 |
| 2    | [ECHO message](https://www.runoob.com/redis/connection-echo.html) 打印字符串 |
| 3    | [PING](https://www.runoob.com/redis/connection-ping.html) 查看服务是否运行 |
| 4    | [QUIT](https://www.runoob.com/redis/connection-quit.html) 关闭当前连接 |
| 5    | [SELECT index](https://www.runoob.com/redis/connection-select.html) 切换到指定的数据库 |

## Redis 脚本命令

下表列出了 redis 脚本常用命令：

| 序号 | 命令及描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | [EVAL script numkeys key [key ...\] arg [arg ...]](https://www.runoob.com/redis/scripting-eval.html) 执行 Lua 脚本。 |
| 2    | [EVALSHA sha1 numkeys key [key ...\] arg [arg ...]](https://www.runoob.com/redis/scripting-evalsha.html) 执行 Lua 脚本。 |
| 3    | SCRIPT EXISTS script [script ...\]查看指定的脚本是否已经被保存在缓存当中。 |
| 4    | SCRIPT FLUSH 从脚本缓存中移除所有脚本。                      |
| 5    | SCRIPT KILL杀死当前正在运行的 Lua 脚本。                     |
| 6    | SCRIPT LOAD script将脚本 script 添加到脚本缓存中，但并不立即执行这个脚本。 |

## 客户端命令

| S.N. | 命令               | 描述                                       |
| :--- | :----------------- | :----------------------------------------- |
| 1    | **CLIENT LIST**    | 返回连接到 redis 服务的客户端列表          |
| 2    | **CLIENT SETNAME** | 设置当前连接的名称                         |
| 3    | **CLIENT GETNAME** | 获取通过 CLIENT SETNAME 命令设置的服务名称 |
| 4    | **CLIENT PAUSE**   | 挂起客户端连接，指定挂起的时间以毫秒计     |
| 5    | **CLIENT KILL**    | 关闭客户端连接                             |

## redis面试题汇总

### 　　**1、redis是什么？**

　　Redis是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。

### 　　**2、Redis有什么特点**

　　Redis是一个**Key-Value**类型的内存数据库，和memcached有点像，整个数据库都是在**内存**当中进行加载操作，定期通过**异步操作把数据库数据flush到硬盘上进行保存**。Redis的性能很高，可以处理超过10万次/秒的读写操作，是目前已知**性能最快的Key-Value DB**。

　　除了性能外，Redis还支持保存多种数据结构，此外单个value的最大限制是1GB，比memcached的1MB高太多了，因此Redis可以用来实现很多有用的功能，比方说用他的List来做FIFO双向链表，实现一个轻量级的高性 能消息队列服务，用他的Set可以做高性能的tag系统等等。另外Redis也可以对存入的Key-Value设置expire时间，因此也可以被当作一 个功能加强版的memcached来用。

　　当然，Redis也有缺陷，那就是是数据库容量受到物理内存的限制，不能用作海量数据的高性能读写，因此Redis比较适合那些局限在较小数据量的高性能操作和运算上。

### 　　**3.使用redis有哪些好处？**

　　(1) 速度快，因为数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)

　　(2) 支持丰富数据类型，支持string，list，set，sorted set，hash

　　(3) 支持事务，操作都是原子性，所谓的原子性就是对数据的更改要么全部执行，要么全部不执行

　　(4) 丰富的特性：可用于缓存，消息，按key设置过期时间，过期后将会自动删除



### 　　**4.redis相比memcached有哪些优势？**

　　(1) memcached所有的值均是简单的字符串，redis作为其替代者，支持更为丰富的数据类型

　　(2) redis的速度比memcached快很多 

​		(3) redis可以持久化其数据



### 　　**5.Memcache与Redis的区别都有哪些？**

　　1)、存储方式 Memecache把数据全部存在内存之中，断电后会挂掉，数据不能超过内存大小。 Redis有部份存在硬盘上，这样能保证数据的持久性。

　　2)、数据支持类型 Memcache对数据类型支持相对简单。 Redis有复杂的数据类型。

　　3)、使用底层模型不同 它们之间底层实现方式 以及与客户端之间通信的应用协议不一样。 Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。



### 　　**6.redis常见性能问题和解决方案：**

　　1).Master写内存快照，save命令调度rdbSave函数，会阻塞主线程的工作，当快照比较大时对性能影响是非常大的，会间断性暂停服务，所以Master最好不要写内存快照。

　　2).Master AOF持久化，如果不重写AOF文件，这个持久化方式对性能的影响是最小的，但是AOF文件会不断增大，AOF文件过大会影响Master重启的恢复速度。Master最好不要做任何持久化工作，包括内存快照和AOF日志文件，特别是不要启用内存快照做持久

　　化,如果数据比较关键，某个Slave开启AOF备份数据，策略为每秒同步一次。

　　3).Master调用BGREWRITEAOF重写AOF文件，AOF在重写的时候会占大量的CPU和内存资源，导致服务load过高，出现短暂服务暂停现象。

　　4). Redis主从复制的性能问题，为了主从复制的速度和连接的稳定性，Slave和Master最好在同一个局域网内



### 　　**7. mySQL里有2000w数据，redis中只存20w的数据，如何保证redis中的数据都是热点数据**

　　相关知识：redis 内存数据集大小上升到一定大小的时候，就会施行数据淘汰策略（回收策略）。redis 提供 6种数据淘汰策略：

　　volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰

　　volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰

　　volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰

　　allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰

　　allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰

　　no-enviction（驱逐）：禁止驱逐数据



### 　　**8.请用Redis和任意语言实现一段恶意登录保护的代码，限制1小时内每用户Id最多只能登录5次。具体登录函数或功能用空函数即可，不用详细写出。**

　　用列表实现:列表中每个元素代表登陆时间,只要最后的第5次登陆时间和现在时间差不超过1小时就禁止登陆.用Python写的代码如下：

```
#!/usr/bin/env python3
import redis  
import sys  
import time  
 
r = redis.StrictRedis(host=’127.0.0.1′, port=6379, db=0)  
try:       
    id = sys.argv[1]
except:      
    print(‘input argument error’)    
    sys.exit(0)  
if r.llen(id) >= 5 and time.time() – float(r.lindex(id, 4)) <= 3600:      
    print(“you are forbidden logining”)
else:       
    print(‘you are allowed to login’)    
    r.lpush(id, time.time())    
    # login_func()
```

### 　　**9.为什么redis需要把所有数据放到内存中?**

　　Redis为了达到最快的读写速度将数据都读到内存中，并通过异步的方式将数据写入磁盘。所以redis具有快速和数据持久化的特征。如果不将数据放在内存中，磁盘I/O速度为严重影响redis的性能。在内存越来越便宜的今天，redis将会越来越受欢迎。

　　如果设置了最大使用的内存，则数据已有记录数达到内存限值后不能继续插入新值。



### 　　**10.Redis是单进程单线程的**

　　redis利用队列技术将并发访问变为串行访问，消除了传统数据库串行控制的开销



### 　　**11.redis的并发竞争问题如何解决?**

　　Redis为单进程单线程模式，采用队列模式将并发访问变为串行访问。Redis本身没有锁的概念，Redis对于多个客户端连接并不存在竞争，但是在Jedis客户端对Redis进行并发访问时会发生连接超时、数据转换错误、阻塞、客户端关闭连接等问题，这些问题均是

　　由于客户端连接混乱造成。对此有2种解决方法：

　　1.客户端角度，为保证每个客户端间正常有序与Redis进行通信，对连接进行池化，同时对客户端读写Redis操作采用内部锁synchronized。

　　2.服务器角度，利用setnx实现锁。

　　注：对于第一种，需要应用程序自己处理资源的同步，可以使用的方法比较通俗，可以使用synchronized也可以使用lock；第二种需要用到Redis的setnx命令，但是需要注意一些问题。



### 　　**12.redis事物的了解CAS(check-and-set 操作实现乐观锁 )?**

　　和众多其它数据库一样，Redis作为NoSQL数据库也同样提供了事务机制。在Redis中，MULTI/EXEC/DISCARD/WATCH这四个命令是我们实现事务的基石。相信对有关系型数据库开发经验的开发者而言这一概念并不陌生，即便如此，我们还是会简要的列出

　　Redis中

　　事务的实现特征：

　　1). 在事务中的所有命令都将会被串行化的顺序执行，事务执行期间，Redis不会再为其它客户端的请求提供任何服务，从而保证了事物中的所有命令被原子的执行。

　　2). 和关系型数据库中的事务相比，在Redis事务中如果有某一条命令执行失败，其后的命令仍然会被继续执行。

　　3). 我们可以通过MULTI命令开启一个事务，有关系型数据库开发经验的人可以将其理解为"BEGIN TRANSACTION"语句。在该语句之后执行的命令都将被视为事务之内的操作，最后我们可以通过执行EXEC/DISCARD命令来提交/回滚该事务内的所有操作。这两

　　个Redis命令可被视为等同于关系型数据库中的COMMIT/ROLLBACK语句。

　　4). 在事务开启之前，如果客户端与服务器之间出现通讯故障并导致网络断开，其后所有待执行的语句都将不会被服务器执行。然而如果网络中断事件是发生在客户端执行EXEC命令之后，那么该事务中的所有命令都会被服务器执行。

　　5). 当使用Append-Only模式时，Redis会通过调用系统函数write将该事务内的所有写操作在本次调用中全部写入磁盘。然而如果在写入的过程中出现系统崩溃，如电源故障导致的宕机，那么此时也许只有部分数据被写入到磁盘，而另外一部分数据却已经丢失。

　　Redis服务器会在重新启动时执行一系列必要的一致性检测，一旦发现类似问题，就会立即退出并给出相应的错误提示。此时，我们就要充分利用Redis工具包中提供的redis-check-aof工具，该工具可以帮助我们定位到数据不一致的错误，并将已经写入的部

　　分数据进行回滚。修复之后我们就可以再次重新启动Redis服务器了。



### 　　**13.WATCH命令和基于CAS的乐观锁：**

　　在Redis的事务中，WATCH命令可用于提供CAS(check-and-set)功能。假设我们通过WATCH命令在事务执行之前监控了多个Keys，倘若在WATCH之后有任何Key的值发生了变化，EXEC命令执行的事务都将被放弃，同时返回Null multi-bulk应答以通知调用者事务

　　执行失败。例如，我们再次假设Redis中并未提供incr命令来完成键值的原子性递增，如果要实现该功能，我们只能自行编写相应的代码。其伪码如下：

```
　　val = GET mykey
　　val = val + 1
　　SET mykey $val
```

　　以上代码只有在单连接的情况下才可以保证执行结果是正确的，因为如果在同一时刻有多个客户端在同时执行该段代码，那么就会出现多线程程序中经常出现的一种错误场景--竞态争用(race condition)。比如，客户端A和B都在同一时刻读取了mykey的原有值，假设该值为10，此后两个客户端又均将该值加一后set回Redis服务器，这样就会导致mykey的结果为11，而不是我们认为的12。为了解决类似的问题，我们需要借助WATCH命令的帮助，见如下代码：

```
　　WATCH mykey
　　val = GET mykey
　　val = val + 1
　　MULTI
　　SET mykey $val
　　EXEC
```

　　和此前代码不同的是，新代码在获取mykey的值之前先通过WATCH命令监控了该键，此后又将set命令包围在事务中，这样就可以有效的保证每个连接在执行EXEC之前，如果当前连接获取的mykey的值被其它连接的客户端修改，那么当前连接的EXEC命令将执行失败。这样调用者在判断返回值后就可以获悉val是否被重新设置成功。

**
**

### 　　**14.redis持久化的几种方式**

　　1、快照（snapshots）

　　缺省情况情况下，Redis把数据快照存放在磁盘上的二进制文件中，文件名为dump.rdb。你可以配置Redis的持久化策略，例如数据集中每N秒钟有超过M次更新，就将数据写入磁盘；或者你可以手工调用命令SAVE或BGSAVE。

　　工作原理

　　． Redis forks.

　　． 子进程开始将数据写到临时RDB文件中。

　　． 当子进程完成写RDB文件，用新文件替换老文件。

　　． 这种方式可以使Redis使用copy-on-write技术。

　　2、AOF

　　快照模式并不十分健壮，当系统停止，或者无意中Redis被kill掉，最后写入Redis的数据就会丢失。这对某些应用也许不是大问题，但对于要求高可靠性的应用来说，

　　Redis就不是一个合适的选择。

　　Append-only文件模式是另一种选择。

　　你可以在配置文件中打开AOF模式

　　3、虚拟内存方式

　　当你的key很小而value很大时,使用VM的效果会比较好.因为这样节约的内存比较大.

　　当你的key不小时,可以考虑使用一些非常方法将很大的key变成很大的value,比如你可以考虑将key,value组合成一个新的value.

　　vm-max-threads这个参数,可以设置访问swap文件的线程数,设置最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的.可能会造成比较长时间的延迟,但是对数据完整性有很好的保证.

　　自己测试的时候发现用虚拟内存性能也不错。如果数据量很大，可以考虑分布式或者其他数据库



　　**15.redis的缓存失效策略和主键失效机制**

　　作为缓存系统都要定期清理无效数据，就需要一个主键失效和淘汰策略.

　　在Redis当中，有生存期的key被称为volatile。在创建缓存时，要为给定的key设置生存期，当key过期的时候（生存期为0），它可能会被删除。

　　1、影响生存时间的一些操作

　　生存时间可以通过使用 DEL 命令来删除整个 key 来移除，或者被 SET 和 GETSET 命令覆盖原来的数据，也就是说，修改key对应的value和使用另外相同的key和value来覆盖以后，当前数据的生存时间不同。

　　比如说，对一个 key 执行INCR命令，对一个列表进行LPUSH命令，或者对一个哈希表执行HSET命令，这类操作都不会修改 key 本身的生存时间。另一方面，如果使用RENAME对一个 key 进行改名，那么改名后的 key的生存时间和改名前一样。

　　RENAME命令的另一种可能是，尝试将一个带生存时间的 key 改名成另一个带生存时间的 another_key ，这时旧的 another_key (以及它的生存时间)会被删除，然后旧的 key 会改名为 another_key ，因此，新的 another_key 的生存时间也和原本的 key 一样。使用PERSIST命令可以在不删除 key 的情况下，移除 key 的生存时间，让 key 重新成为一个persistent key 。

　　2、如何更新生存时间

　　可以对一个已经带有生存时间的 key 执行EXPIRE命令，新指定的生存时间会取代旧的生存时间。过期时间的精度已经被控制在1ms之内，主键失效的时间复杂度是O（1），

　　EXPIRE和TTL命令搭配使用，TTL可以查看key的当前生存时间。设置成功返回 1；当 key 不存在或者不能为 key 设置生存时间时，返回 0 。

　　最大缓存配置

　　在 redis 中，允许用户设置最大使用内存大小

　　server.maxmemory

　　默认为0，没有指定最大缓存，如果有新的数据添加，超过最大内存，则会使redis崩溃，所以一定要设置。redis 内存数据集大小上升到一定大小的时候，就会实行数据淘汰策略。

　　redis 提供 6种数据淘汰策略：

　　． volatile-lru：从已设置过期时间的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰

　　． volatile-ttl：从已设置过期时间的数据集（server.db[i].expires）中挑选将要过期的数据淘汰

　　． volatile-random：从已设置过期时间的数据集（server.db[i].expires）中任意选择数据淘汰

　　． allkeys-lru：从数据集（server.db[i].dict）中挑选最近最少使用的数据淘汰

　　． allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰

　　． no-enviction（驱逐）：禁止驱逐数据

　　注意这里的6种机制，volatile和allkeys规定了是对已设置过期时间的数据集淘汰数据还是从全部数据集淘汰数据，后面的lru、ttl以及random是三种不同的淘汰策略，再加上一种no-enviction永不回收的策略。

　  使用策略规则：

　　1、如果数据呈现幂律分布，也就是一部分数据访问频率高，一部分数据访问频率低，则使用allkeys-lru

　　2、如果数据呈现平等分布，也就是所有的数据访问频率都相同，则使用allkeys-random

　　三种数据淘汰策略：

　　ttl和random比较容易理解，实现也会比较简单。主要是Lru最近最少使用淘汰策略，设计上会对key 按失效时间排序，然后取最先失效的key进行淘汰。