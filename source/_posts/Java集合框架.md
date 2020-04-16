---
title: Java集合框架
date: 2020-04-08 
tags: Java
---

### 1. ArrayList 和LinkedList异同

* **是否保证线程安全**：ArrayList和LinkedList都是不同步的，也就是不保证线程安全
* **底层数据结构**：ArrayList底层使用的是Object数组，LinkedList底层使用的是双向链表数据结构（JDK1.6之前为循环链表，JDK1.7取消了循环）

![](https://images.cnitblog.com/blog/556852/201308/17200016-b868ad987c4947ceb76c42b44887c783.png)
<!--more-->

* **插入和删除是否受元素位置的影响：**
  * ArrayList采用数组存储，所以插入和删除元素的时间复杂度受元素位置影响，时间复杂度近似O(n)
  * LinkedList采用链表存储，所以插入删除元素时间复杂度不受元素位置的影响，时间复杂度近似O(1)
* **是否支持快速访问：**LinkedList不支持高效的随机元素访问，而ArrayList支持。（快速访问就是通过元素的序号快速获取元素对象（对应于`get(int index)`方法））
* **内存空间占用：**
  * ArrayList的空间浪费主要体现在list列表的结尾会预留一定的容量空间
  * LinkedList的空间浪费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）

**补充：RandomAccess接口**

```java
public interface RandomAccess{
}
```

查看源码，我们发现实际上`RandomAccess`接口中什么也没有定义。所以RandomAccess接口不过是一个标识罢了。标识什么？标识实现这个接口的类具有随机访问功能。

在`binarySearch()`方法中，他要判断传入的list是否`RandomAccess`的实例，如果是，调用`indexedBinarySearch()`方法，如果不是，调用`iteratorBinarySearch()`方法

```java
public static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key) { 
  if (list instanceof RandomAccess || list.size()<BINARYSEARCH_THRESHOLD) 
    return Collections.indexedBinarySearch(list, key); 
  else
    return Collections.iteratorBinarySearch(list, key);
}
```

ArrayList 实现了 RandomAccess 接口， 而 LinkedList 没有实现。为什么呢？我觉得还是和底层数据结构有关！

ArrayList 底层是数组，而 LinkedList 底层是链表。数组天然支持随机访问，时间复杂度为 O（1），所以称为快速随机访问。链表需要遍历到特定位置才能访问特定位置的元素，时间复杂度为 O（n），所以不支持快速随机访问。ArrayList 实现了 RandomAccess 接口，就表明了他具有快速随机访问功能。 RandomAccess 接口只是标识，并不是说 ArrayList 实现 RandomAccess 接口才具有快速随机访问功能的！

**下面再总结一下** **list** **的遍历方式选择：**

* 实现了RandomAccess接口的list，优先选择普通for循环 ，其次foreach,

* 未实现RandomAccess接口的list， 优先选择iterator遍历（foreach遍历底层也是通过iterator实现的），大

size的数据，千万不要使用普通for循环



### 2. **ArrayList** **与** **Vector** 区别

* Vector类的所有方法都是同步的。可以由两个线程安全地访问一个Vector对象、但是一个线程访问Vector的话代码要在同步操作上耗费大量的时间。

* Arraylist不是同步的，所以在不需要保证线程安全时建议使用Arraylist。



### 3.HashMap的底层实现

#### JDK1.8之前

JDK1.8 之前 HashMap 底层是 **数组和链表** 结合在一起使用也就是 **链表散列**。**HashMap** **通过** **key** **的** **hashCode** **经过扰动函数处理过后得到** **hash** **值，然后通过** `(n - 1) & hash` **判断当前元素存放的位置（这里的** **n** **指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的** **hash** **值以及** **key** **是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。**

> **所谓扰动函数指的就是** **HashMap** **的** **hash** **方法。使用** **hash** **方法也就是扰动函数是为了防止一些实现比较差的**hashCode() **方法 ,换句话说使用扰动函数之后可以减少碰撞。**

**JDK 1.8 HashMap** **的** **hash** **方法源码**

JDK 1.8 的hash方法相比于 JDK 1.7 hash方法更加简化，但是原理不变。

```java
static final int hash(Object key) { 
  int h; 
  // key.hashCode()：返回散列值也就是hashcode 
  // ^ ：按位异或 
  // >>>:无符号右移，忽略符号位，空位都以0补齐 
  return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16); 
}
```

对比一下 JDK1.7的 HashMap 的 hash 方法源码.

```java
static int hash(int h) {
  // This function ensures that hashCodes that differ only by 
  // constant multiples at each bit position have a bounded 
  // number of collisions (approximately 8 at default load factor). 
  h ^= (h >>> 20) ^ (h >>> 12); return h ^ (h >>> 7) ^ (h >>> 4); 
}
```

相比于 JDK1.8 的 hash 方法 ，JDK 1.7 的 hash 方法的性能会稍差一点点，因为毕竟扰动了 4 次。

> 所谓 **“拉链法”** 就是：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

![image-20200105194913491](/Users/ricardo/Library/Application Support/typora-user-images/image-20200105194913491.png)

#### JDK1.8之后

相比于之前的版本， JDK1.8之后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。

![image-20200105194953598](/Users/ricardo/Library/Application Support/typora-user-images/image-20200105194953598.png)

> TreeMap、TreeSet以及JDK1.8之后的HashMap底层都用到了红黑树。红黑树就是为了解决二叉查找树的缺
>
> 陷，因为二叉查找树在某些情况下会退化成一个线性结构。

**推荐阅读**

* 《Java 8系列之重新认识HashMap》 ：https://zhuanlan.zhihu.com/p/21673805

### 4.HashMap和HashTable的区别

1. **线程是否安全：** HashMap 是非线程安全的，HashTable 是线程安全的；HashTable 内部的方法基本都经过

`synchronized` 修饰。（如果你要保证线程安全的话就使用 ConcurrentHashMap 吧！）

2. **效率：** 因为线程安全的问题，HashMap 要比 HashTable 效率高一点。另外，HashTable 基本被淘汰，不要在代码中使用它；

3. **对Null key 和Null value的支持：** HashMap 中，null 可以作为键，这样的键只有一个，可以有一个或多个键所对应的值为 null。但是在 HashTable 中 put 进的键值只要有一个 null，直接抛出 `NullPointerException`。 

4. **初始容量大小和每次扩充容量大小的不同 ：** 

   ①创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1。HashMap 默认的初始化大小为16。之后每次扩充，容量变为原来的2倍。

   ②创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为2的幂次方大小（HashMap 中的 tableSizeFor() 方法保证）。也就是说 HashMap 总是使用2的幂作为哈希表的大小,后面会介绍到为什么是2的幂次方。

5. **底层数据结构：** JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为

8）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。



### 5.HashMap的长度为什么是2的幂次方

​		HashMap为了存取高效，要尽量较少碰撞，就是要尽量把数据分配均匀，每个链表长度大致相同，这个实现就在把数据存到哪个链表中的算法。
​		这个算法实际就是取模，`hash%length`，计算机中直接求余效率不如位移运算，源码中做了优化`hash&(length-1)`，`hash%length`==`hash&(length-1)`的前提是length是2的n次方；这就解释了`HashMap`的长度为什么是2的幂次方。

### 6.HashSet和HashMap的区别

如果你看过HashSet源码就应该知道：HashSet底层就是基于HashMap实现到。（HashSet源码非常非常少，因为除了clone()方法、writeObject()方法、readObject()方法是HashSet自己不得不实现之外，其他方法都是直接调用HashMap中的方法。）

![image-20200105202437786](/Users/ricardo/Library/Application Support/typora-user-images/image-20200105202437786.png)

### 7.ConcurrentHashMap和HashTable的区别

* **底层数据结构：**JDK1.7以前的`ConcurrentHashMap`底层采用 **分段的数组+链表**实现，JDK1.8采用的数据结构跟HashMap1.8的结构一样，数组+链表/红黑二叉树。HashTable和JDK1.8之前的HashMap的底层数据结构类似都是采用 **数组+链表** 的形式，数组是HashMap的主体，链表则是为了解决哈希冲突而存在的。

* **实现线程安全的方式（重要）：**

  * **在JDK1.7的时候，ConcurrentHashMap(分段锁)** 对整个桶数组进行了分割分段（Segment），每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发率。 **到了JDK1.8的时候已经摒弃了Segment的概念，而是直接用Node数组 + 链表 + 红黑树的数据结构来实现，并发控制使用synchronized和 CAS来操作。（JDK1.6以后对synchronized锁做了很多优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销）**整个看起来就像是优化过且线程安全的HashMap，虽然在JDK1.8中还能看到Segment的数据结构，但是已经简化了属性，只是为了兼容旧版本
  * **HashTable（同一把锁）：** 使用synchronized来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用put添加元素，另一个线程不能使用put添加元素，也不能用get，竞争会越来越激烈，效率越低

  

  **两者的对比图：**

  ![image-20200105204126728](/Users/ricardo/Library/Application Support/typora-user-images/image-20200105204126728.png)

  ![image-20200105204142062](/Users/ricardo/Library/Application Support/typora-user-images/image-20200105204142062.png)

  ![image-20200105204217831](/Users/ricardo/Library/Application Support/typora-user-images/image-20200105204217831.png)

  

### 8.ConcurrentHashMap线程安全的具体实现方式/底层具体实现

#### JDK1.7（参考上面示意图）

首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。

**ConcurrentHashMap是由Segment数组结构和HashEntry数组结构组成。**

Segment实现了ReentrantLock，所以Segment是一种可重入锁，扮演锁定角色。HashEntry用于存储键值对数据。

```java
static class Segment<K,V> extends ReentrantLock implements Serializable{
}
```

一个ConcurrentHashMap包含一个Segment数组。Segment的结构和HashMap类似，是一种数组加链表结构，一个Segment包含一个HashEntry数组，每个HashEntry是一个链表结构元素，每个Segment守护着HashEntry数组里的元素，当对HashEntry数组里的数据进行修改时，必须首先获得对应的Segment锁。

#### JDK1.8（参考上面示意图）

ConcurrentHashMap取消了Segment分段锁，采用CAS和synchronized来保证并发安全。数据结构和HashMap 1.8的结构类似，数组+ 链表/红黑二叉树。

synchronized只锁定当前链表或者红黑二叉树的首节点，这样只要hash不冲突，就不会产生并发，效率提升N倍。

### 9.集合框架底层数据结构总结

#### Collection

**1.List**

* **ArrayList：**Object数组
* **Vector:** Object数组
* **LinkedList：**双向链表（JDK1.6之前为循环链表，JDK1.7取消了循环）

**2.Set**

* **HashSet（无序，唯一）：**基于HashMap实现的，底层采用HashMap来保存元素
* **LinkedHashSet：**LinkedHashSet继承于HashSet，并且内部是通过LinkedHashMap来实现的。
* **TreeSet（有序，唯一）：**红黑树（自平衡的排序二叉树）

#### Map

* **HashMap：**JDK1.8前HashMap由数组+链表组成的，数组是HashMap的主体，链表则是为了解决哈希冲突而存在的（“拉链法”解决冲突）。JDK1.8以后在解决哈希冲突时，有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。
* **LinkedHashMap：**LinkedHashMap继承自HashMap，所以它的底层仍然还是基于拉链式的散列式结构，即数组和链表/红黑树组成。另外，LinkedHashMap在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时对链表进行相应的操作，实现了访问顺序相关逻辑。
* **HashTable：**数组+链表组成，数组是HashTable的主体，链表则是为了解决哈希冲突而存在的，线程安全
* **TreeMap：**红黑树（自平衡的排序二叉树）