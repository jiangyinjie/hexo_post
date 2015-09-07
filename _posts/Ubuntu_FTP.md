title: Ubuntu FTP
---
```
sudo apt-get install vsftpd
```
命令执行过程中，安装程序会给本地创建一个名为“ftp”的用户组，命令执行完之后会自动启动FTP服务。


开启、停止、重启vsftpd服务也很简单：

```
service vsftpd start | stop | restart
```
新安装的vsftpd默认是可以匿名访问，如果只想给某一个用户专门访问某一目录下的权限，则需要修改vsftpd的配置了。

首先，创建一个专门用来访问的用户，例如叫“test”：

```
mkdir -p /home/test
useradd test -g ftp -d /home/test -s /sbin/nologin
```
设置密码:

```
passwd test
```
修改vsftpd的配置文件“vi /etc/vsftpd.conf”：

```
#禁止匿名访问
anonymous_enable=NO
#接受本地用户
local_enable=YES
#可以上传
write_enable=YES
#启用在chroot_list_file的用户只能访问根目录
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd.chroot_list
在/etc/vsftpd.chroot_list添加受访问目录限制的用户：
```

```
echo "test" >> /etc/vsftpd.chroot_list
```
安装过程中遇到的一些问题：

“530 Login incorrect”

在 /etc/shells 最后一行添加“/sbin/nologin”

“500 OOPS: vsftpd: refusing to run with writable root inside chroot()”

启用了chroot的话，根目录要设置为不可写

chmod a-w /home/test
OK，重启vsftpd之后就可以使用上面新创建的账号访问
