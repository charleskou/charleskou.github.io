---
title: Maven
date: 2019-08-10 15:15:21
tags: Java, 工具
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