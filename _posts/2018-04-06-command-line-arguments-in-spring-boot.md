---
layout: post
title:  "command-line-arguments-in-spring-boot"
date:   2018-04-06 18:59:10 +0700
categories: [java,spring]
---

# Command-Line Arguments in Spring Boot

```Maven Command-Line Arguments```

### Spring Boot 1.x

```
mvn spring-boot:run -Drun.arguments=--customArgument=custom
```

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



