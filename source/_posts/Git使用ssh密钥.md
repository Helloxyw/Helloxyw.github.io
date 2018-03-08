---
title: Git使用ssh密钥
date: 2016-09-13 19:59:53
tags: Git
---

![](https://desk-fd.zol-img.com.cn/t_s1440x900c5/g5/M00/06/0B/ChMkJllkKMeIJ6CSAAdGdB9XHNIAAeWIAJf87AAB0aM436.jpg)

<!--more-->

---
<b>
git使用https协议，每次pull, push都要输入密码，相当的烦。
使用git协议，然后使用ssh密钥。这样可以省去每次都输密码。
</b>




 ----------
大概需要三个步骤：
 1. 本地生成密钥对；
 2. 设置github上的公钥；
 3. 修改git的remote url为git协议。


----------


## 详细讲解第3部分

修改你本地的ssh remote url. 不用https协议，改用git 协议

可以用`git remote -v `查看你当前的`remote url`

    $ git remote -v
    origin	https://github.com/Helloxyw/Helloxyw.github.io.git (fetch)
    origin	https://github.com/Helloxyw/Helloxyw.github.io.git (push)
可以看到是使用https协议进行访问的。

你可以使用浏览器登陆你的github，在上面可以看到你的ssh协议相应的url。类似如下：

    git@github.com:someaccount/someproject.git

这时，你可以使用`git remote set-url` 来调整你的`url`。

    git remote set-url origin git@github.com:someaccount/someproject.git

完了之后，你便可以再用` git remote -v` 查看一下。
    $ git remote -v
    origin	git@github.com:Helloxyw/Helloxyw.github.io.git (fetch)
    origin	git@github.com:Helloxyw/Helloxyw.github.io.git (push)

 至此，OK。

你可以用`    git fetch, git pull , git push `， 现在进行远程操作，应该就不需要输入密码那么烦了。


                         			Writhen On 18-Apr-2016 By Ricardo Xu
