---
layout: post
title:  "spring-boot-tutorial"
date:   2018-04-10 16:33:10 +0700
categories: [java,spring]
---

# Spring Boot Tutorial

### Making the Spring Boot Project with Maven

```
mvn archetype:generate -DgroupId=com.jeonguk.example -DartifactId=JCG-SpringBoot-example -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```
- spring-boot-starter-web
    - 이 의존성은 이 프로젝트를 웹 프로젝트로 표시하고 컨트롤러를 만들고 웹 관련 클래스를 만들기 위해 종속성을 추가합니다.
- spring-boot-starter-data-jpa
    - 이 의존성은 많은 데이터베이스 관련 작업을 수행 할 수있는 JPA 지원을 프로젝트에 제공합니다.
- h2
    - H2는 메모리 내장 데이터베이스입니다.
- spring-boot-starter-tes
    - 이 의존성은 모든 테스트 관련 JAR을 JUnit 및 Mockito와 같은 프로젝트로 수집합니다.

```pom.xml```

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>1.5.10.RELEASE</version>
  <relativePath/> <!-- lookup parent from repository -->
</parent>

<properties>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
  <java.version>1.8</java.version>
</properties>

<dependencies>

  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
  </dependency>

  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>

  <dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
  </dependency>
  
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
  </dependency>
  
</dependencies>
```

- 코드를 JAR에 패키지하기 위해 JAR 또는 WAR로 응용 프로그램 자체의 패키징을 관리하는 훌륭한 도구 인 spring-boot-maven-plugin을 사용할 것입니다. 다음은 종속 파일에 추가 할 수있는 방법입니다.

```pom.xml```

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

- Check Dependency Tree

```
mvn dependency:tree
```

```Dependency Tree```

```
[INFO] --------------------
[INFO] Building JCG-SpringBoot-example 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.10:tree (default-cli) @ JCG-SpringBoot-example ---
[INFO] com.javacodegeeks.example:JCG-SpringBoot-example:jar:1.0-SNAPSHOT
[INFO] +- org.springframework.boot:spring-boot-starter-data-jpa:jar:1.5.10.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:1.5.10.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot:jar:1.5.10.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-autoconfigure:jar:1.5.10.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-starter-logging:jar:1.5.10.RELEASE:compile
[INFO] |  |  |  +- ch.qos.logback:logback-classic:jar:1.1.11:compile
[INFO] |  |  |  |  \- ch.qos.logback:logback-core:jar:1.1.11:compile
[INFO] |  |  |  +- org.slf4j:jul-to-slf4j:jar:1.7.25:compile
[INFO] |  |  |  \- org.slf4j:log4j-over-slf4j:jar:1.7.25:compile
[INFO] |  |  \- org.yaml:snakeyaml:jar:1.17:runtime
[INFO] |  +- org.springframework.boot:spring-boot-starter-aop:jar:1.5.10.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-aop:jar:4.3.14.RELEASE:compile
[INFO] |  |  \- org.aspectj:aspectjweaver:jar:1.8.13:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-jdbc:jar:1.5.10.RELEASE:compile
[INFO] |  |  +- org.apache.tomcat:tomcat-jdbc:jar:8.5.27:compile
[INFO] |  |  |  \- org.apache.tomcat:tomcat-juli:jar:8.5.27:compile
[INFO] |  |  \- org.springframework:spring-jdbc:jar:4.3.14.RELEASE:compile
[INFO] |  +- org.hibernate:hibernate-core:jar:5.0.12.Final:compile
[INFO] |  |  +- org.jboss.logging:jboss-logging:jar:3.3.1.Final:compile
[INFO] |  |  +- org.hibernate.javax.persistence:hibernate-jpa-2.1-api:jar:1.0.0.Final:compile
[INFO] |  |  +- org.javassist:javassist:jar:3.21.0-GA:compile
[INFO] |  |  +- antlr:antlr:jar:2.7.7:compile
[INFO] |  |  +- org.jboss:jandex:jar:2.0.0.Final:compile
[INFO] |  |  +- dom4j:dom4j:jar:1.6.1:compile
[INFO] |  |  \- org.hibernate.common:hibernate-commons-annotations:jar:5.0.1.Final:compile
[INFO] |  +- org.hibernate:hibernate-entitymanager:jar:5.0.12.Final:compile
[INFO] |  +- javax.transaction:javax.transaction-api:jar:1.2:compile
[INFO] |  +- org.springframework.data:spring-data-jpa:jar:1.11.10.RELEASE:compile
[INFO] |  |  +- org.springframework.data:spring-data-commons:jar:1.13.10.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-orm:jar:4.3.14.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-context:jar:4.3.14.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-tx:jar:4.3.14.RELEASE:compile
[INFO] |  |  +- org.springframework:spring-beans:jar:4.3.14.RELEASE:compile
[INFO] |  |  +- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO] |  |  \- org.slf4j:jcl-over-slf4j:jar:1.7.25:compile
[INFO] |  \- org.springframework:spring-aspects:jar:4.3.14.RELEASE:compile
[INFO] +- org.springframework.boot:spring-boot-starter-web:jar:1.5.10.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-tomcat:jar:1.5.10.RELEASE:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:8.5.27:compile
[INFO] |  |  |  \- org.apache.tomcat:tomcat-annotations-api:jar:8.5.27:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:8.5.27:compile
[INFO] |  |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:8.5.27:compile
[INFO] |  +- org.hibernate:hibernate-validator:jar:5.3.6.Final:compile
[INFO] |  |  +- javax.validation:validation-api:jar:1.1.0.Final:compile
[INFO] |  |  \- com.fasterxml:classmate:jar:1.3.4:compile
[INFO] |  +- com.fasterxml.jackson.core:jackson-databind:jar:2.8.10:compile
[INFO] |  |  +- com.fasterxml.jackson.core:jackson-annotations:jar:2.8.0:compile
[INFO] |  |  \- com.fasterxml.jackson.core:jackson-core:jar:2.8.10:compile
[INFO] |  +- org.springframework:spring-web:jar:4.3.14.RELEASE:compile
[INFO] |  \- org.springframework:spring-webmvc:jar:4.3.14.RELEASE:compile
[INFO] |     \- org.springframework:spring-expression:jar:4.3.14.RELEASE:compile
[INFO] +- com.h2database:h2:jar:1.4.196:runtime
[INFO] \- org.springframework.boot:spring-boot-starter-test:jar:1.5.10.RELEASE:test
[INFO]    +- org.springframework.boot:spring-boot-test:jar:1.5.10.RELEASE:test
[INFO]    +- org.springframework.boot:spring-boot-test-autoconfigure:jar:1.5.10.RELEASE:test
[INFO]    +- com.jayway.jsonpath:json-path:jar:2.2.0:test
[INFO]    |  \- net.minidev:json-smart:jar:2.2.1:test
[INFO]    |     \- net.minidev:accessors-smart:jar:1.1:test
[INFO]    |        \- org.ow2.asm:asm:jar:5.0.3:test
[INFO]    +- junit:junit:jar:4.12:test
[INFO]    +- org.assertj:assertj-core:jar:2.6.0:test
[INFO]    +- org.mockito:mockito-core:jar:1.10.19:test
[INFO]    |  \- org.objenesis:objenesis:jar:2.1:test
[INFO]    +- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO]    +- org.hamcrest:hamcrest-library:jar:1.3:test
[INFO]    +- org.skyscreamer:jsonassert:jar:1.4.0:test
[INFO]    |  \- com.vaadin.external.google:android-json:jar:0.0.20131108.vaadin1:test
[INFO]    +- org.springframework:spring-core:jar:4.3.14.RELEASE:compile
[INFO]    \- org.springframework:spring-test:jar:4.3.14.RELEASE:test
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```


### Gradle equivalent for the build file

- Maven은 우수한 빌드 시스템이지만, Gradle을 선호하는 경우 pom.xml 빌드 파일의 Gradle에 해당합니다.

```build.gradle```

```
buildscript {
	ext {
		springBootVersion = '1.5.10.RELEASE'
	}
	repositories {
		mavenCentral()
	}
	dependencies {
		classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
	}
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'

group = 'com.javacodegeeks.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = 1.8

repositories {
	mavenCentral()
}


dependencies {
	compile('org.springframework.boot:spring-boot-starter-data-jpa')
	compile('org.springframework.boot:spring-boot-starter-web')
	runtime('com.h2database:h2')
	testCompile('org.springframework.boot:spring-boot-starter-test')
}
```

