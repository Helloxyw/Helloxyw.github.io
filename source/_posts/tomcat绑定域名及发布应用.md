---
title: Tomcat绑定域名发布应用
date: 2016-10-30 19:59:53
tags: Tomcat
---

---

<b>我们使用`Tomcat`发布应用时，默认是绑定在`8080`端口，这种情况一般在测试后使用。如果应用需要部署到服务器上则需要修改端口为`80`，并通过域名直接访问。具体如何做呢？
</b>

![](http://7xta11.com1.z0.glb.clouddn.com/ChMkJ1cghZGIbfOAAARV4Pwpv4QAAQsDQJ9YssABFX4485.jpg)
#### 打包应用为war

<b>首先打包应用为.war文件，具体步骤为在`Myeclipse`中点击`File->Export`选择`web->war file`下一步之后选择要打包的应用，重命名为发布名称，比如`test.war`确定之后即打包。</b>

<!--more-->

<b>我们把War包传到主机上去，放到Tomcat的webapps下，启动tomcat的startup.bat，会自动解压项目；到了这里，还不够。我们只能通过 http://外网IP:8080/项目名称访问； (http://外网IP:8080/项目名称访问；)</b>

<strong>我们现在要干两个事情，第一个是去掉端口，第二个是去掉项目名称。</strong>

#### 配置容器


<h4>1.这里是在本地调试时的修改，真正部署可以跳过这段，看下个段落</h4>


修改tomcat安装目录下的conf下的server.xml

> 找到Connector节点，将其port改为80后保存，结果如下：


        <Connector port="80" protocol="HTTP/1.1"
                connectionTimeout="20000"
                redirectPort="8443" />


> 绑定域名

这里博主假设域名是`testphoto.com`<em>(注意域名没有大小写之分，就算是大写也会被浏览器解析成小写)</em> 要绑定的项目时WebPhoto

<b>1.首先因为是在本地访问该域名，所以需要修改Hosts文件，ubuntu系统下修改/`etc/hosts`加上一条记录</b>

    127.0.0.1      testphoto.com

<b>2.之后修改server.xml文件</b>

  找到 Engine节点,在里面添加一个 Host 节点，Engine其中有一个默认的`Hostname="localhost" `的Host 节点，增添的Host节点:

    <Host name="testphoto.com"   appBase="webapps"
          unpackWARs="true" autoDeploy="true">

    </Host>


此时在启动tomcat后，输入绑定的域名`testphoto.com`就可以看到tomcat了，但是要访问我们的项目，还是要在后面加上项目名称，
如`testphoto.com/WebPhoto`

<b>3.绑定项目到域名</b>

  在第二步的基础上，在<Host>节点中加入下面的配置


    <Context path="/" docBase="/opt/tomcat7/webapps/WebPhoto">
    </Context>

  `docBase`是你的应用的绝对路径。


#### 此时通过`testphoto.com`就可以访问你的页面了

![](http://7xta11.com1.z0.glb.clouddn.com/1.png)





<h4>2.真正部署应用到网上的配置</h4>

  和在本地调试配置步骤一样，只不过这里是在服务器上配置。

>修改端口，改成80端口

    <Connector port="80" protocol="HTTP/1.1"
            connectionTimeout="20000"
            redirectPort="8443" />


> 绑定域名


    <Host name="top.pockerface.cn"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

    <Context path="/" docBase="/opt/tomcat7/webapps/WebPhoto"></Context>

    </Host>


<b>这里的`top.pockerface.cn`是博主的二级域名，需要在DNS解析中，解析域名指向你的服务器。</b>

#### 此时通过`top.pockerface.cn`就可以访问你的页面了

![](http://7xta11.com1.z0.glb.clouddn.com/1.png)
