# Ubuntu16.04下安装配置jdk
### 1.通过ppa(源) 方式安装.
<b>这里推荐第1种,因为可以通过 `apt-get upgrade` 方式方便获得jdk的升级</b>

<h4>在我们继续了解如何安装Java之前，让我们快速地了解`JRE、OpenJDK`和`Oracle JDK`之间的不同之处。</h4>

<h4>`JRE（Java Runtime Environment）`，它是你运行一个基于Java语言应用程序的所正常需要的环境。如果你不是一个程序员的话，这些足够你的需要。</h4>

<h4>JDK代表Java开发工具包，如果你想做一些有关Java的开发（阅读程序），这正是你所需要的。</h4>

<h4>OpenJDK是Java开发工具包的开源实现，Oracle JDK是Java开发工具包的官方Oracle版本。尽管OpenJDK已经足够满足大多数的案例，但是许多程序比如Android Studio建议使用Oracle JDK，以避免UI/性能问题。</h4>

### 检查Java是否已经安装在Ubuntu上
<strong>打开终端输入命令：</strong>

    java -version


<strong>如果有看到类似以下的输出，则表明你的电脑上已经安装好了JDK，否则就是没有安装：</strong>

    java version "1.8.0_73"
    Java(TM) SE Runtime Environment (build 1.8.0_73-b02)
    Java HotSpot(TM) 64-Bit Server VM (build 25.73-b02, mixed mode)

<strong>安装Oracle JDK

    sudo add-apt-repository ppa:webupd8team/java

    sudo apt-get update

    sudo apt-get install oracle-java8-installer


<strong>    设置 Java 8 环境变量：</strong>


    sudo apt-get install oracle-java8-set-default

<strong>此时打开配置文件</strong>

      sudo gedit ~/.bashrc

<strong>在打开的文本编辑器末尾换行添加如下内容：(部分内容在安装时可能已经自动配置好了)</strong>


    export JAVA_HOME=/usr/local/lib/jdk1.8.0_73
    export JRE_HOME=${JAVA_HOME}/jre
    export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
    export PATH=${JAVA_HOME}/bin:$PATH
<strong>右上角保存后，在终端输入 `java -version` 或 `java` 或者`javac`回车，如果显示了版本信息或者一些帮助信息，则表示配置成功</strong>

### 2:通过官网下载安装包安装.

#### 首先去<a href="http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html">官网</a>下载最新的`jdk-8u92-linux-x64.tar.gz `

#### 打开终端创建Java目标路径文件

	sudo mkdir /usr/lib/jvm

#### 解压jdk文件到目标文件下

  	sudo tar -C /usr/lib/jvm -xzf (你下载的路径)/jdk-8u92-linux-x64.tar.gz

#### 查看本机上是否还有java可选

	sudo update-alternatives --list java
#### 若显示   `update-alternatives: 错误: 无 java 的候选项  `  则表示系统中没有java可选，可以进行以下步骤
#### 配置环境变量

	sudo gedit ~/.bashrc
#### 在打开的文本编辑器末尾换行添加如下内容：

		export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_92   

		export JRE_HOME=${JAVA_HOME}/jre

		export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib

		export PATH=${JAVA_HOME}/bin:$PATH

<strong>右上角保存后，在终端输入 `java -version` 或 `java` 或者`javac`回车，如果显示了版本信息或者一些帮助信息，则表示配置成功</strong>
