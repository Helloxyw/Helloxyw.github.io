---

title: 剑指offer
date: 2019-05-15 21:15:53
tags: Redis

---









![](data:image/png;base64,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)







<h3>4.Redis</h3>

- 数据类型丰富
- 支持数据磁盘持久化存储
- 支持主从
- 支持分片

![](https://i.postimg.cc/sfnpFq6R/redis.png)

<!-- more-->

<h3>为什么Redis能这么快</h3>

<h4>支持10000+QPS（query per second,每秒内查询次数）</h4>


* 完全基于内存，绝大部分请求是纯粹的内存操作，执行效率高
* 数据结构简单，对数据操作也简单
* 主线程采用单线程，单线程也能处理高并发请求，想多核也可启动多实例

<h3>供用户使用的数据类型</h3>

* String : 最基本的数据类型，二进制安全

* Hash: String 元素组成的字典，适合用于存储对象

* List: 列表，按照String元素插入排序

* Set: String元素组成的无序集合，通过哈希表实现，不允许重复

* Sorted Set : 通过分数来为集合中的成员进行从小到大的排序

* 用于计数的HyperLongLong，用于支持存储地理位置信息的Geo

  

<h3>问题1：从海量key里查询出某一固定前缀的key</h3>

<b>留意细节</b>

* 摸清数据规模，即问清边界

1. 方案1:`KEYS pattern`: 查找所有符合给定模式pattern的key

   存在的问题：

   * KEYS指令一次性返回所有匹配的key
   * 键的数量过大会使服务卡顿

2. 方案2:`SCAN cursor [MATCH pattern][COUNT count]`

   * 基于游标的迭代器，需要基于上一次的游标延续之前的迭代过程
   * 以0作为游标开始一次新的迭代，直到命令返回游标0完成一次遍历
   * 不保证每次执行都返回某个给定数量的元素，支持模糊查询
   * 一次返回的数量不可控，只能是大概率符合count参数



<h3>问题2：如何通过Redis实现分布式锁</h3>

> 分布式锁：是用来控制分布式系统中互斥访问共享资源的一种手段，从而避免并行导致的结果不可控。基本的实现原理和单进程锁是一致的，通过一个共享标识来确定唯一性，对共享标识进行修改时能保证原子性和对锁服务调用方的可见性。
>
> 分布式锁一般要能保证：
>
> * 同一时刻只能有一个线程持有锁
> * 锁能够可重入
> * 不会发生死锁
> * 具备阻塞锁特性，且能够及时从阻塞状态唤醒
> * 锁服务保证高性能和高可用

**分布式锁需要解决的问题**

* 互斥性
* 安全性
* 死锁
* 容错



#### 方案一：

`SETNX key value`：如果key不存在，则创建并赋值

* 时间复杂度`O(1)`
* 返回值：设置成功，返回1；设置失败，返回0

**其中需要解决SETNX长期有效问题**

`EXPIRE key seconds`

*  设置key的生存时间，当key过期时（生命时间为0），会被自动删除

**该方案一的伪代码实现**

```java
RedisService redisService = SpringUtils.getBean(RedisService.class);
long status = redisService.setnx(key,"1");

if(status == 1){
  //设置key的生存时间
  redisService.expire(key,expire);
  //执行独占资源逻辑
  doOcuppiedWork();
}
```

**缺点：原子性得不到满足**

#### 方案二：

`SET key value [EX seconds][PX milliseconds][NX|XX] `

* EX seconds :设置键的过期时间为second秒
* PX milliseconds： 设置键的过期时间为milliseconds毫秒
* NX:只在键不存在时，才对键进行设置操作
* XX:只在键已经存在时，才对键进行设置操作
* SET操作成功完成时，返回OK,否则返回nil

**该方案二的伪代码实现**

```java
RedisService redisService = SpringUtils.getBean(RedisService.class);
String result = redisService.set(localKey,requestId,SET_IF_NOT_EXIST,SET_WITH_EXPIRE_TIME,expireTime);
if("OK".equals(result)){
  //执行独占资源逻辑
  doOcuppiedWork();
}
```



<h4>大量的key同时过期的注意事项</h4>

**集中过期，由于清除大量的key很耗时，会出现短暂的卡顿现象**

* 解决方案：在设置key的过期时间的时候，给每个key加上随机值



<h3>问题3:如何使用Redis做异步队列</h3>

**方案一 **

使用`List`做队列，`RPUSH`生产消息，`LPOP`消费消息

* 缺点：没有等待队列里有值就直接消费
* 弥补：可以在应用层引入`Sleep`机制去调用`LPOP`重试

**方案二**

`BLPOP key[key...] timeout`:阻塞直到消息队列有消息或者超时

* 缺点：只能供一个消费者消费



**方案三**

`PUB/SUB` :主题订阅者模式

![](https://i.postimg.cc/KjDWk3RC/WX20190512-011155-2x.png)

* 发送者（`PUB`）发送消息，订阅者（`SUB`）接收消息

* 订阅者可以订阅任意数量的频道

* 缺点：消息的发布是无状态的，无法保证可达（需要专业的消息队列如`kafka`)



<h3>问题4:Redis如何做持久化</h3>



**方案一**

**RDB（快照）持久化：保存某个时间点的全量数据快照**

* `SAVE`: 阻塞Redis的服务器进程，直到RDB文件被创建完毕
* **`BGSAVE`: `FORK`出一个子进程来创建RDB文件，不阻塞服务器进程**



**`RDB`持久化缺点：** 

* 内存数据的全量同步，数据量大会由于`I/O`而严重影响性能
* 可能会因为Redis挂掉而丢失从当前至最近一次快照期间的数据



**方案二**

**AOF(`Append-only- File`)持久化：保存写状态**

* 记录下除了查询以外的所有变更数据数据库状态的指令
* 以append的形式追加保存到AOF文件中（增量）



<h3>问题5:使用Pipeline的好处</h3>

* `Pipeline`和Linux的管道类似
* Redis基于请求/响应模型，单个请求处理需要一一应答
* `Pipeline`批量执行指令，节省多次I/O往返时间
* 有顺序依赖的指令建议分批发送



<h3>问题6:Redis的同步机制</h3>

[![image.png](https://i.postimg.cc/mk7FWKB8/image.png)](https://postimg.cc/9wFfG847)

**全同步原理**

1. Salve发送sync命令到Master
2. Master启动一个后台进程，将redis中的数据快照保存到文件中
3. Master将保存数据快照期间接收到的写命令缓存起来
4. Master完成文件写操作后，将该文件发送给Slav e
5. 使用新的AOF文件替换掉旧的AOF文件
6. Master将这期间收集的增量写命令发送给Slave端



**增量同步机制**

1. Master接收到用户的操作指令，判断是否需要传播到Slave
2. 将操作记录追加到AOF文件
3. 将操作传播到其他Slave: 1、对齐主从库；2、往响应缓存写入指令
4. 将缓存中的数据发送给Slave



**Redis Sentinel**

**解决主从同步Master宕机后的主从切换问题**

* 监控：检查主从服务器是否运行正常
* 提醒：通过API向管理员或者其他应用程序发送故障通知
* 自动故障迁移：主从切换



**流言协议Gossip**

> 在杂乱无章中寻求一致

* 每个节点都随机地与对方通信，最终所有节点的状态达成一致
* 种子节点定期随机向其他节点发送节点列表以及需要传播的消息
* 不保证信息一定会传递给所有节点，但是最终会趋于一致



<h3>Redis的集群原理</h3>

**如何从海量数据里快速找到所需**

* 分片：按照某种规则去划分数据，分散存储在多个节点上
*  常规的按照哈希划分无法实现节点的动态增减

**一致性哈希算法**

1. 对2^32取模，将哈希值空间组织成虚拟的圆环

2. 将数据key使用相同的函数Hash计算出哈希值

   ![](https://i.postimg.cc/7YQRzk9d/WX20190515-210010-2x.png)

3. Hash环数据倾斜问题

   [![WX20190515-210423-2x.png](https://i.postimg.cc/q79h6z6q/WX20190515-210423-2x.png)](https://postimg.cc/WDwbQpnc)

4. 引入虚拟节点解决数据倾斜问题

   [![WX20190515-210621-2x.png](https://i.postimg.cc/y6n1wD6R/WX20190515-210621-2x.png)](https://postimg.cc/hhzRmPd4)



<h3>6.JAVA底层知识</h3>

![](http://img0.pconline.com.cn/pconline/1303/12/3208685_java-01.jpg)

<h4>6.1谈谈你对Java的理解</h4>

* 平台无关性（一次编译，到处运行）
* GC(垃圾回收机制)
* 语言特性（范型、反射、lmbda表达式）
* 面向对象（继承、封装、多态）
* 类库（集合、并发库、网络库、IO、NIO）
* 异常处理



<h4>6.2平台无关性如何实现</h4>

[![WX20190617-235712-2x.png](https://i.postimg.cc/6qbwrd9r/WX20190617-235712-2x.png)](https://postimg.cc/jnfGKnn2)

**为什么JVM不直接将源码解析成机器码去执行**

* 准备工作：每次执行前都需要各种检查
* 兼容性：也可以将别的语言解析成字节码



<h4>6.3 JVM如何加载.class文件</h4>

JVM之所以存在的原因就是**屏蔽底层操作系统平台的不同，并且降低基于原生语言开发的复杂性**。这里需要抓住一个重点

>JVM是一个内存中的虚拟机，JVM的存储就是内存。我们所写的类、常量、变量、方法都在内存中

这决定着我们写的代码是否健壮，是否高效

[![WX20190618-000650-2x.png](https://i.postimg.cc/R0Jj8kvn/WX20190618-000650-2x.png)](https://postimg.cc/xkYxNFJ0)



<h4>6.4什么是反射</h4>

>JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。 



**Robot.java**

```java
pcakage com.interview.javabasic.reflect;

public class Robot{
  private String name;
  public void sayHi(String helloSentence){
    System.out.println(helloSentence + "" + name);
  }
 private String throwHello(String tag){
   return "hello " + tag; 
 }
}
```

**ReflectSample.java**

```java
pcakage com.interview.javabasic.reflect;

public class ReflectSample{
  Class rc = Class.forName("com.interview.javabasic.reflect.Robot");
  Robot r = (Robot)arc.newInstance();
  System.out.println("Class name is" + rc.getName());
  
  //获取所有的方法
  Method getHello = rc.getDeclareMethod("throwHello", String.class);
  getHello.setAccessible(true);  //该方法是私有方法
  Object str = getHello.invoke(r,"Bob");
  System.out.println("getHello result is" + str);
  
  Method sayHi = rc.getMethod("sayHi", String.class);
  sayHi.invoke(r, "welcome");
  
  Field name = rc.getDeclareField("name");
  name.setAccessible(true);
  name.set(r, "Alice");
  sayHi.invoke(r, "welcome");
   
}
```



<h4>6.5谈谈ClassLoader</h4>

![WX20190619-231034-2x.png](https://i.postimg.cc/J0qw25G0/WX20190619-231034-2x.png)



[![WX20190619-231219-2x.png](https://i.postimg.cc/KjZcjzTK/WX20190619-231219-2x.png)](https://postimg.cc/FdBXnhDm)







 [![WX20190619-232415-2x.png](https://i.postimg.cc/zDS6FpmY/WX20190619-232415-2x.png)](https://postimg.cc/pyrs2fXs)





[![WX20190619-234626-2x.png](https://i.postimg.cc/wBWfrx5X/WX20190619-234626-2x.png)](https://postimg.cc/wtmQypGM)

**MyClassLoader.java**

```java
package com.interview.basic.reflect;

import java.io.*;

/**
 * Created with IDEA
 * author:RicardoXu
 * Date:2019/6/19
 * Time:23:42
 * 自定义ClassLoader
 */
public class MyClassLoader extends ClassLoader {

    private String path;
    private String classLoaderName;

    public MyClassLoader(String path, String classLoaderName) {
        this.path = path;
        this.classLoaderName = classLoaderName;
    }


    /**
     * 用于寻找类文件
     *
     * @param name
     * @return
     */
    @Override
    public Class findClass(String name) {
        byte[] b = loadClassData(name);
        return defineClass(name, b, 0, b.length);
    }

    /**
     * 用于加载类文件
     * @param name
     * @return
     */
    public byte[] loadClassData(String name){
        name = path + name + ".class";
        InputStream in = null;
        ByteArrayOutputStream out = null;
        try {
            in = new FileInputStream(new File(name));
            out = new ByteArrayOutputStream();
            int i = 0;
            while((i = in.read())!= -1){
                out.write(i);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            try {
                out.close();
                in.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return out.toByteArray();
    }
}

```

**MyClassLoaderCheck.java**

```java
package com.interview.basic.reflect;

/**
 * Created with IDEA
 * author:RicardoXu
 * Date:2019/6/20
 * Time:00:06
 */
public class MyClassLoaderCheck {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException {
        //要扫描的class路径
        String path = "xx/xxx/xxx";
        MyClassLoader myClassLoader = new MyClassLoader(path,"myClassLoader");
        //test.class
        Class c = myClassLoader.loadClass("test");
        System.out.println(c.getClassLoader());
        c.newInstance();

    }
}
```

**运行结果**

[![WX20190620-001931-2x.png](https://i.postimg.cc/9FwZymN4/WX20190620-001931-2x.png)](https://postimg.cc/ZCZWSSmZ)



<h4>6.6 ClassLoader的双亲委派机制</h4>

[![WX20190620-002428-2x.png](https://i.postimg.cc/PrkTMht3/WX20190620-002428-2x.png)](https://postimg.cc/fk5GwGSm)



**ClassLoader.java**

```java
protected Class<?> loadClass(String name, boolean resolve)
        throws ClassNotFoundException
    {
        synchronized (getClassLoadingLock(name)) {
            // First, check if the class has already been loaded
            Class<?> c = findLoadedClass(name);
            if (c == null) {
                long t0 = System.nanoTime();
                try {
                    if (parent != null) {
                        c = parent.loadClass(name, false);
                    } else {
                        c = findBootstrapClassOrNull(name);
                    }
                } catch (ClassNotFoundException e) {
                    // ClassNotFoundException thrown if class not found
                    // from the non-null parent class loader
                }

                if (c == null) {
                    // If still not found, then invoke findClass in order
                    // to find the class.
                    long t1 = System.nanoTime();
                    c = findClass(name);

                    // this is the defining class loader; record the stats
                    sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                    sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                    sun.misc.PerfCounter.getFindClasses().increment();
                }
            }
            if (resolve) {
                resolveClass(c);
            }
            return c;
        }
    }

```

[![WX20190620-003548-2x.png](https://i.postimg.cc/nrx7xgJH/WX20190620-003548-2x.png)](https://postimg.cc/R6PqQPh8)

<h4>6.7类的加载方式</h4>

* 隐式加载： new
* 显式加载：loadClass , forName等



[![WX20190626-213715-2x.png](https://i.postimg.cc/m22dNx0h/WX20190626-213715-2x.png)](https://postimg.cc/grQqm7cP)

[![WX20190626-214427-2x.png](https://i.postimg.cc/9M1J7JXv/WX20190626-214427-2x.png)](https://postimg.cc/hzzLF8dr)

**疑问：既然`forName`已经可以完成类的初始化了，为什么还需要`loadClass`呢？**

存在即合理。`SpringIOC`的`LazyLoading `（延迟加载技术） ,`SpringIOC`为了加快初始化速度，因此大量使用了延时加载技术。而使用`ClassLoader.loadClass`不需要执行类中的初始化代码，可以加快加载速度，把类的初始化工作留到实际使用这个类的时候。







