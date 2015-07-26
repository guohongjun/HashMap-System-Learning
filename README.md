# 深入理解HashMap
我想大家对于HashMap并不陌生，程序员很多使用过该HashMap。估计很多公司面试的时候都会聊起HashMap，既然HashMap这么重要，今天我们就一起谈谈这个牛逼的HashMap。
>* HashMap概述及实现原理
>* HashMap的数据结构
>* HashMap的几个关键属性
>* HashMap的存取实现
>* fail-fast策略
>* Hash冲突以及如何解决hash冲突



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

##HashMap的几个关键属性##
##HashMap的存取实现##
##fail-fast策略##
##Hash冲突以及如何解决hash冲突##