---
title: Ubuntu下shadowsocks 配合SwitchyOmega科学上网
date: 2017-11-03
tag: ShadowSocks
---

最近重新装起了阔别已久的ubuntu，第一件事当然是科学上网啊。大概需要下面几步

### 1.一个代理服务账号

&nbsp;&nbsp;这里我推荐 <a href="https://jikess.org/">jikess</a>，一个月只需要12大洋，200G代理流量，速度也很客观。自行注册购买，或者也可以先领几十M先体验下。

![](https://desk-fd.zol-img.com.cn/t_s1024x768c5/g5/M00/02/08/ChMkJlbKzWGIZIhdAAQNZj09ufQAALJAQMP9cYABA1-499.jpg)

<!--more-->

### 2. ubuntu使用shadowsocks

 <b>1.安装shadowsocks命令行程序，配置命令。

 2.安装shadowsocks GUI图形界面程序，配置。
</b>

#### 第一种安装shadowsocks命令行程序

用PIP安装很简单

        sudo apt-get update
        sudo apt-get install python-pip
        sudo apt-get install python-setuptools m2crypto
        
接着安装shadowsocks

	pip install shadowsocks
    
如果是ubuntu16.04 直接 (16.04 里可以直接用apt 而不用 apt-get 这是一项改进）

	sudo apt install shadowsocks
当然你在安装时候肯定有提示需要安装一些依赖比如`python-setuptools m2crypto` ，依照提示安装然后再安装就好。也可以网上搜索有很多教程的。

<h4>启动shadowsocks</h4>

安装好后，在本地我们要用到sslocal ，终端输入sslocal --help 可以查看帮助，像这样

<img src="http://7xta11.com1.z0.glb.clouddn.com/2018-01-04%2017-03-17%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png">

通过帮助提示我们知道各个参数怎么配置，比如 sslocal -c 后面加上我们的json配置文件，或者像下面这样直接命令参数写上运行。

比如

	sslocal -s 11.22.33.44 -p 50003 -k "123456" -l 1080 -t 600 -m aes-256-cfb
 -s表示服务IP, -p指的是服务端的端口，-l是本地端口默认是1080, -k 是密码（要加””）, -t超时默认300,-m是加密方法默认`aes-256-cfb`，
 
 <b>为了方便我推荐直接用sslcoal -c 配置文件路径 这样的方式，简单好用。</b>

我们可以在/home/{user}/ 下新建个文件shadowsocks.json  ({user}是你自己电脑上的用户名)。内容是这样：

    {
        "server":"11.22.33.44",
        "server_port":50003,
        "local_port":1080,
        "password":"123456",
        "timeout":600,
        "method":"aes-256-cfb"
    }

server  你服务端的IP
servier_port  你服务端的端口
local_port  本地端口，一般默认1080
passwd  ss服务端设置的密码
timeout  超时设置 和服务端一样
method  加密方法 和服务端一样

确定上面的配置文件没有问题，然后我们就可以在终端输入 `sslocal -c /home/{user}/shadowsocks.json` 回车运行。如果没有问题的话，下面会是这样…

<img src="http://7xta11.com1.z0.glb.clouddn.com/2018-01-04%2017-11-06%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png">


#### 第二种 安装图形界面 
<b>在 Ubuntu下 通过PPA源安装，仅支持Ubuntu 14.04或更高版本。</b>

    sudo add-apt-repository ppa:hzwhuang/ss-qt5
    sudo apt-get update
    sudo apt-get install shadowsocks-qt5

由于是图形界面，配置和windows基本没啥差别就不赘述了。

经过上面两种方式的配置，你只是启动了sslocal 但是要上网你还需要配置下浏览器到指定到代理端口比如1080才可以正式上网。你可以去系统的代理设置按照说明设置代理，但一般是全局的，然而我们访问baidu,taobao等着些网站如果用代理就有点绕了，而且还会浪费服务器流量。我们最好配置我们的浏览器让它可以自动切换，该用代理用代理该直接连接自动直接连接。所以请看配置浏览器。

### 3.配置浏览器（chrome)

我们需要给chrome安装SwitchyOmega插件，但是没有代理之前是不能从谷歌商店安装这个插件的，但是我们可以从Github上直接下载最新版 https://github.com/FelisCatus/SwitchyOmega/releases/ （这个是chrome的）然后浏览器地址打开chrome://extensions/，将下载的插件托进去安装。


<b>上面也是我之前一直用的方法，屡试不爽。但是这次不知道为啥在拖进chrome后始终没反应，无法离线安装。于是我决定先配置全局代理，这样chrome可以直接访问谷歌商店，然后再直接在线安装SwitchyOmega，实现代理自动切换的目的。</b>

<h4>先配置全局代理</h4>

1、安装GenPAC 
GenPAC 是基于gfwlist的代理自动配置（Proxy Auto-config）文件生成工具，支持自定义规则。在多数情况下，我们更希望使用PAC模式的代理，让我们访问国内网站时不再先绕地球跑一圈，在Windows和Mac上的shadowsocks客户端可以轻松切换到PAC模式，而在Ubuntu上我们需要使用pac文件来设置系统代理以达到相同的效果

    sudo pip install genpac
    pip install --upgrade genpac
    
2、使用GenPAC生成pac文件

	 genpac -p "SOCKS5 127.0.0.1:1080" --gfwlist-proxy="SOCKS5 127.0.0.1:1080" --gfwlist-url=https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt --output="autoproxy.pac"
    

3、设置全局代理

点击：System settings > Network > Network Proxy，选择 Method 为 Automatic，设置 Configuration URL 为 autoproxy.pac 文件的路径，点击 Apply System Wide。
格式如：``file:///home/{user}/autoproxy.pac``

<img src="http://7xta11.com1.z0.glb.clouddn.com/2018-01-05%2014-28-09%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png">

4、安装switchyOmega
  此时应该可以按照一定过滤规则访问外网了，但仍配置下浏览器（毕竟善始善终）
  
  <img src="http://7xta11.com1.z0.glb.clouddn.com/2018-01-05%2014-32-26%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png">
  

<h4> 配置switchyOmega</h4>

1.配置代理地址

安装好插件会自动跳到设置选项，有提示你可以跳过。左边新建情景模式-选择代理服务器-比如命名为proxy（叫什么无所谓）其他默认之后创建，之后在代理协议选择SOCKS5，地址为127.0.0.1,端口默认1080 。然后保存即应用选项。

<img src="http://7xta11.com1.z0.glb.clouddn.com/2018-01-05%2014-35-17%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png">

2.设置自动切换

接着点击自动切换 ( Auto switch）上面的不用管，在按照规则列表匹配请求后面选择刚才新建的proxy，默认情景模式选择直接连接。点击应用选项保存。再往下规则列表设置选择AutoProxy 然后将这个地址(`https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt`) 填进去，点击下面的立即更新情景模式，会有提示更新成功！

<img src="http://7xta11.com1.z0.glb.clouddn.com/2018-01-05%2014-37-58%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png">


<b>点击浏览器右上角的SwitchyOmega图标，下面选择自动切换，然后打开google.com试试，其他的就不在这贴图了。</b>


<h4>至此shadowsocks搭建完成，天高任鸟飞，海阔任鱼跃。</h4>


