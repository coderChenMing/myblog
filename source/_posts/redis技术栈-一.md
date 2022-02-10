---
title: redis技术栈(一)
date: 2020-12-16 18:40:11
updated: 2020-12-16 18:40:11
tags: redis
categories: 缓存
---
## 认识redis
    Redis 是一种开源（BSD 许可）、内存中数据结构存储，用作数据库、缓存和消息代理。 
    Redis 提供了数据结构，例如字符串、散列、列表、集合、
    带有范围查询的排序集合(sorted sets with range queries)、
    位图、超级日志、地理空间索引(geospatial indexes)和流。
    Redis (built-in replication)内置复制、Lua 脚本、
    LRU 驱逐(LRU eviction)、事务(transactions)
    和不同级别的磁盘持久化，
    并通过 Redis Sentinel 和 
    Redis Cluster 自动分区(automatic partitioning)提供高可用性。
    官网地址:redis.io
    特点:开源(BSD licensed),使用内存数据结构存储
    用途:数据库,缓存,消息代理(message broker)

## 下载安装
    旧版本地址https://download.redis.io/releases/

## 键值存储
    键值存储的本质是能够在键内存储一些称为值的数据。
    只有当我们知道用于存储它的确切密钥时，才能稍后检索这些数据。 
    Redis 通常将它称为数据结构服务器，因为它具有外部键值外壳，
    但是每个值都可以包含复杂的数据结构，例如字符串、列表、散列
    或称为排序集以及概率的有序数据结构 像 hyperloglog 这样的数据结构。
## 命令展示
    Redis 提供的其他基本操作是 DEL 删除给定的键和关联的值，
    INCR 以原子方式递增存储在给定键上的数字：
    DECR 以原子方式递减存储在给定键上的数字：
    当您使用递增和递减命令操作 Redis 字符串时，您正在实现计数器。
    计数器是 Redis 非常流行的应用程序。
    
    单个命令实现的所有Redis操作都是原子的，包括对更复杂数据结构的操作。 
    因此，当您使用修改某些值的 Redis 命令时，您不必考虑并发访问。
    
    Redis 可以被告知一个键应该只存在一定的时间长度。 
    这是通过 EXPIRE 和 TTL 命令以及类似的 PEXPIRE 和 PTTL 命令来实现的，
    这些命令使用时间以毫秒而不是秒为单位进行操作。
    eg: SET resource:lock "Redis Demo"
        EXPIRE resource:lock 120