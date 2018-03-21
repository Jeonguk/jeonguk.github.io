---
layout: post
title:  "Maven Dependency Scopes"
date:   2018-03-21 18:54:10 +0700
categories: [java,maven,language]
---

# Maven Dependency Scopes

```
Maven은 Java 생태계에서 가장 많이 사용되는 빌드 도구 중 하나이며 핵심 기능 중 하나는 종속성 관리입니다.
```

### Dependency Scopes

```
종속성 범위는 종속성의 전이성을 제한하는 데 도움이 될 수 있으며 빌드 된 다른 작업에 대한 클래스 경로를 수정합니다. Maven에는 6 개의 기본 종속성 범위가 있습니다.

그리고 가져 오기를 제외한 각 범위가 전이 의존성에 영향을 미친다는 것을 이해하는 것이 중요합니다.
```

``` Compile ```

- 이 범위는 다른 범위가 제공되지 않는 경우 기본 범위입니다.
- 이 범위의 종속성은 모든 빌드 작업에서 프로젝트의 클래스 경로에서 사용할 수 있으며 종속 프로젝트에 전파됩니다.
더 중요한 것은 이러한 종속성 또한 전이적입니다.

```xml
<dependency>
    <groupId>commons-lang</groupId>
    <artifactId>commons-lang</artifactId>
    <version>2.6</version>
</dependency>
```


