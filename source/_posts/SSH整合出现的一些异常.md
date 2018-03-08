---
title: SSH整合过程中的问题
date: 2017-1-13
tag: SSH
---


------
## 首先介绍下博主的工作环境
<strong>
* 系统：`Ubuntu 16.04 LTS`
* IDE: `Myeclispe 2015`
* Server: `Myeclipse2015`自带的`Myeclipse Tomcat v7.0`
* 框架版本: `Struts 2.1`  `Spring 4.1` `Hibernate 4.1.4`

![](https://desk-fd.zol-img.com.cn/t_s1366x768c5/g2/M00/01/0E/Cg-4WlU4niOIChfEAAtm0dNCMiIAACHugPmzzsAC2bp782.jpg)

<!--more-->

## 先介绍下 `SSH` 经典架构（大神请跳过)

首先，SSH不是一个框架，而是多个框架（struts+spring+hibernate）的集成，是目前较流行的一种Web应用程序开源集成框架，用于构建灵活、易于扩展的多层Web应用程序。

集成SSH框架的系统从职责上分为四层：表示层、业务逻辑层、数据持久层和域模块层（实体层）。

Struts作为系统的整体基础架构，负责MVC的分离，在Struts框架的模型部分，控制业务跳转，利用Hibernate框架对持久层提供支持。Spring一方面作为一个轻量级的IoC容器，负责查找、定位、创建和管理对象及对象之间的依赖关系，另一方面能使Struts和Hibernate更好地工作。

## 以下是最近做项目时遇到的一些问题

 ### 1.Struts2自带的`antlr-2.7.2.jar`与Hibernate4.1.4自带的`antlr-2.7.7.jar`发生冲突

  ** 报错内容如下: **

      java.lang.NoSuchMethodError: antlr.collections.AST.getLine()I

### 这是一个ssh整合的经典bug ,解决方案是:
  ** 在Myeclipse2015中打开`window -> perference -> project libraries `在里面找到`struts2.1 Libraries-> core->
  antlr-2.7.2.jar` 点击取消勾选这个jar包， 然后点击Apply ，最后重新部署下工程就ok了**


 ### 2.Spring事务管理出错
   ** 报错内容如下: **

      Write operations are not allowed in read-only mode (FlushMode.MANUAL): Turn your Session into FlushMode.COMMIT/AUTO or remove 'readOnly' marker
      from transaction definition.
** 大意是：只读模式下(FlushMode.NEVER/MANUAL)写操作不被允许：把你的Session改成FlushMode.COMMIT/AUTO或者清除事务定义中的readOnly标记。**

  ### 解决方案是:配置Spring的事务管理
  在applicationContext.xml中加入


    <tx:advice id="txadvice" transaction-manager="transactionManager">
    <tx:attributes>
      <tx:method name="save*" propagation="REQUIRED" rollback-for="Exception" />
      <tx:method name="modify*" propagation="REQUIRED"
        rollback-for="Exception" />
      <tx:method name="del*" propagation="REQUIRED" rollback-for="Exception" />
      <tx:method name="*" propagation="REQUIRED" read-only="false" />
    </tx:attributes>
    </tx:advice>
    <aop:config>
    <aop:pointcut id="daoMethod" expression="execution(* com.muke.employee.daoImpl.*.*(..))" />
    <aop:advisor pointcut-ref="daoMethod" advice-ref="txadvice" />
    </aop:config>

* `expression="execution( com.dao..(..))" `其中第一个*代表返回值，第二代表daoImpl下子包，第三个代表方法名，“（..）”代表方法参数。*
