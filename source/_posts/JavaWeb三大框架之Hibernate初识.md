# JavaWeb三大框架之Hibernate初识

标签（空格分隔）： Java ssh框架

---

**ORM**
    `ORM (Object / Relationship Mapping)` : 对象/关系映射
    <p>它的好处是使让习惯于面向对象编程的开发人员少写于底层数据库有关的`sql`语句，方便了程序的维护与修改，及跨平台性。</p>


----------


**Hibernate**
    `Hibernate`就是`Java`中一种成熟的基于`ORM`的框架


----------

**Hibernate开发的基本步骤**

 1. 编写配置文档 `hibernate.cfg.xml`
 2. 编写实体类 (需要遵循`JavaBean`的设计规范)
 3. 生成对应实体类的映射文件（如 `Student`类的映射文件`Student.hbm.xml`)并添加到配置文档`hibernate.cfg.xml`中 
 4. 调用`Hibernate API`进行测试
 


----------


**Session**

`Hibernate`对数据库的操作都需要使用到`Session`对象，就类似于`JDBC`开发中的`Connection`对象。本质上讲`Hibernate`操作数据库，就是通过调用`Session`对象的各种函数实现的。

单表操作的常用方法：`save . delete .  update .  get / load`对应相应的`增删查改`

其中查询方法`get`与`load`的主要区别在于：`get`在使用的时候立即发送`sql`语句，且获得的就是实体类的对象类型。而`load`是在使用到对象的非主键属性时才会发送sql语句，且它返回的是一个代理对象.

    
                    Writren on 6.April.2016 Rainday
    




