---
title: 使用项目管理工具 `Maven`
date: 2016-10-20 10:33:00
tags: JavaWeb,Tools
---

---
>  Maven是什么

`Maven`是一个项目管理和整合工具.`Maven`为开发者提供了一套完整的构建生命周期框架。开发团队几乎不用
花多少时间就能够自动完成工程的基础构建配置,因为 `Maven` 使用了一个标准的目录结构和一个默认的构建生
命周期。

![](http://7xta11.com1.z0.glb.clouddn.com/walle.jpg)

在有多个开发团队环境的情况下,`Maven` 能够在很短的时间内使得每项工作都按照标准进行。因为大部分的工程
配置操作都非常简单并且可复用,在创建报告、检查、构建和测试自动配置时,`Maven` 可以让开发者的工作变得
更简单。

<!--more-->

`Maven` 能够帮助开发者完成以下工作:

* 构建
* 文档生成
* 报告
* 依赖
* SCMs
* 发布
* 分发
* 邮件列表

总的来说,`Maven` 简化了工程的构建过程,并对其标准化。它无缝衔接了编译、发布、文档生成、团队合作和其
他任务。`Maven` 提高了重用性,负责了大部分构建相关的任务。

> 我们为什么要用`Maven`

选择使用`Maven`与否与项目大小无关。有两点很重要：<b>标准项目结构、依赖处理</b>


  <strong>1.统一的标准结构</strong>

  试想一下，我用IDEA创建的一个项目，拷贝到别人的机器，能导入别人的eclipse吗？
或者我netbeans，别人IDEA？不同IDE会有差异，拷贝过去还得整理环境。
这还不是最要命的，如果项目和我机器环境有关，那到别人机器上，各种莫名其妙的问题怎么搞？
小项目轻松鼓捣可能就搞定了，如果项目很大很复杂呢？哪个环节出了问题——每次新环境都折腾吗？
如果你用maven这个统一的标准结构，那么一切迎刃而解，所有IDE都支持它。
执行`mvn eclipse:eclipse`，就会根据pom文件生成eclipse的项目文件，然后导入eclipse即可。
idea也一样，mvn idea:idea，虽然没用过netbeans，但肯定也是一样的方式。

<strong>2.依赖处理</strong>


若项目依赖spring-jdbc，直接在pom中写上spring-jdbc的坐标，一个命令，自动下载依赖的包。
不仅如此，依赖的依赖也会添加进来，比如spring-jdbc依赖core/context等，没这个，它根本没法玩。
哪怕是写个留言本的系统，用pom都是非常有利的，稍有规模的项目就更不用说了。
尤其是公司内部，各系统、各模块之间的依赖如果专门进行管理，可能会错综复杂到想吐。
既然依赖都在pom中指明了坐标，那么项目内各大大小小的jar包就不用了。互相分享交流也更方便了。


> Maven安装及配置

<h3>系统要求</h3>

| 项目           | 要求           |
| ------------- |:-------------:|
| JDK           |Maven 3.3 要求 JDK 1.7 或以上 |
| 磁盘           |Maven 自身安装需要大约 10 MB 空间。除此之外，额外的磁盘空间将用于你的本地 Maven 仓库。你本地仓库的大小取决于使用情况，但预期至少 500 MB|
| 操作系统	     |没有最低要求       |



首先介绍下博主的电脑环境

* 系统： `Ubuntu16.04 LTS`
* JDK: `1.8.0_73`

<h3>在执行下列操作时，请确保你已经配置好了Java环境</h3>

<h4>步骤1 ： 下载`Maven`文件</h4>

  从以下网址下载 Maven 3.2.5：http://maven.apache.org/download.html

<h4>步骤2 ：解压`Maven`文件</h4>

解压文件到你想要的位置来安装 Maven 3.2.5，你会得到 apache-maven-3.2.5 子目录。

博主的路径是 : `/usr/share/maven`

<h4>步骤3 ： 设置`Maven`的环境变量</h4>

    export M2_HOME=/usr/share/maven
    export M2=$M2_HOME/bin
    export MAVEN_OPTS=-Xms256m -Xmx512m

<h4>步骤4 ： 添加 Maven bin 目录到系统路径中</h4>

    export PATH=$M2:$PATH


<h4>步骤5 ： 验证 Maven 安装</h4>

    ~$ mvn -version
    Apache Maven 3.3.9
    Maven home: /usr/share/maven
    Java version: 1.8.0_73, vendor: Oracle Corporation
    Java home: /usr/local/lib/jdk1.8.0_73/jre
    Default locale: zh_CN, platform encoding: UTF-8
    OS name: "linux", version: "4.4.0-42-generic", arch: "amd64", family: "unix"


<h4>恭喜！你完成了所有的设置，开始使用 Apache Maven 吧。</h4>


<h3>使用`Maven`导入项目到`Myeclipse`</h3>

因为博主秉着<em>快速上手，以用为主</em>的精神，所以在这里就不一一介绍Maven生命周期，外部依赖等具体内容了，请各位小伙伴自行`google`。

对于Java项目，在github上看到的大多数都是基于maven构建的，现在很多也开始转用Gradle，比如hibernate和spring。最近想研究一些开源项目，不过clone后导入Myeclipse，发现源码包是以普通文件显示的,这样对于开发学习很不方便。

GitHub上Maven项目一般为了结构清晰且不依赖具体的IDE而没有将本地配置文件放到版本库中，这样导入MyEclipse时由于没有.classpath，.project而不能被识别成正规的项目，解决方法如下：

<h4>首先 使用Maven生成Eclipse项目</h4>

    mvn eclipse:eclipse

使用上述命令，将生成后的项目导入MyEclipse即可，此方式非常方便。
由于本地仓库可能缺少一些你所导入的项目所需的依赖，所以需要更新依赖。
在Myeclipse中右键项目`Maven4Myeclipse` > `Update Project`

<h3>Myeclipse创建Maven web 工程</h3>

标准的建立`Maven`支持的`JavaWeb`项目是这样的

* <b>首先新建一个web项目</b>
* <b>然后勾选`Add maven support`</b>

![](http://7xta11.com1.z0.glb.clouddn.com/2016-10-21%2009-07-01%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

<b>之后一步步按默认就可以了</b>

<h3>常见的问题</h3>

 * 提示读取某个.jar包出错或者无缝大找到.

 <b>解决方案 : 根据报错信息定位到本地仓库中的那个jar包，删除然后再次`Install`一下.</b>
