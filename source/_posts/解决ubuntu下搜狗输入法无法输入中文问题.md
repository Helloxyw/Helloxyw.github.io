


<h1>解决ubuntu16.04下搜狗输入法无法输入中文问题</h1>

今天打开电脑,突然发现一直正常使用的搜狗输入法无法无法输入中文(<b>具体现象是，可以呼出搜狗输入法界面，但是候选词列表无显示</b>).在查阅了别人的博客后解决了这个问题，下面是解决方案.

我的版本号:
* OS：`Ubuntu16.04LST`
* 搜狗版本:`sogoupinyin_2.1.0.0082_amd64.deb`
<center>
![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1490240967&di=a8c79abaa880c6efe8cd7a471576e4b1&imgtype=jpg&er=1&src=http%3A%2F%2Fwww.xiazaizhijia.com%2Fuploads%2Fallimg%2F140217%2F36-14021GAA2615.png)

</center>
<h3>方法一:</h3>
重启搜狗输入法,看是否有效
	
	
		~$ killall fcitx 
		~$ killall sogou-qinpanel
		~$ fcitx

<h3>方法二：检查修复安装依赖</h3>
我本机依赖完好,所以应该不是依赖的问题。但如果刚安装搜狗无法使用,可以尝试下修复依赖.
	
		~$ sudo apt  install -f
		

<h3>方法三:删除配置文件,重启搜狗</h3>
ubuntu下搜狗配置文件在~/.config下的3个文件夹内:
	`SogouPY`,`SogouPY.users`、`sogou-qimpanel`
	删除这3个文件夹,之后重启.

<b>注:我就是用这个方法解决了无法输入中文问题</b>

<h3>总结</h3>
Linux下软件经常会因为配置问题而崩溃，最直观的现象就是无法正常使用.因此，如果Linux下正常使用的软件，突然崩溃无法使用，可以尝试删除或修改配置文件的方式尝试解决。
