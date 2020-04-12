---
title: sonar
tags: [Java, Tool]
date: 2019-08-13 16:42:01
---

## start
```sh
docker run -d --name sonarqube \
    -p 9797:9000 -p 9092:9092 \
    -v /opt/sonarqube/temp:/opt/sonarqube/temp \
    -v /opt/sonarqube/conf:/opt/sonarqube/conf \
    -v /opt/sonarqube/extensions:/opt/sonarqube/extensions \
    -e SONARQUBE_JDBC_USERNAME=username \
    -e SONARQUBE_JDBC_PASSWORD=password \
    -e SONARQUBE_JDBC_URL="jdbc:mysql://192.168.20.234:3306/sonar?useUnicode=true&characterEncoding=utf8" \
    sonarqube:7.1
```

## postgres db
```
docker pull postgres:10
docker pull sonarqube:7.9.1-community
docker run -d -p 5432:5432 -e POSTGRES_PASSWORD=1 --name postgres postgres:10
docker run -d --name sonarqube \
    -p 9000:9000 \
    -e "SONARQUBE_JDBC_URL=jdbc:postgresql://192.168.114.131:5432/sonar" \
    -e "SONARQUBE_JDBC_USERNAME=postgres" \
    -e "SONARQUBE_JDBC_PASSWORD=1" \
    sonarqube:7.9.1-community
```

## plugins
```sh
FROM sonarqube
ADD ./sonar-l10n-zh-plugin-1.15.jar /opt/sonarqube/extensions/plugins/sonar-l10n-zh-plugin-1.15.jar
ADD ./sonar-java-plugin-4.7.0.9212.jar /opt/sonarqube/extensions/plugins/sonar-java-plugin-4.7.0.9212.jar
ADD ./sonar-findbugs-plugin-3.4.4.jar /opt/sonarqube/extensions/plugins/sonar-findbugs-plugin-3.4.4.jar
ADD ./checkstyle-sonar-plugin-3.6.jar /opt/sonarqube/extensions/plugins/checkstyle-sonar-plugin-3.6.jar
ADD ./backelite-sonar-swift-plugin-0.3.2.jar /opt/sonarqube/extensions/plugins/backelite-sonar-swift-plugin-0.3.2.jar
ADD ./sonar-web-plugin-2.5.0.476.jar /opt/sonarqube/extensions/plugins/sonar-web-plugin-2.5.0.476.jar
ADD ./sonar-javascript-plugin-2.21.0.4409.jar /opt/sonarqube/extensions/plugins/sonar-javascript-plugin-2.21.0.4409.jar
```

## IDE Support
Eclipse
IDEA

maven项目sonar扫描本地配置和扫描相关命令
```
mvn sonar:sonar
```

## 术语
- Reliability 可靠性
- Security 安全
- Maintainability 可维护性
- Coverage 覆盖率
- Duplications 重复
- Quality Gate 质量门限
- Compliant Solution 兼容的解决方案

## sonar rules
建议手工关闭的规则：
1. Field names should comply with a naming convention
2. Local variable and method parameter names should comply with a naming convention
3. Method names should comply with a naming convention
4. Fields in a "Serializable" class should either be transient or serializable
5. Classes without "public" constructors should be "final"
6. Overriding methods should do more than simply call the same method in the super class
7. Static non-final field names should comply with a naming convention
8. Utility classes should not have public constructors
9. Abstract classes without fields should be converted to interfaces
10. @FunctionalInterface annotation should be used to flag Single Abstract Method interfaces
11. An abstract class should have both abstract and concrete methods
12. "throws" declarations should not be superfluou
13. Redundant modifiers should not be used
14. Control structures should use curly braces
15. Methods should not have too many return statements
16. Generic exceptions should never be thrown