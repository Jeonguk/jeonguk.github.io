---
layout: post
title:  "HikariCP MySQL Configuration"
date:   2018-01-22 10:50:10 +0700
categories: [mysql, configuration]
---

### HikariCP MySQL Configuration Information

* MySQL 권장 설정 설명

* prepStmtCacheSize

```
이 값은 MySQL 드라이버가 연결 당 캐시 할 준비된 명령문의 수를 설정합니다. 기본값은 보수적입니다.이 값을 250-500 사이로 설정하는 것이 좋습니다.
```

* prepStmtCacheSqlLimit

```
이것은 드라이버가 캐시 할 준비된 SQL 문의 최대 길이입니다. 
MySQL의 디폴트는 256이다. 
특히 Hibernate와 같은 ORM 프레임 워크에서는이 디폴트가 생성 된 구문 길이의 임계 값보다 훨씬 낮다. 
권장 설정은 2048입니다.
```

* cachePrepStmts

```
캐시가 사실상 사용 불가능한 경우, 위의 매개 변수 중 어느 것도 디폴트 값대로 적용되지 않습니다. 이 매개 변수를 true로 설정해야합니다.
```

* useServerPrepStmts

```
최신 버전의 MySQL은 server-side prepared statements 를 지원하므로 상당한 성능 향상을 제공 할 수 있습니다. 
이 속성을 true로 설정합니다.
```

* HikariCP의 일반적인 MySQL configuration 은 다음과 같습니다.

```
jdbcUrl=jdbc:mysql://localhost:3306/simpsons
user=test
password=test
dataSource.cachePrepStmts=true
dataSource.prepStmtCacheSize=250
dataSource.prepStmtCacheSqlLimit=2048
dataSource.useServerPrepStmts=true
dataSource.useLocalSessionState=true
dataSource.useLocalTransactionState=true
dataSource.rewriteBatchedStatements=true
dataSource.cacheResultSetMetadata=true
dataSource.cacheServerConfiguration=true
dataSource.elideSetAutoCommits=true
dataSource.maintainTimeStats=false
```
