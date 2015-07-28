# 深入理解HashMap
我想大家对于HashMap并不陌生，程序员很多使用过该HashMap。估计很多公司面试的时候都会聊起HashMap，既然HashMap这么重要，今天我们就一起谈谈这个牛逼的HashMap。
>* HashMap概述及实现原理
>* HashMap的数据结构
>* HashMap的几个关键属性
>* HashMap的存取实现
>* fail-fast策略
>* Hash冲突以及如何解决hash冲突
>* HashMap和Hashtable区别

## HashMap概述 ##

>定义：HashMap实现了Map接口，继承自AbstractMap。其中Map接口定义了键映射到值的规则，而AbstractMap类提供 Map 接口的骨干实现，以最大限度地减少实现此接口所需的工作，其实AbstractMap类已经实现了Map，这里标注MapLZ觉得应该是更加清晰！

```java 
public class HashMap<K,V>
    extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable{

}
```
**实现原理:**
简单地说，hashmap的key做hash算法，并将hash值映射到内存地址，直接取得key对应的value。
HashMap的高性能需要保证以下几点：

- 将key hashd的算法必须是高效的
- hash值映射到内存地址（数组索引）的算法是快速的
- 根据内存地址（数组索引）可以直接取得对应的值

##HashMap的数据结构##
>hashmap的数据结构：在java语言中，最基本的数据结构就两种，一种是数组，另一种是模拟指针（引用），所有的数据结构都可以使用这两种数据结构构造，hashmap也是可以这样的。hashmap其实就是链表散列，是数组和链表的结合体。图片来自于[作者：egg](xtfggef@gmail.com)的文章。
![此处输入图片的描述][1]

观察hashmap的结构图，我们了解到hashmap底层是一个数组，数组中每一项是一个链表。HashMap提供了三个构造函数：

- HashMap()是一个默认的构造器，初始容量16，负载因子为0.75.
- HashMap(int initialCapacity)是一个指定初始容量为initialCapacity，负载因子为0.75的空的hashmap。
- HashMap(int initialCapacity, float loadFactor)是一个指定初始容量为initialCapacity，负载因子为loadFactor的空的hashmap。

我们查看一下hashmap的初始化源码：
```java
public HashMap() {
    this(DEFAULT_INITIAL_CAPACITY,DEFAULT_LOAD_FACTOR);
}
  
public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}

public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +loadFactor);
        this.loadFactor = loadFactor;
        threshold = initialCapacity;
        init();
}
```

##HashMap的几个关键属性##
#### 1，initialCapacity
#### 2，加载因子loadFactor

- initialCapacity为hashmap的最大容量，也就是底层数组的长度。
- loadFactor为加载因子，即散列表的实际元素数目(n)/ 散列表的容量(m)。另外，laodFactor越大，存储长度越小，查询时间越长。loadFactor越小，存储长度越大，查询时间短。hashmap默认的是0.75.负载因子衡量的是一个散列表的空间的使用程度，负载因子越大表示散列表的装填程度越高，反之愈小。对于使用链表法的散列表来说，查找一个元素的平均时间是O(1+a)。

##HashMap的存取实现##



##fail-fast策略##
> fail-fast策略：我们知道hashmap不是线程安全的，如果我们在使用迭代器过程中其他线程更改了map，就会抛出ConcurrentModificationException，这就是所谓fail-fast策略。

那么这个fail-fast策略是如何实现的呢？
这个策略主要是通过modCount的这个值实现的，modCount顾名思义就是hashmap的修改次数。每次在hashmap的内容被修改都会增加这个值，那么在hashmap的迭代器被初始化的都会将这个值赋值给expectedModCount。

```java 
HashIterator() {
    expectedModCount = modCount;
    if (size > 0) { // advance to first entry
        Entry[] t = table;
        while (index < t.length && (next = t[index++]) == null);
    }
}
```
在hashmap的迭代器执行的过程中，代码会判断modCount和expectedModCount的值，如果不相等则表示hashmap的其他线程修改。
```java
public final boolean hasNext() {
    return next != null;
}

final Entry<K,V> nextEntry() {
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
    Entry<K,V> e = next;
    if (e == null)
        throw new NoSuchElementException();

    if ((next = e.next) == null) {
        Entry[] t = table;
        while (index < t.length && (next = t[index++]) == null);
    }
    current = e;
    return e;
}
```

##Hash冲突以及如何解决hash冲突##


##HashMap和Hashtable区别##




[1]: http://img.my.csdn.net/uploads/201211/17/1353118778_2052.png