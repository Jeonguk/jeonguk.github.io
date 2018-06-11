---
layout: post
title:  "Spring Core Annotations"
date:   2018-06-11 11:42:10 +0700
categories: [java,spring]
---

# Spring Core Annotations

### org.springframework.beans.factory.annotation과 org.springframework.context.annotation 패키지의 주석을 사용하여 Spring DI 엔진의 기능을 활용할 수 있습니다.

```DI-Related Annotations```

> @Autowired

- Constructor injection:

```java
class Car {
    Engine engine;
 
    @Autowired
    Car(Engine engine) {
        this.engine = engine;
    }
}
```

- Setter injection:

```java
class Car {
    Engine engine;
 
    @Autowired
    void setEngine(Engine engine) {
        this.engine = engine;
    }
}
```

- Field injection:

```java
class Car {
    @Autowired
    Engine engine;
}
```

> @Bean

- @Bean marks a factory method which instantiates a Spring bean:

```java
@Bean
Engine engine() {
    return new Engine();
}
```

```java
@Bean("engine")
Engine getEngine() {
    return new Engine();
}
```


