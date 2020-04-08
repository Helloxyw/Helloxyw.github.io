---
title: kafak
date: 2020-04-08 
tags: kafak
---

<h3>1. 消息队列的好处</h3>
* 缓存和削峰： 上游数据时有突发流量，下游可能扛不住，或者下游没有足够的机器来保证冗余，kafka在中间可以起到一个缓冲作用，把消息暂存在kafka中，下游服务就可以按照自己的节奏进行慢慢处理

![](https://images.vogel.de/vogelonline/bdb/1484300/1484344/41.jpg)
<!--more-->

* 解耦和扩展性：项目开始的时候，并不能确定具体需求。消息队列可以作为一个接口层，解耦重要的业务流程。只需要遵守约定，针对数据编程即可获取扩展能力。
* 冗余：可以采用一对多的方式，一个生产者发布消息，可以被多个订阅topic的服务消费到，供多个毫无关联的业务使用
* 健壮性：消息队列可以堆积请求，所以消费端业务即使短时间死掉，也不会影响主要业务的正常进行。
* 异步通信：很多时候，用户不想也不需要立即处理消息。消息队列提供了异步处理机制，允许用户把一个消息放入队列，但并不立即处理它。想向队列中放入多少消息就放多少，然后在需要的时候再去处理它。


<h3>2.消息队列通信的模式</h3>
**一、点对点模式（队列模式）**

![image-20191226210942873](https://pic3.zhimg.com/80/v2-8ca010d65aa8e4c385a901fb2e91f31a_1440w.jpg)

> 点对点模式通常是基于拉取或者轮询的消息传送模型，这个模型的特点是发送到队列的消息被一个且只有一个消费者进行处理。生产者将消息放入消息队列后，由消费者主动的去拉取消息进行消费。点对点模型的优点是消费者拉取消息的频率可以由自己控制。但是消息队列是否有消息需要消费，在消费者端无法感知，所以在消费者端需要额外的线程去监控。

**二、发布订阅模式**

![image-20191226211754863](https://pic4.zhimg.com/80/v2-92c5c49e68a6213914936ed979c05c6b_1440w.jpg)

>发布订阅模式是一个基于消息推送的消息传送模型，该模型可以有多种不同的订阅者。生产者将消息放入消息队列后，队列会将该消息推送给订阅过该类消息的消费者（类似微信公众号）。由于消费者被动接收推送，所以无需感知消息队列是否有待消费的消息！但consumer1、consumer2、consumer3由于机器性能不一样，所以处理消息的能力也会不一样，但消息队列却无法感知消费者消费的速度！所以推送的速度成了发布订阅者模式的一个问题！假设三个消费者的处理速度分别是8M/s、5M/s、2M/s，如果队列推送的速度为5M/s，则consumer3无法承受！如果队列推送的速度为2M/s，则consumer1、consumer2会出现资源的极大浪费！



**kafka采用的模式**

两种方式各有优缺点：

* 点对点模式中多个消费者共同消费同一个队列，效率高
* 发布订阅模式，一个消息可以被多次消费，能支持冗余的消费（例如两个消费者共同消费一个消息，防止某个消费者挂了）

显然要构建一个大数据下消息队列，两种模式都是必需的。因此kafka引入了ConsumerGroup（消费组）的概念，Consumer Group是以发布订阅模式工作的；一个Consumer Group中可以有多个Consumer，Group内的Consumer以点对点模式工作。

![image-20191228151527931](https://lotabout.me/2018/kafka-introduction/kafka-consumer-group.svg)

<h3>3.Kafka基础架构及术语</h3>
**kafka是一种高吞吐量的分布式发布订阅消息系统**，它可以处理消费者规模的网站中的所有动作流数据。具有：

* 高吞吐量
* 低延迟
* 容错
* 耐久性
* 可扩展性

![image-20191228135443219](https://pic1.zhimg.com/80/v2-4692429e9184ed4a93911fa3a1361d28_1440w.jpg)

**相关概念：**

* Producer： 生产者，消息的生产者，是消息的入口

**Kafka Cluster**

* Broker：kafka实例，每个服务器上有一个或多个kafka实例，我们姑且认为每个broker对应一台服务器。每个kafka集群内的broker都有一个**不重复**的编号，如图中的broker-0、broker-1等。。。
* Topic：消息的主题，可以理解为消息的分类，kafka数据就保存在topic。每个broker上可以创建多个topic
* Partition：Topic的分区，每个topic可以有多个分区，分区的作用就是负载，提高kafka的吞吐量。同一个topic在不同分区的数据不重复的，partition的表现形式就是一个个文件夹。
* Replication：每一个分区都有多个副本，副本的作用是做备胎。当主分区（Leader)故障的时候会选择一个备胎（Follower)上位，成为Leader。在kafka中默认副本最大数目是10个，且副本的数量不能大于Broker的数量，follower和leader绝对是在不同的机器，同一机器对同一分区也只可能存放一个副本（包括自己）
* Message：每一条发送的消息主体
* Consumer：消费者，即消息的消费方，是消息的出口
* ConsumerGroup：我们可以将多个消费者组成一个消费组，在kafka的设计中同一个分区的数据只能被消费组中的某一个消费者消费。同一个消费组的消费者可以消费同一个topic的不同分区的数据，这是为了提高kafka的吞吐量。
* Zookeeper：kafka集群依赖zookeeper来保存集群的元数据，来保证系统的可用性。

<h3>4.工作流程分析</h3>
**发送数据**

Producer在写入数据时**永远的找Leader**，不会直接将数据写入Follower。写入的流程如下图：

![image-20191228142209444](https://pic2.zhimg.com/80/v2-b7e72e9c5b9971e89ec174a2c2201ed9_1440w.jpg)

需要注意的一点：消息写入Leader后，follower是主动的去leader同步的！Producer采用push模式将数据发布到broker，每条消息追加到分区中，顺序写入磁盘，所以保证**同一分区**内的数据是有序的！

![image-20191228142638781](https://pic3.zhimg.com/80/v2-87d558aaa349bf920711b9c157e11f6a_1440w.jpg)

上面说到数据会写入不同的分区，kafka分区的主要目的是：

* 方便扩展：因为同一个topic可以有多个partition，所以我们可以通过扩展机器去轻松的应对日益增长的数据量
* 提高并发：以partition为读写单位，可以多个消费者同时消费数据，提高了消息的处理效率。

​        

​        熟悉负载均衡的同学应该知道，当我们向某个服务器发送请求的时候，服务端可能会对请求做一个负载，将流量分发到不同的服务器，那在kafka中，如果某个topic有多个partition，producer又怎么知道该将数据发往哪个partition呢？kafka有以下几个原则：

1. partition在写入是可以指定需要写入的partition，如果有指定，则写入对应的partition
2. 如果没有指定partition，但是设置了数据的key，则会根据key值hash出一个partition
3. 如果没有指定partition，又没有设置key，则会轮询出一个partition



​        保证数据不丢失是一个消息中间件的基本保证，那producer在向kafka写入消息的时候，怎么保证消息不丢失呢？其实上面的写入流程图中有描述出来，那就是通过ACK应答机制！在生产者向队列写入数据时，可以设置参数来确定是否确认kafka接收到数据，这个参数可设置的值为0、1、-1（all）。

* 0：代表producer往集群发送数据，不需要等到集群的返回，不确保消息发送成功。安全性最低但是效率最高
* 1：代表producer往集群发送数据只要leader应答就可以发送下一条，只确保leader发送成功
* -1：代表producer往集群发送数据需要所有的follower都完成从leader的同步才会发送下一条，确保leader发送成功和所有副本都完成备份，安全性最高，但是效率最低

最后需要注意的是，如果往不存在topic写数据，能不能写入成功呢？kafka会自动创建topic，分区和副本的数量根据默认值都是1。



**保存数据**

topic可以分为一个或多个partition，partition在服务器上的表现形式就是一个一个的文件夹，每个partition的文件夹下面会有多组segment文件，每组segment文件又包含.index文件、.log文件、.timeindex文件（早起版本中没有）三个文件，log文件实际上就是存储message的地方，而index和timeindex文件为索引文件，用于检索消息。

![image-20191228144748824](https://pic4.zhimg.com/80/v2-72e50c12fd9c6fbf58d3b5ca14c90623_1440w.jpg)

如上图，这个partition有三组segment文件，每个log文件大小是一样的，但是存储message数量不一定相等的（每条message大小不一致）。文件的命名就是以该segment最小offset来命名的，如000.index存储offset为0~368795的消息，kafka就是利用分段+索引的方式来解决查找效率的问题。



**Message结构**

存入log的message主要包含消息体、消息大小、offset、压缩类型。。。等等！重点了解下面三个：

* offset：offset是一个占用8byte的语有序id号，它可以唯一确定每条消息在partition内的位置！
* 消息大小：消息大小占用4byte,用于描述消息的大小
* 消息体：消息体存放的是实际的消息数据（被压缩过），占用的空间根据具体的消息而不会一样。

**存储策略**

无论消息是否被消费，kafka都会保存所有的消息。那么对于旧数据有什么删除策略?

* 基于时间，默认配置是168小时（7天）
* 基于大小，默认配置1073741824

需要注意的是，kafka读取特定消息的时间复杂度是O(1)，所以这里删除过期的文件并不会提高kafka性能！



**消费数据**

消息存储在log文件后，消费者就可以进行消费了。消费组内的消费者是以点对点模式进行消费，消费者主动的去kafka集群拉取消息，与producer相同的是，消费者在拉取消息的时候也是找leader去拉取。

多个消费者可以组成一个消费者组（Consumer Group），每个消费者组都有一个组id！同一个Consumer Group下的Consumer可以消费同一个topic下不同partition的数据，但是不会组内多个Consumer消费同一个partition的数据。

![image-20191228152107909](https://pic4.zhimg.com/80/v2-75a79cba9cfafe5c2f4d5349acb72207_1440w.jpg)

图示是Consumer Group内的Consumer小于partition数量的情况，所以会出现某个Consumer消费多个partition数据的情况，消费的速度也就不及只处理一个partition的Consumer的处理速度！如果Consumer Group的Consumer多于Partition的数量，那会不会出现多个Consumer消费同一个partition的数据呢？上面已经提到不会出现这种情况！多出来的Consumer不消费任何partition的数据。所以在实际的应用中，建议**Consumer Group的Consumer数量于Partition的数量一致！**



**查找message**

假如现在需要查找一个offset为368801的message是什么样的过程？

![image-20191228152622924](https://pic1.zhimg.com/80/v2-87051d884344edf9f8fd97a3dacb32d0_1440w.jpg)

1. 先找到offset为368801的message所在的segment文件（利用二分查找），在这里找到的就是第二个segment文件。
2. 打开找到的segment中的.index文件（也就是368796.index文件，该文件起始偏移量为368796+1，我们要查找offset为368801的message在该index内的偏移量为368796+5  = 368801，所以这里要查找的**相对offset**为5）。由于该文件采用的是稀疏索引的方式存储着相对offset及对应message物理偏移量的关系，所以直接找相对offset为5的索引找不到，这里同样利用二分法查找相对offset小于或者等于指定的相对offset的索引条目中最大的那个相对offset，所以找到的是相对offset为4的那个索引
3. 根据找到的相对offset为4的索引确定message存储的物理偏移量位置为256，打开数据文件，从位置为256那个地方开始顺序扫描直到找到offset为368801的那条message。

这套机制是建立在offset为有序的基础上，利用**segment + 有序offset + 稀疏索引 + 二分查找 + 顺序查找**等多种手段来高效的查找数据！至此，消费者就能拿到需要处理的数据进行处理了。那每个消费者又是怎么记录自己消费的位置呢？在早期的版本中，消费者将消费到的offset维护到zookeeper中，Consumer每隔一段时间上报一次，这里容易导致重复消费，且性能不好！在新的版本中消费者消费到的offset以及直接维护在kafka集群的_consumer_offsets这个topic中。



<h3>参考</h3>
* 稀疏索引

  ![GhnHHg.png](https://s1.ax1x.com/2020/04/08/GhnHHg.png)



<h3>面试题</h3>
**1.Kafka中的ISR、AR代表什么？ISR的伸缩性又指什么？**

提到副本，肯定就会想到正本。副本是正本的拷贝。在kafka中，正本和副本都称为副本(Replica)，但存在leader和follower之分。

* ISR：In-Sync Replicas 副本同步队列（所有与leader副本保持一定程度同步的副本，包括leader副本）
* AR: Assigned Replicas 所有副本
* OSR：Out-of-Sync Replicas  于leader副本同步滞后过多的副本（不包含leader副本）

ISR集合是AR集合的子集。消息会先发送给leader副本，然后follower副本才能从leader副本中拉取消息进行同步。同步期间，follower副本相对于leader副本而言会有一定程度的滞后。`一定程度的滞后`是指可忍受的滞后范围，这个范围可以通过参数配置。

ISR是由leader维护，follower从leader同步数据有一定延迟：

* 延迟时间：replica.lag.time.max.ms（0.9.0.0版本之后只支持延迟时间维度）
* 延迟条数：replica.lag.max.message（0.9.0.0版本之前只支持延迟条数维度）

任意一个超过阈值都会把follower剔除出ISR，存入OSR。由此可见AR = ISR + OSR。正常情况下，所有follower副本都应该与leader副本保持一定程度的同步，即AR = ISR，OR集合为空。

**ISR伸缩性**

**leader副本负责维护和跟踪**`ISR`集合中所有follower副本的滞后状态，当follower副本落后太多或失效时，leader副本会把它从ISR集合中剔除。如果`OSR`集合中有follower副本“追”上了leader副本，那么leader副本会把它从OSR集合转移至ISR集合。默认情况下，当leader副本发生故障时，只有在ISR集合中的follower副本才有资格被选举为新的leader，而在OSR中的副本则没有任何机会。



**2.broker作用**

broker是消息的代理，producer往broker里面的指定topic中写消息，Consumer从broker里面拉取指定topic的消息，然后进行业务处理，broker在中间起到一个代理保存消息的中转站



**3.kafka中zookeeper起到什么作用？可以不用zookeeper么**

zookeeper是一个分布式的协调组件，早起版本kafka用zk做meta信息存储，Consumer的消费状态、Group的管理以及offset的值。考虑到zk本身的一些因素以及整个架构较大概率存在单点问题，新版本中逐渐弱化了zookeeper的作用。新的Consumer使用了kafka内部的group coordination协议，也逐渐减少了对zookeeper的依赖。

但是broker依然依赖于zk，zookeeper在kafka中还用来选举controller和检查broker是否存活等。



**4.kafka follower如何与leader同步数据**

![](https://img-blog.csdnimg.cn/20190425101924979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoZWVwODUyMQ==,size_16,color_FFFFFF,t_70)

kafka的复制既不是完全的同步复制，也不是单纯的异步复制。

* 完全同步复制：要求all live follower都复制完，这条消息才算commit，这种复制方式极大影响了吞吐率
* 异步复制：follower异步的从leader复制数据，数据只要被leader写入log就被认为已经commit，这种情况下，如果leader挂掉，会丢失数据

kafka使用ISR方式很好的均衡了确保数据不丢失以及吞吐率。follower可以批量的从leader复制数据，而且leader充分利用磁盘顺序读以及send file(zero copy)机制，这样极大的提高复制性能，内部批量写磁盘，大幅减少了follower与leader的消息量差

**5.如果leader crash时，ISR为空怎么办？**

kafka在broker端提供了一个配置参数：unclean.leader.election，这个参数有两个值：

* true（默认）：允许不同步的副本成为leader，由于不同步副本消息较为滞后，此时成为leader，可能会出现消息不一致的情况
* false：不允许不同步的副本成为leader，此时如果发生ISR列表为空，会一直等待旧leader恢复，降低了可用性

**6.kafka中消息是否会丢失和重复消费？**

要确定kafka的消息是否丢失或重复消费，从两个方面分析入手：消息发送和消息消费

**消息发送**

kafak消息发送有两种方式：同步（sync)和异步（async），默认是同步方式，可通过producer.type 属性进行配置。kafka通过配置request.required.asks属性来确认消息的生产：

* 0：表示不进行消息接收是否成功的确认
* 1：表示leader接收成功时确认
* -1：表示leader和follower都接收成功时确认

综上所述，有6种消息生产情况，下面分情况来分析消息丢失的场景：

* Asks =0 :不和kafka进行消息接收确认，则当网络异常、缓冲区满了等情况时，消息可能丢失
* Asks =1 :同步模式下，只有leader确认接收成功后但挂掉了，副本没有同步，数据可能丢失

**消息消费**

kafka消息消费有两个Consumer接口，Low-level API和 High-level API：

* Low-level API：消费者自己维护offset等值，实现对kafka的完全控制
* High-level API：封装了对Partition和offset的管理，使用简单

如果使用高级接口High-level API，可能存在一个问题就是当消费者从集群中把消息取出来、并提交了新的消息offset值后，还没来得及消费就挂掉了，那么下次再消费时之前没消费成功的消息就”诡异“的消息了

**解决办法**

* 消息丢失：同步模式下，确认机制设置为-1，即让消息写入leader和follower之后再确认消息发送成功；异步模式下，为防止缓冲区满，可以在配置文件设置不限制阻塞超时时间，当缓冲区满时，让生产者一直处于阻塞状态
* 消息重复消费：将消息的唯一标识保存到外部介质中，每次消费时判断是否处理过即可



**7.为什么kafka不支持读写分离**

​		在kafak中，生产者写入消息、消费者读取消息的操作都是与leader副本进行交互的，从而实现的是一种主写主读的生产者消费模型。

​		kafak并不支持主写从读，因为主写从读有两个很明显的缺点：

* 数据一致性问题： 数据从主节点转到从节点必然会有一个延时的时间窗口，这个时间窗口会导致主从节点之间的数据不一致。某一时刻，在主节点和从节点中A数据的值都为X，之后将主节点中A的值修改为Y，那么在这个变更通知到从节点之前，应用读取从节点中的A数据的值并不是最新的Y,这就产生了数据不一致的问题
* 延时问题：类似Redis这种组件，数据从写入主节点到同步至从节点中的过程需要经历网络->主节点内存->网络->从节点内存这几个阶段，整个过程会耗费一定的时间。而在kafka中，主从同步会比redis更加耗时，他需要经历网络->主节点内存->主节点磁盘->网络->从节点内存->从节点磁盘这几个阶段。对延时敏感的应用而言，主写从读的功能并不太适用

**8.kafka中是怎么体现消息顺序性的？**

kafka每个Partition中的消息在写入时都是有序的，消费时，每个Partition只能被每一个Group中的一个消费者消费，保证了消费时也是有序。

整个topic不保证有序，如果为了保证整个topic有序，那么将Partition调整为1

