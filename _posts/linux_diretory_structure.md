title: linux 目录结构
---
这是关于linxu的一篇文章，记录一下这一阶段看的内容，当我第一次用linux的时候是大一的，当然那个时候谈不上用，就是纯粹的装系统，装了之后没在上面写过一行代码，装也不知道自己怎么稀里糊涂装的，对于分区啊，那就是照着网上的来，也不明白为什么要分 / 和 /home 啊什么的，分区是要根据自己的需求来的，首先看我画的一张表，是用xmind画的。![](/images/linux-directory.png)
想看备注的朋友，可以点击 [链接](http://yun.baidu.com/share/link?shareid=273522111&uk=20168775) 
图右边一部分是必须和 / 在同一个分区，因为系统启动的时候只加载 / 分区，在/bin等目录有系统启动所必须的命令和文件。左边一部分就看自己的需求了，像 /var 这个目录，如果是邮件服务的话，所有邮件都在里面，所以这个目录可以单独分开，/usr 这个目录还是比较神奇的，大多数软件都是安装在这个目录的，而 /usr 的子目录 /local 的目录结构又是和 /usr 比较相似。yum 或 apt-get 的软件大多都是安装在 /usr 中的， 而一般自己安装的软件都是在 /usr/local中的。