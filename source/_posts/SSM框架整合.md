<h2> SSM框架整合</h2>
----
<B>这里的SSM指的是（`Spring-Springmvc-Mybatis`)三大框架</B>

<h4>前言</h4>
&nbsp;&nbsp;&nbsp;&nbsp;我们看招聘信息的时候，经常会看到这一点，需要具备`SSH`(`Struts-Spring-Hibernate`)框架的技能；而且在大部分教学课堂中，也会把`SSH`作为最核心的教学内容。 
&nbsp;&nbsp;&nbsp;&nbsp;但是，我们在实际应用中发现，`SpringMVC`可以完全替代`Struts`，配合注解的方式，编程非常快捷，而且通过restful风格定义url，让地址看起来非常优雅。 
&nbsp;&nbsp;&nbsp;&nbsp;另外，`MyBatis`也可以替换`hibernate`，正因为`MyBatis`的半自动特点，我们程序猿可以完全掌控SQL，这会让有数据库经验的程序猿能开发出高效率的SQL语句，而且XML配置管理起来也非常方便。 
&nbsp;&nbsp;&nbsp;&nbsp;下面我们开始进行SSM框架的整合。

<h4>介绍</h4>
在整合之前先介绍一下这三个框架，之前有了解的可以跳过.这里尽量通俗易懂的简单讲解一下。
 
 * Springmvc
 	&nbsp;&nbsp;&nbsp;&nbsp;它用于web层，相当于controller（等价于传统的servlet和struts的action），用来处理用户请求。举个例子，用户在地址栏输入http://网站域名/login，那么springmvc就会拦截到这个请求，并且调用controller层中相应的方法，（中间可能包含验证用户名和密码的业务逻辑，以及查询数据库操作，但这些都不是springmvc的职责），最终把结果返回给用户，并且返回相应的页面（当然也可以只反馈josn/xml等格式数据）。springmvc就是做前面和后面过程的活，与用户打交道！！
 	
 * Spring
&nbsp;&nbsp;&nbsp;&nbsp; 太强大了，以至于我无法用一个词或一句话来概括它。但与我们平时开发接触最多的估计就是IOC容器，它可以装载bean（也就是我们Java中的类，当然也包括service dao里面的），有了这个机制，我们就不用在每次使用这个类的时候为它初始化，很少看到关键字new。另外spring的aop，事务管理等等都是我们经常用到的。

* Mybatis
&nbsp;&nbsp;&nbsp;&nbsp;如果你问我它跟鼎鼎大名的Hibernate有什么区别？我只想说，他更符合我的需求。第一，它能自由控制sql，这会让有数据库经验的人编写的代码能搞提升数据库访问的效率。第二，它可以使用xml的方式来组织管理我们的sql，因为一般程序出错很多情况下是sql出错，别人接手代码后能快速找到出错地方，甚至可以优化原来写的sql。

<h4>SSM框架整合配置</h4>
&nbsp;&nbsp;&nbsp;&nbsp;首先在Myeclipse中建立一个`Maven Web`工程。(对这步有疑问的可以看我之前的博客[使用项目管理工具 `Maven`](http://blog.pockerface.cn/2016/10/20/使用项目管理利器Maven/))
这是相应的项目目录
<center>
![](http://7xta11.com1.z0.glb.clouddn.com/SSM.png)
</center>
<strong>这里介绍下Maven目录规范下各目录的作用</strong>
~~====~~

| 文件名           | 作用          |
| ------------- |:-------------:|
|src       	|根目录，没什么好说的，下面有main和test |
|main	|主要目录，可以放java代码和一些资源文件|
|java	|存放我们的java代码 |
|resources	| 存放资源文件，譬如各种的spring，mybatis，log配置文件|
mapper	|存放dao中每个方法对应的sql，在这里配置，无需写daoImpl|
|spring		|这里当然是存放spring相关的配置文件，有dao service web三层|
|test	|这里是测试分支|
|java	|测试java代码，应遵循包名相同的原则，这个文件夹同样要使用Build Path -> Use as Source Folder，这样看包结构会方便很多|
|resources	|没什么好说的，好像也很少用到，但这个是maven的规范|	
webapp	|用来存放我们前端的静态资源，如jsp js css|
|resources	|这里的资源是指项目的静态资源，如js css images等|
|WEB-INF	|很重要的一个目录，外部浏览器无法访问，只有羡慕内部才能访问，可以把jsp放在这里，另外就是web.xml了。你可能有疑问了，为什么上面java中的resources里面的配置文件不妨在这里，那么是不是会被外部窃取到？你想太多了，部署时候基本上只有webapp里的会直接输出到根目录，其他都会放入WEB-INF里面，项目内部依然可以使用classpath:XXX来访问，好像IDE里可以设置部署输出目录|

<strong>讲解几个必要的包，顺便讲解一下每个包的作用.</strong>
<center>
![](http://7xta11.com1.z0.glb.clouddn.com/SSM2.png)
</center>

|包名	|名称	|作用	|
|---------|:------:|--------:|
|dao	|数据访问层（接口）|与数据打交道，可以是数据库操作，也可以是文件读写操作，甚至是redis缓存操作，总之与数据操作有关的都放在这里，也有人叫做dal或者数据持久层都差不多意思。为什么没有daoImpl，因为我们用的是mybatis，所以可以直接在配置文件中实现接口的每个方法。|
|entity|实体类|一般与数据库的表相对应，封装dao层取出来的数据为一个对象，也就是我们常说的pojo，一般只在dao层与service层之间传输|
|dto|数据传输层|刚学框架的人可能不明白这个有什么用，其实就是用于service层与web层之间传输，为什么不直接用entity（pojo）？其实在实际开发中发现，很多时候一个entity并不能满足我们的业务需求，可能呈现给用户的信息十分之多，这时候就有了dto，也相当于vo，记住一定不要把这个混杂在entity里面|
|service| 	业务逻辑（接口）|写我们的业务逻辑，也有人叫bll，在设计业务接口时候应该站在“使用者”的角度|
|impl |业务逻辑（实现）|实现我们业务接口，一般事务控制是写在这里|
|web|控制器 |springmvc就是在这里发挥作用的，一般人叫做controller控制器，相当于struts中的action|

<strong>添加依赖</strong>
	SSM整合需要导入相应的jar包,我们使用maven来管理我们的jar，所以只需要在`pom.xml`中加入相应的依赖。如果没有使用maven的话,可以自己去官网下载相应的jar包，然后放到项目的WEB-INF/lib目录下.
	
	
<b>三个主要框架版本</b>

 * `Spring---4.3.3.RELEASE`
*	`spring mvc ---- 4.3.3.RELEASE`
*	`mybatis ---- 3.2.5`

<b>其他</b>

* `junit ---- 4.8.1`

* `MySQL ---- 5.1.38`

* `log4j ---- 1.2.17`

* `c3p0 ---- 0.9.2.1`

* `mybatis-spring ---- 1.3.0`  

* `jstl ---- 1.2`

<b>pom.xml文件如下</b>
	 
	  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
      <modelVersion>4.0.0</modelVersion>  
      <groupId>com.fendo.ssm</groupId>  
      <artifactId>fendo-SSM</artifactId>  
      <packaging>war</packaging>  
      <version>0.0.1-SNAPSHOT</version>  
      <name>fendo-SSM Maven Webapp</name>  
      <url>http://maven.apache.org</url>  
        
        <!-- 初始化框架的版本号 -->  
        <properties>  
            <spring.version>4.3.3.RELEASE</spring.version>  
        </properties>  
        
        
      <dependencies>  
        <dependency>  
          <groupId>junit</groupId>  
          <artifactId>junit</artifactId>  
          <version>3.8.1</version>  
          <scope>test</scope>  
        </dependency>  
          
             <!-- 加入ServletAPI -->  
            <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->  
            <dependency>  
                <groupId>javax.servlet</groupId>  
                <artifactId>servlet-api</artifactId>  
                <version>2.3</version>  
                <scope>provided</scope>  
            </dependency>  
              
              
            <!-- MySQL依赖 start -->  
            <dependency>  
                <groupId>mysql</groupId>  
                <artifactId>mysql-connector-java</artifactId>  
                <version>5.1.38</version>  
            </dependency>  
      
            <!-- MySQL依赖 end -->  
              
              
              
            <!-- 加入MyBatis 依赖 start -->  
            <dependency>  
                <groupId>org.mybatis</groupId>  
                <artifactId>mybatis</artifactId>  
                <version>3.2.5</version>  
            </dependency>  
            <!-- 加入MyBatis 依赖 end -->  
      
            <!-- Log4j start -->  
            <dependency>  
                <groupId>log4j</groupId>  
                <artifactId>log4j</artifactId>  
                <version>1.2.17</version>  
            </dependency>  
            <!-- Log4j end -->  
      
               <!-- 引入Spring(包含SpringMVC) 依赖 start -->  
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-core</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
      
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-web</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-oxm</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-tx</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
      
      
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-jdbc</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
      
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-webmvc</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-aop</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
      
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-context-support</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
      
            <dependency>  
                <groupId>org.springframework</groupId>  
                <artifactId>spring-test</artifactId>  
                <version>${spring.version}</version>  
            </dependency>  
      
            <!-- 引入Spring 依赖 end -->  
      
            <!-- 引用c3p0 依赖 start-->  
            <dependency>  
                <groupId>com.mchange</groupId>  
                <artifactId>c3p0</artifactId>  
                <version>0.9.2.1</version>  
            </dependency>  
            <!-- 引用c3p0 依赖 end-->  
      
            <!-- 引用插件依赖：MyBatis整合Spring -->  
            <dependency>  
                <groupId>org.mybatis</groupId>  
                <artifactId>mybatis-spring</artifactId>  
                <version>1.3.0</version>  
            </dependency>  
          
            <!-- JSTL -->  
            <dependency>  
                <groupId>jstl</groupId>  
                <artifactId>jstl</artifactId>  
                <version>1.2</version>  
            </dependency>  
          
      </dependencies>  
      <build>  
        <finalName>fendo-SSM</finalName>  
          
        <plugins>  
          <!-- 加入Tomcat插件 -->  
            <plugin>  
              <groupId>org.apache.tomcat.maven</groupId>  
              <artifactId>tomcat7-maven-plugin</artifactId>  
              <version>2.2</version>  
                    <configuration>   
                        <url>http://localhost:8080/manager/text</url>  
                        <username>admin</username>    
                        <password>admin</password>  
                    </configuration>   
            </plugin>  
        </plugins>  
          
      </build>  
    </project>
    
    
   
   <h4>编码配置文件</h4>
      <b>第一步：</b>我们先在spring文件夹里新建`spring-dao.xml`文件，因为spring的配置太多，我们这里分三层，分别是dao, service , web.
      
   1. 读入数据库连接相关参数（可选）
   2. 配置数据连接池
   
   	1.  配置连接属性，可以不读配置项文件直接在这里写死
   	2.  配置c3p0，只配了几个常用的
  
   3. 配置SqlSessionFactory对象（mybatis）
   4. 扫描dao层接口，动态实现dao接口，也就是说不需要daoImpl，sql和参数都写在xml文件上
   
   <b>spring-dao.xml</b>
   
   	
	  <?xml version="1.0" encoding="UTF-8"?>
	  <beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
		xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
		xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
		 http://www.springframework.org/schema/context 
	     http://www.springframework.org/schema/context/spring-context-4.0.xsd
	     http://www.springframework.org/schema/tx  
	     http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	     ">
	     <context:annotation-config />

		<!-- 配置整合MyBatis过程 -->
		<!-- 1.配置数据库相关参数properties的属性:${url} -->
		<context:property-placeholder location="classpath:jdbc.properties" />
	
		<!--2.数据库连接池 -->
		<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
			<!-- 配置连接池属性 -->
			<property name="driverClass" value="${jdbc.driverClass}"></property>
			<property name="jdbcUrl" value="${jdbc.url}"></property>
			<property name="user" value="${jdbc.username}"></property>
			<property name="password" value="${jdbc.password}"></property>

			<!-- c3p0私有属性 -->
			<property name="maxPoolSize" value="30"></property>
			<property name="minPoolSize" value="10"></property>
			<!-- 关闭连接后不自动commit -->
			<property name="autoCommitOnClose" value="false"></property>
			<!-- 获取连接超时时间 -->
			<property name="checkoutTimeout" value="1000"></property>
			<!-- 当获取连接失败重试次数 -->
			<property name="acquireRetryAttempts" value="2"></property>
		</bean>
	    
	    <!-- 3.配置sqlSessionFactory对象 -->
		<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
			<!-- 注入数据库连接池 -->
			<property name="dataSource" ref="dataSource"></property>
			<!-- 配置MyBatis全局配置文件:myBatis-config.xml -->
			<property name="configLocation" value="classpath:mybatis-config.xml"></property>
			<!-- 扫描entity包，使用别名 -->
			<property name="typeAliasesPackage" value="org.seckill.entity"></property>
			<!-- 扫描sql配置文件:mapper需要的xml文件 -->
			<property name="mapperLocations" value="classpath:mapper/*.xml"></property>
		</bean>

		<!-- 4.配置扫描Dao接口包，动态实现Dao接口，注入到spring容器中 -->
		<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
			<!-- 注入sqlSessionFactory -->
			<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
			<!-- 给出需要扫描的Dao接口包 -->
			<property name="basePackage" value="com.blog.dao"></property>
		</bean>
	</beans>
	
&nbsp;&nbsp;因为数据库配置相关参数是读取配置文件，所以在resources文件夹里新建一个jdbc.properties文件，存放我们4个最常见的数据库连接属性，这是我本地的，大家记得修改.如果大家上传github时记得删掉密码，不然别人就很容易得到你服务器的数据库配置信息.	

<b>jdbc.properties</b>
	
	jdbc.driverClass=com.mysql.jdbc.Driver
	jdbc.url=jdbc\:mysql\://127.0.0.1\:3306/personBlog
	jdbc.username=root
	jdbc.password=
	

<b>友情提示：</b>配置文件中的jdbc.username，如果写成username，可能会与系统环境中的username变量冲突，所以到时候真正连接数据库的时候，用户名就被替换成系统中的用户名（有得可能是administrator），那肯定是连接不成功的，这里有个小坑,需要大家注意.

这里用到了mybatis,所以接下来需要配置mybatis核心文件,在`resources`文件夹下新建`mybatis-config.xml`文件

 <li>  使用自增主键</li>
<li> 使用列别名</li>
 <li>   开启驼峰命名转换 create_time -> createTime</li>

<b>mybatis-config.xml</b>

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE configuration
	  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
	  "http://mybatis.org/dtd/mybatis-3-config.dtd">

	<configuration>
		<!-- 配置全局属性 -->
		<settings>
			<!-- 使用jdbc的getGenerateKeys获取数据库自增主键 -->
			<setting name="useGererateKeys" value="true" />
			<!-- 使用列别名替换列名 默认：true -->
			<setting name="useColumnLabel" value="true" />
			<!-- 开启驼峰命名转换 -->
			<setting name="mapUnderscoreToCamelCase" value="true" />
		</settings>

	</configuration>
	
<b>第二步:</b>刚弄好dao层，接下来到service层了。在spring文件夹里新建`spring-service.xml`文件。	


	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
		xmlns:p="http://www.springframework.org/schema/p"
		xmlns:context="http://www.springframework.org/schema/context" 			
		xmlns:tx="http://www.springframework.org/schema/tx"
		xsi:schemaLocation="http://www.springframework.org/schema/beans 		
		http://	www.springframework.org/schema/beans/spring-beans-4.1.xsd
		 http://www.springframework.org/schema/context 
	     http://www.springframework.org/schema/context/spring-context-4.0.xsd
	     http://www.springframework.org/schema/tx  
	     http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	     ">
		<!-- 扫描service包下所有使用注解的类型 -->
		<context:component-scan base-package="com.blog.service"></context:component-scan>

		<!-- 注入事务管理器 -->
		<bean id="transactionManager"
			class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
			<!-- 注入数据库连接池 -->
			<property name="dataSource" ref="dataSource"></property>
		</bean>
	
		<!--配置基于注解的声明式事务 -->
		<tx:annotation-driven transaction-manager="transactionManager"/>
	</beans>


<b>第三步:</b>配置web层，在spring文件夹里新建spring-web.xml文件.

 1.  开启SpringMVC注解模式，可以使用@RequestMapping，@PathVariable，@ResponseBody等
  2. 对静态资源处理，如js，css，jpg等
  3.   配置jsp 显示ViewResolver，例如在controller中某个方法返回一个string类型的 "login"，实际上会返回"/WEB-INF/login.jsp"
  4.   扫描web层 @Controller
  
  <b>spring-web.xml</b>
  
		  
		<?xml version="1.0" encoding="UTF-8"?>
		<beans xmlns="http://www.springframework.org/schema/beans"
			xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
			xmlns:p="http://www.springframework.org/schema/p" xmlns:context="http://www.springframework.org/schema/context"
			xmlns:tx="http://www.springframework.org/schema/tx"
			xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
			 http://www.springframework.org/schema/context 
		     http://www.springframework.org/schema/context/spring-context-4.0.xsd
		     http://www.springframework.org/schema/tx  
		     http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
		     http://www.springframework.org/schema/mvc
		 	http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd
		     ">
			<!-- 配置springMVC -->
			<!-- 1:开启springMVC注解模式 -->
			<!-- 简化配置： （1）自动注册DefaultAnnotationHandlerMapping,AnnotationMethodHandlerAdapter 
				(2)提供一系列：数据绑定，数字和日期的format @NumberFormat ,@DataTimeFormat ,xml ,json默认读写支持 -->

			<mvc:annotation-driven/>
	
			<!-- 2.静态资源默认servlet配置
				1：允许加入对静态资源的处理：js,gif,png
				2:允许使用"/"做整体映射
			 -->
			<mvc:default-servlet-handler/>
	
			<!--3：配置jsp 显示ViewResolver-->
			<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
				<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"></property>
				<property name="prefix" value="/WEB-INF/jsp/"></property>
				<property name="suffix" value=".jsp"></property>
			</bean>
	
			<!-- 4:扫描web相关的bean -->
			<context:component-scan base-package="org.seckill.web"/>
		</beans>

  
  <b>第四步:</b>最后就是修改web.xml文件了，它在webapp的`WEB-INF`下。
<b>web.xml</b>

	<?xml version="1.0" encoding="UTF-8"?>
	<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns="http://java.sun.com/xml/ns/javaee"
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
		id="WebApp_ID" version="3.0">

	<!-- maven命令创建的servlet版本较低，默认没有打开JSTL，所以更改servlet版本为3.0 -->
	  <!-- 配置DispatcherServlet -->
		<servlet>
			<servlet-name>blog-dispatcher</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<!-- 配置springMVC需要加载的配置文件
				spring-dao.xml ,spring-service.xml ,spring-web.xml
				Mybatis ->spring ->springMVC
			 -->
			 <init-param>
			 	<param-name>contextConfigLocation</param-name>
			 	<param-value>classpath:spring/spring-*.xml</param-value>
			 </init-param>
		</servlet>
		<servlet-mapping>
			<servlet-name>blog-dispatcher</servlet-name>
			<!-- 默认匹配所有请求 -->
			<url-pattern>/</url-pattern>
		</servlet-mapping>
	
	</web-app>

我们在项目中经常会使用到日志，所以这里还有配置日志xml，在`resources`文件夹里新建`logback.xml`文件，所给出的日志输出格式也是最基本的控制台呼出，大家有兴趣查看`logback官方文档`。


		<?xml version="1.0" encoding="UTF-8"?>
		<configuration debug="true">

		<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
			<!-- encoders are by default assigned the type ch.qos.logback.classic.encoder.PatternLayoutEncoder -->
			<encoder>
				<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
				</pattern>
			</encoder>
		</appender>

		<root level="debug">
			<appender-ref ref="STDOUT" />
		</root>
	</configuration>
	


