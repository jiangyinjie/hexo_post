title: Maven-Nexus
---
下载Nexus http://nexus.sonatype.org/downloads/。
## 1.配置nexus 
>首先登录，默认地址http://localhost:8081/nexus/，默认用户名密码为admin/admin123. nexus默认是关闭远程索引下载功能的。开启的方式： 
点击Administration菜单下面的Repositories，将这三个仓库Apache Snapshots，Codehaus Snapshots，Maven Central 的 Download Remote Indexes修改为true。然后在这三个仓库上分别右键，选择Re-index，这样Nexus就会去下载远程的索引文件。 
## 2.管理仓库 
    以管理员用户登陆然后点击左边导航菜单Repositories。下面的Repositories。Nexus提供了三种不同的仓库。 
### (1) 代理仓库 
    一个代理仓库是对远程仓库的一个代理。默认情况下，Nexus自带了如下配置的代理仓库： 
>Apache Snapshots 
这个仓库包含了来自于Apache软件基金会的快照版本。http://people.apache.org/repo/m2-snapshot-repository 
Codehaus Snapshots 
这个仓库包含了来自于Codehaus的快照版本。 http://snapshots.repository.codehaus.org/ 
Central Maven Repository 
这是中央Maven仓库（发布版本）。 http://repo1.maven.org/maven2/ 
###(2)宿主仓库 
>一个宿主仓库是由Nexus托管的仓库。Maven自带了如下配置的宿主仓库。 
3rd Party 
这个宿主仓库应该用来存储在公共Maven仓库中找不到的第三方依赖。这种依赖的样例有：你组织使用的，商业的，私有的类库如Oracle JDBC驱动。 
Releases 
这个宿主仓库是你组织公布内部发布版本的地方。 
Snapshots 
这个宿主仓库是你组织发布内部快照版本的地方。 
###(3)虚拟仓库 
>一个虚拟仓库作为Maven 1的适配器存在。Nexus自带了一个central-m1虚拟仓库 
##3. 管理组 
组是Nexus一个强大的特性，它允许你在一个单独的URL中组合多个仓库。Nexus自带了两个组：public和public-snapshots。public组中组合了三个宿主仓库：3rd Party, Releases, 和Snapshots，还有中央Maven仓库。而public-snapshots组中组合了Apache Snapshots和Codehaus Snapshots仓库。 
4. 配置maven 
要让maven使用Nexus作为仓库，要修改~/.m2/settings.xml. 
Xml代码 
```
<profiles> 
   <profile> 
     <id>nexus</id> 
     <repositories> 
       <repository> 
           <id>nexus</id> 
           <name>local private nexus</name> 
           <url>http://localhost:8081/nexus/content/groups/public</url> 
       </repository> 
     </repositories> 
   </profile> 
   <profile> 
     <id>nexus-snapshots</id> 
     <repositories> 
       <repository> 
           <id>nexus-snapshots</id> 
           <name>local private nexus snapshots</name> 
           <url>http://localhost:8081/nexus/content/groups/public-snapshots</url> 
       </repository> 
     </repositories> 
   </profile> 
</profiles>
<activeProfiles> 
    <activeProfile>nexus</activeProfile> 
    <activeProfile>nexus-snapshots</activeProfile> 
</activeProfiles>
```
##5.部署构件至Nexus 
   要部署构件至Nexus，在distributionManagement中提供仓库URL，然后运行mvn deploy。Maven会通过一个简单的HTTP PUT将项目`POM`和构件推入至你的Nexus安装。需要配置你项目POM中distributionManagement部分的repository。 
Xml代码 
```
<distributionManagement> 
<repository> 
    <id>releases</id> 
    <name>Internal Releases</name> 
    <url>http://localhost:8081/nexus/content/repositories/releases</url> 
</repository> 
<snapshotRepository> 
    <id>Snapshots</id> 
    <name>Internal Snapshots</name> 
    <url>http://localhost:8081/nexus/content/repositories/snapshots</url> 
</snapshotRepository> 
</distributionManagement>
```
这样还没完，这时如果部署会报错，还要在～/.m2/settings.xml中添加如下的服务器登录信息：
Xml代码 
```
<server> 
<id>releases</id> 
<username>admin</username> 
<password>admin123</password> 
</server> 
<server> 
<id>Snapshots</id> 
<username>admin</username> 
<password>admin123</password> 
</server>
```
## 6.Nexus监听端口 
默认情况下，Nexus监听端口8081。你可以更改这个端口，通过更改${NEXUS_HOME}/conf/plexus.properties的值，为此，停止Nexus，更改文件中applicationPort的值，然后重启Nexus。

## 7.私服仓库设置
```
<mirrors>
    <mirror>
      <id>nexus</id>
      <mirrorOf>central</mirrorOf>
      <name>central repository.</name>
      <url>http://ip:8081/nexus/content/groups/public</url>
    </mirror>
</mirrors>
```