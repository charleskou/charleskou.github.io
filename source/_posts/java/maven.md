---
title: Maven
date: 2019-08-10 15:15:21
tags: [Java, Tool]
---

## 常用命令
```
mvn clean
mvn package
mvn install
mvn clean package -DskipTests
mvn clean package -DskipTests -U
```
mvn install 会将项目生成的构件安装到本地Maven仓库

## mirror
阿里镜像：
```xml
<mirror>  
    <id>repo2</id>  
    <mirrorOf>central</mirrorOf>  
    <name>human readable name for this mirror.</name>  
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
</mirror>
```

## nexus
maven私服

### 配置参考
配置文件路径(示例)：C:\Users\username\.m2\settings.xml
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <localRepository/>
    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>
    <profiles>
		<profile>
			<id>nexus</id>
			<repositories>
				<repository>
					<id>nexus</id>
					<url>http://private.mvnrepository.com/repository/maven-public/</url>
					<releases>
						<enabled>true</enabled>
					</releases>
					<snapshots>
						<enabled>true</enabled>
					</snapshots>
				</repository>
			</repositories>
		</profile>
    </profiles>
	<activeProfiles>
        <activeProfile>nexus</activeProfile>
    </activeProfiles>
	<servers>
        <server>
          <id>nexus</id>
          <username>username</username>
          <password>password</password>
        </server>
    </servers> 
    <mirrors>
        <mirror>  
            <id>repo2</id>  
            <mirrorOf>central</mirrorOf>  
            <name>human readable name for this mirror.</name>  
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>  
        </mirror>
    </mirrors>
</settings>
```

### 上传至私服
```
mvn deploy
```
用来将项目生成的构件分发到远程Maven仓库
```xml
<distributionManagement>
    <repository>
        <id>nexus</id>
        <name>Nexus Release</name>
        <url>http://private.mvnrepository.com/repository/maven-releases/</url>
    </repository>
    <snapshotRepository>
        <id>nexus</id>
        <name>Nexus Snapshot</name>
        <url>http://private.mvnrepository.com/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```
Maven区别对待release版本的构件和snapshot版本的构件，snapshot为开发过程中的版本，实时，但不稳定，release版本则比较稳定。Maven会根据你项目的版本来判断将构件分发到哪个仓库。

## snapshot 更新策略

maven对构件的更新判断基本上是两种，一种是稳定版本，一种是maven特有的SNAPSHOT版本。
稳定版本很好判断，直接根据maven构件的坐标体系就能够获得。先从本地仓库中找，如果本地仓库没有，就从pom.xml和setting.xml配置的远程仓库来找。
SNAPSHOT版本的判断比较麻烦，基本步骤如下：
假设我在2014年08月22日09时40分52秒在我自己的电脑上使用 “mvn install” 构建了“com.mycompany.demo:test:1.0-SNAPSHOT”。那么Maven会在本地仓库目录“~/.m2/com/mycompany/demo/test/1.0-SNAPSHOT/”下生成文件“maven-metadata-local.xml”，内容如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata modelVersion="1.1.0">
  <groupId>com.mycompany.demo</groupId>
  <artifactId>test</artifactId>
  <version>1.0-SNAPSHOT</version>
  <versioning>
    <snapshot>
      <localCopy>true</localCopy>
    </snapshot>
    <lastUpdated>20140822084052</lastUpdated>
    <snapshotVersions>
      <snapshotVersion>
        <extension>jar</extension>
        <value>1.0-SNAPSHOT</value>
        <updated>20140822084052</updated>
      </snapshotVersion>
      <snapshotVersion>
        <extension>pom</extension>
        <value>1.0-SNAPSHOT</value>
        <updated>20140822084052</updated>
      </snapshotVersion>
    </snapshotVersions>
  </versioning>
</metadata>
```
十点钟的时候，其他同事更新了com.mycompany.demo:test:1.0-SNAPSHOT的内容，并通过 "mvn deploy" 发布到了公司的Maven服务器上。公司Maven服务器上产生了文件：test-1.0-20140822.100021-1.jar, test-1.0-20140822.100021-1.pom并更新了maven-metadata.xml，内容如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata modelVersion="1.1.0">
  <groupId>com.mycompany.demo</groupId>
  <artifactId>test</artifactId>
  <version>1.0-SNAPSHOT</version>
  <versioning>
    <snapshot>
      <timestamp>20140822.100021</timestamp>
      <buildNumber>34</buildNumber>
    </snapshot>
    <lastUpdated>20140822100021</lastUpdated>
    <snapshotVersions>
      <snapshotVersion>
        <extension>jar</extension>
        <value>1.0-20140822.100021-1</value>
        <updated>20140822100021</updated>
      </snapshotVersion>
      <snapshotVersion>
        <extension>pom</extension>
        <value>1.0-20140822.100021-1</value>
        <updated>20130407081828</updated>
      </snapshotVersion>
  </versioning>
</metadata>
```xml
在这期间我的电脑上没有发生过任何关于test的构建。

某一天，我需要构建一个依赖于test的项目，于是我运行了mvn package来打包。这个时候，maven做了什么呢（背景：我通过配置镜像，使我本地Maven的任何资源都是从公司的Maven服务器下载的）？
- Step1：从公司的Maven服务器上下载maven-metadata.xml，重命名为“maven-metadata-<RepositoryID>.xml”，并保存到本地仓库相应目录。
- Step2：比较maven-metadata-local.xml与maven-metadata-<RepositoryID>.xml中的lastUpdated时间戳的值。
如果maven-metadata-local.xml中的时间戳比较大，则终止。
如果maven-metadata-<RepositoryID>.xml中的时间戳较大，则从公司Maven服务器上下载最新版本。即：testu-1.0.1-20130407.081828-34.jar。
这个过程分两步：
（1）下载test-1.0-20140822.100021-1.jar到本地Maven仓库。
（2）将test-1.0-20140822.100021-1.jar复制一份，覆盖掉原先的test-1.0-SNAPSHOT.jar。也就是说，如果Maven从远程仓库下载了最新的SNAPSHOT发布包的话，那么最新的待时间戳的包和xxx-SNAPSHOT包是完全一样的。

作者：邓涛
链接：https://www.zhihu.com/question/24930782/answer/29532274
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。