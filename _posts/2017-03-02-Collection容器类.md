---
layout:     post
title:      容器类和并发包
date:       2017-03-02
author:     ZWHT
header-img: img/post-bg-re-vs-ng2.jpg
catalog: true
tags:
    - Java
---

* TOC 
{:toc}

## 容器类

  最常用的是两个接口(Collection和Map)的实现类.以及Collections类(有一些公共方法,如sort,Iterator遍历)

![Alt text](/img/Collection/Collection_1.png)

实现Collection：   List和Set（无重复元素，无序，可以允许一个空值，用equals判断），Queue  ；
- Arraylist，Vector,LinkedList实现了List, Vector是线程安全synchronized关键字(Stringbuffer,Vector,HasTable是线程安全)
- Set分为TreeSet（SortedSet）和HashSet（必须保证对象的唯一性，所以要实现equals和hascode,是用Map实现的）
- Queue 分为PriorityQueue和Queue（LinkedList实现）
实现Map：分为HasMap和TreeMap

## Collection接口
### ArrayList

- 基于数组实现,初始Capacity为10,每次扩容1.5*(原来大小)+1
- 插入元素时需要移动元素.删除元素时不会减少数组容量
- 非线程安全

### LinkedList
- 基于双向链表实现.
- 非线程安全

### Vector
- 基于Synchronize实现线程安全

### HashSet
- 基于HasMap实现
- 非线程安全
- Set不允许元素重复

### TreeSet
- 基于TreeMap实现,支持排序
- 非线程安全

## Map接口

### HasMap
>由Entry<K,V> 构成的一个数组结构，若冲突时该数组位置就构成一个链表，Entry里由next构成单链表结构。

**容量:** 默认16.容量需为2的n次幂（可以通过hash值&（length-1）来取模来计算在数组中的索引，比除法运算快，而且在数组中分布均匀，减少冲突）
**阈值:** 默认12
**负载因子:**默认0.75
```java
Map<String,String> map=new HashMap<String,String>(5,0.6F);
//实际Entry数组大小为大于5的最小2次幂即8.
```
**put()**
- 判断是否为null
- 创建一个Entry对象,对key进行hash之后,hash&(size-1)得到key要存储的位置.
- 通过链表解决冲突并判断是否已经存在该key,有则覆盖,没有则头插法插入链表(`当链表个数超过8时,采用红黑树提高效率`)
- 如果此时Entry数组>=阈值.则扩容,并重新计算hash.

`注意:`
- 采用数组方式存储Entry对象.
- 基于hash寻找存放在数组的位置.堆hash冲突采用链表
- 如果用KeySet来遍历时,如果Map有新加或删除.则会抛出ConcurrentModificationException异常.(fast-fail快速失败)
- 非线程安全
- 插入时可能存在扩容.
- key和value允许null

##　hasmap和hastable（实现了map接口）
- 前者允许key和value为null，后者不行
- 前者初始为2的幂次，每次新增为原来的2倍。后者为11，新增为2*old+1（近似质数，冲突小）
- 前者线程不安全，后者线程安全（方法时synchronize的）
- 前者entry链表数据过多时，采用红黑树。
- 前者has值要计算，后者直接使用对象的hascode

## Hasmap和concurrenthasmap
- hasmap是不安全的，允许null作为key和value；
- con线程安全的，不允许null作为key和value；
- concurrenthasmap的并发度是segment的大小,默认是16(分为16个段)
- concurrenthasmap.size() .先使用无锁的方式求和,如果失败在使用加锁



