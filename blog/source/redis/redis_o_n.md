title: "Redis常用命令的时间复杂度整理"
date: 2018-03-26 20:37:11 +0800
update: 2018-03-26 20:53:32 +0800
author: me
tags:
    - Redis
    - Tech
preview: 整理了 Redis 常用命令的时间复杂度，以供项目参考～

---


> 背景

看 [redis 文档](http://redisdoc.com/ ) 的时候经常会看到某个命令的时间复杂度，突然发现自己这块知识点有点欠缺，现在做个总结

> 准备

- 参考资料：Redis 命令参考 http://redisdoc.com/
- 时间复杂度定义：https://zh.wikipedia.org/wiki/%E6%97%B6%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6

*在计算机科学中，算法的时间复杂度是一个函数，它定性描述了该算法的运行时间。这是一个代表算法输入值的字符串的长度的函数。时间复杂度常用大O符号表述，不包括这个函数的低阶项和首项系数。使用这种方式时，时间复杂度可被称为是渐近的，亦即考察输入值大小趋近无穷时的情况。例如，如果一个算法对于任何大小为 n （必须比 n0 大）的输入，它至多需要 5n3 + 3n 的时间运行完毕，那么它的渐近时间复杂度是 O(n3)*

> 开始

Redis  的命令按照常用的数据结构分为六种类型：

- Key 
- String
- Hash
- List
- Set
- SortedSet

本文主要整理以上 6 类的最常使用的命令，全部的命令建议查看上文的文档。

下面分别整理如下

###  Key
命令 |用法| 作用 | 时间复杂度 | 解释 | 其他 
---|---|---|---|---|---
DEL|`DEL key [key ...]` |删除给定的key  |`O(N)` |字符串`O(1)`,其他结构`O(M)`,`M`是集合的元素个数  
EXISTS|`EXISTS key` |检查给定`key`是否存在  |`O(1)`|   
EXPIRE|`EXPIRE key seconds` |为给定`key` 设置生存时间|`O(1)`|  |参数是秒 
EXPIREAT|`EXPIREAT key timestamp` |为给定`key` 设置生存时间|`O(1)`||参数是秒时间戳 
KEYS|`KEYS pattern` |查找所有符合给定模式`pattern` 的`key`|`O(N)` | `N`为数据库中 key 的数量 | 
PERSIST|`PERSIST key` |移除给定`key`的生存时间|`O(1)` |  |持久某个`key` 
PEXPIRE|`PEXPIRE key milliseconds` |为给定`key` 设置生存时间|`O(1)` |  | 参数是毫秒
PEXPIREAT|`PEXPIREAT key milliseconds-timestamp` |为给定`key` 设置生存时间|`O(1)` |   参数是毫秒时间戳
PTTL|`PTTL key` |返回某个`key`的生存时间|`O(1)` |  | 参数是毫秒
TTL|`TTL key` |返回某个`key`的生存时间|`O(1)` |  | 参数是秒
TYPE|`TYPE key` |返回某个`key`的所储存的值的类型|`O(1)` |  | 


###  String

###  Hash

###  List

###  Set

###  SortedSet


