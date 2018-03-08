---
title: Mysql 笔记
date: 2016-12-20 19:59:53
tags: Mysql
---


<h4>外键(foreign key)</h4>

定义:如果一个实体的某个字段指向另一个实体的主键,这个字段就是这个实体的外键.

&nbsp;&nbsp;被指向的实体，称之为主实体(主表),也叫父实体(父表).
&nbsp;&nbsp;负责指向的实体，称之为从实体(从表),也叫子实体(子表).


<center>
![](https://desk-fd.zol-img.com.cn/t_s1366x768c5/g5/M00/02/08/ChMkJ1bKzWGIGD1xAAlbi8UmgHEAALJAQMblhoACVuj729.jpg)
</center>

 <!--more-->
 
<h4>一 : 外键约束</h4>

 MySQL通过外键约束来保证表与表之间的数据的`完整性`和`准确性`。
 
 <h4>外键使用条件</h4>
 
  * 两个表必须是InnoDB表，MyISAM表暂时不支持外键（据说以后的版本有可能支持，但至少目前不支持）
 
 * 外键列必须建立了索引，MySQL 4.1.2以后的版本在建立外键时会自动创建索引，但如果在较早的版本则需要显示建立
 
 * 外键关系的两个表的列必须是数据类型相似，也就是可以相互转换类型的列，比如int和tinyint可以，而int和char则不可以
 
 外键的好处 : 可以使得两张表关联，保证数据的一致性和实现一些级联操作.
 
  <h4>创建外键语法:</h4>
  	
  	[CONSTRAINT [symbol]] FOREIGN KEY
	[index_name] (index_col_name, ...)
	REFERENCES tbl_name (index_col_name,...)
	[ON DELETE reference_option]
	[ON UPDATE reference_option]
	
例如:

	<!--Blog表  子表-->
	   CREATE TABLE `t_blog` (
	  `id` INT(11) NOT NULL AUTO_INCREMENT COMMENT '博客类型',
	  `title` VARCHAR(200) NOT NULL COMMENT '博客题目',
	  `summary` VARCHAR(400) DEFAULT NULL COMMENT '博客摘要',
	  `releaseDate` DATETIME DEFAULT NULL COMMENT '发布日期',
	  `clickHit` INT(11) DEFAULT NULL COMMENT '评论次数',
	  `replyHit` INT(11) DEFAULT NULL COMMENT '回复次数',
	  `content` TEXT COMMENT '博客内容',
	  `keyWord` VARCHAR(200) DEFAULT NULL COMMENT '关键字',
	  `type_id` INT(11) DEFAULT NULL COMMENT '外键关联博客类别',
	  PRIMARY KEY (`id`),
	  KEY `type_id` (`type_id`),
	  CONSTRAINT `t_blog_ibfk_1` FOREIGN KEY (`type_id`) REFERENCES `t_blogtype` (`id`)   //创建外键
	) ENGINE=INNODB AUTO_INCREMENT=35 DEFAULT CHARSET=utf8;
	
	<!--BlogType表  父表-->
	  CREATE TABLE `t_blogtype` (
	  `id` INT(11) NOT NULL AUTO_INCREMENT COMMENT '博客id',
	  `typeName` VARCHAR(30) DEFAULT NULL COMMENT '博客类别',
	  `orderNum` INT(11) DEFAULT NULL COMMENT '博客排序',
	  PRIMARY KEY (`id`)
	) ENGINE=INNODB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8;


<b>注意:</b>

&nbsp;&nbsp;如果子表试图创建一个在父表中不存在的外键值，InnoDB会拒绝任何`INSERT`或`UPDATE`操作。如果父表试图`UPDATE`或者`DELETE`任何子表中存在或匹配的外键值，最终动作取决于外键约束定义中的`ON UPDATE`和`ON DELETE`选项。InnoDB支持5种不同的动作，如果没有指定`ON DELETE`或者`ON UPDATE`，默认的动作为`RESTRICT`:

1. `CASCADE`:从父表中删除或更新对应的行，同时自动的删除或更新子表中匹配的行。`ON DELETE CANSCADE`和`ON UPDATE CANSCADE`都被InnoDB所支持。

  2. `SET NULL`: 从父表中删除或更新对应的行，同时将子表中的外键列设为空。注意，这些在外键列没有被设为NOT NULL时才有效。`ON DELETE SET NULL`和`ON UPDATE SET SET NULL`都被InnoDB所支持。

  3. ` NO ACTION`: InnoDB拒绝删除或者更新父表。

  4. `RESTRICT`: 拒绝删除或者更新父表。指定RESTRICT（或者NO ACTION）和忽略`ON DELETE`或者`ON UPDATE`选项的效果是一样的。

  5. `SET DEFAULT`: InnoDB目前不支持。
  
  <b>外键约束使用最多的两种情况：</b>
  
  1. 父表更新时子表也更新，父表删除时如果子表有匹配的项，删除失败.

2. 父表更新时子表也更新，父表删除时子表匹配的项也删除。

 前一种情况，在外键定义中，我们使用`ON UPDATE CASCADE ON DELETE RESTRICT`；
  
  后一种情况，可以使用`ON UPDATE CASCADE ON DELETE CASCADE`。
  
  


