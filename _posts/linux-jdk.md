title: JDK-Linux安装
---
Oracle[下载](http://www.oracle.com/technetwork/java/javase/downloads/index.html)Linux下的JDK压缩包
```
sudo mv jdk1.7.0_10 /usr/lib/jvm/
```
配置环境变量
```
sudo gedit ~/.profile
export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_10
export JRE_HOME=/usr/lib/jvm/jdk1.7.0_10/jre 
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
export PATH=$JAVA_HOME/bin:$PATH
```
```
$ source ~/.profile
```