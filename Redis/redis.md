1. 什么是redis， 他可以用来干嘛？
    定义:redis是一个内存kv数据库
    应用:广泛的应用在缓存，也可以用在分布式锁,支持lua脚本， 持久化， 主从复制

2. redis的基本数据类型
    string
    list
    hash
    set
    zset

    bitmap
    hyperloglog
    geo

    进阶:
    string
        max=512M
        encoding:
            int 8byte 
            embstr <= 39 byte 
            raw > 39 byte
    list
        实现方式: ziplist hashtable
    hash
        实现方式:ziplist hashtable
    set
    zset
    bitmap
    hyperloglog
    geo

 3. redis 为什么那么快？
        基于内存
        单线程模型+多路复用
        合理编码

4. 什么是缓存击穿，缓存穿透，缓存雪崩
        client  -> redis -> mysql

        缓存穿透:
            查询一个key时，由于redis和mysql都没有， 每次请求都会请求到mysql

            出现原因:
                数据被删除了 ->  默认值 | 布隆过滤器(hash)
                非法请求 -> 接口入口处校验
        缓存击穿:
            热点key在redis中某个时间点过期， 瞬间会有大量的请求到db

            解决方案:永不过期
        缓存雪崩:
             数据库压力太大，或者宕机    
                     
5. redis的过期策略
    定时过期:
        key设置过期时间， 过期立刻就删除
        优点:瞬时
        缺点:频繁检测，消耗资源
    定期过期
        每隔一段时间，清理一波要过期的key
        优点:减少资源消耗
        缺点:不够实时
    惰性过期
        只有在访问时，才会检测是否过期
        优点:资源消耗极小
        缺点:某些不被访问的key可能永远不会被检测到

    redis使用的是:定期过期+惰性过期

6. redis的内存淘汰策略 - 当内存不足时通过以下策略
    volatile-lru  - 设置了过期key 
    allkeys-lru

    volatile-lfu  - 过期key 最不经常使用
    allkeys-lfu  -  所有key  最不经常使用

    volatile-random
    allkeys-random

    volatile-ttl

    noeviction














https://zhuanlan.zhihu.com/p/427496556