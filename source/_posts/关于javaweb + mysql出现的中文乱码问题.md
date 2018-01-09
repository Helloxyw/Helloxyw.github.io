# 关于javaweb + mysql出现的中文乱码问题

标签（空格分隔）： java mysql



----------
## 遇到的问题 ##


我用的是`Ubuntu 15.10`下的`Myeclipse 2013`,数据库用的是`mysql`。

Myeclipse内的字符编码全部设置的是`UTF-8`格式，在进行数据库插入操作后，发现数据库内的表的中文内容全部是乱码`？？`的形式。


----------


## 解决方案 ##

 - 首先我查看了数据库内编码格式
` mysql> show variables like 'character%';`        //查询当前mysql数据库的所有属性的字符编码
 +--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | latin1                     |
| character_set_connection | latin1                     |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | latin1                     |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
 - 将其中几项编码格式修改为`UTF8`
 `default-character-set=utf8；`
`character_set_client=utf8；`
`character_set_server = utf8； `
之后重启mysql
 - 之后在`jdbc`连接数据库时，指定字符集
`private static String url="jdbc:mysql://localhost:3306/hibernate?useUnicode=true&characterEncoding=UTF-8";`
之后数据库就可以正常显示中文了

## Hibernate坑 ##
由于使用`Hibernate`框架,如果在项目内的`hibernate.cfg.xml`配置文件中这样指定字符集的话


`<property name="connection.url">
			<jdbc:mysql://localhost:3306/hibernate?useUnicode=true&characterEncoding=utf8>
	</property>`
是出现编译错误，错误提示为将&连接符改为;。

正确的格式是
`<property name="connection.url">
			<![CDATA[jdbc:mysql://localhost:3306/hibernate?useUnicode=true&characterEncoding=utf8]]>
	</property>`

	                   Written in 4.4.2016  
