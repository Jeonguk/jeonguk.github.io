---
layout: post
title:  "command-line-arguments-in-spring-boot"
date:   2018-04-06 18:59:10 +0700
categories: [java,spring]
---

# Command-Line Arguments in Spring Boot

```Maven Command-Line Arguments```

### Spring Boot 1.x
- For Spring Boot 1.x, we can pass the arguments to our application using -Drun.arguments:

```
mvn spring-boot:run -Drun.arguments=--customArgument=custom
```

- multiple parameters
```
mvn spring-boot:run -Drun.arguments=--spring.main.banner-mode=off,--customArgument=custom
```

### Spring Boot 2.x

```
mvn spring-boot:run -Dspring-boot.run.arguments=--spring.main.banner-mode=off,--customArgument=custom
```

```Gradle Command-Line Arguments```

```
bootRun {
    if (project.hasProperty('args')) {
        args project.args.split(',')
    }
}
```

- command-line arguments
```
./gradlew bootRun -Pargs=--spring.main.banner-mode=off,--customArgument=custom
```

```Overriding System Properties```
- application.properties

```
server.port=8081
spring.application.name=SampleApp
```

- Spring Boot 1.x
```
mvn spring-boot:run -Drun.arguments=--server.port=8085
```

- Spring Boot 2.x
```
mvn spring-boot:run -Dspring-boot.run.arguments=--server.port=8085
```


```Accessing Command-Line Arguments```

```java
@SpringBootApplication
public class Application extends SpringBootServletInitializer {
    public static void main(String[] args) {
        for(String arg:args) {
            System.out.println(arg);
        }
        SpringApplication.run(Application.class, args);
    }
}
```




