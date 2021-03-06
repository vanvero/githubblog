Title: ambari 实践笔记
Date: 2018-07-03 19:20
Modified: 2018-07-03 19:20
Category: Big data
Tags: ambari
Slug: J7
Authors: nJcx
Summary: 记录一下ambari实践笔记 ~


#### 介绍


#### 安装

系统：centos7.3，maven-3.5.4，ambari-2.7.1

目录： /opt

先安装maven

```bash
# wget http://ftp.yz.yamagata-u.ac.jp/pub/network/apache/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz
# tar -zxvf apache-maven-3.5.4-bin.tar.gz
```
添加环境变量

```bash

export MAVEN_HOME=/opt/apache-maven-3.5.4
export PATH=${MAVEN_HOME}/bin:${PATH}

```
source /etc/profile  

找到conf/settings.xml,修改Maven源

```xml

 <mirrors>    
            <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>https://maven.aliyun.com/repository/releases/</url>
        </mirror>
    
        <!-- 中央仓库1 -->
        <mirror>
            <id>repo1</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>https://maven.aliyun.com/repository/central/</url>
        </mirror>
    
        <!-- 中央仓库2 -->
        <mirror>
            <id>repo2</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>https://maven.aliyun.com/repository/public/</url>
        </mirror>
  </mirrors> 

```

```bash

# wget https://mirrors.aliyun.com/apache/ambari/ambari-2.7.1/apache-ambari-2.7.1-src.tar.gz 
# tar -zxvf apache-ambari-2.7.1-src.tar.gz
# cd apache-ambari-2.7.1-src

mvn versions:set -DnewVersion=2.7.1.0.0 
pushd ambari-metrics
mvn versions:set -DnewVersion=2.7.1.0.0
popd


```
接下来会开启啰嗦模式~，耐心休息休息

```bash

mvn -B clean install rpm:rpm -DnewVersion=2.7.1.0.0 -DbuildNumber=90430db08a5f543a97d97918cf5f711f2786ad8a -DskipTests -Dpython.ver="python >= 2.6"

```

- server安装

编译后，在如下目录查找，ambari-server/target/rpm/ambari-server/RPMS/noarch/

```bash

yum install ambari-server*.rpm
 
```
```bash
ambari-server setup
ambari-server start
```

- agent 安装
编译后，在如下目录查找，ambari-agent/target/rpm/ambari-agent/RPMS/x86_64/ 

```bash
yum install ambari-agent*.rpm
```

vim /etc/ambari-agent/ambari.ini

```bash
[server]
hostname=localhost

```
ambari-agent start

访问页面
http://<ambari-server-host>:8080
账户：admin：admin


方法二：

master

```bash
wget -nv http://public-repo-1.hortonwor.com/ambari/centos7/2.x/updates/2.7.1.0/ambari.repo -O /etc/yum.repos.d/ambari.repo

yum install ambari-server -y

```

slave1

```bash

yum install ambari-agent -y

```

