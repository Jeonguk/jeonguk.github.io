---
layout: post
title:  "MongoDB-vs-MySQL"
date:   2018-02-13 10:20:10 +0700
categories: [database,nosql]
---

# MongoDB vs MySQL

* 이 두 데이터베이스 간의 주요 차이점은 데이터 표현입니다. MongoDB에서 데이터는 JSON과 유사한 문서의 모음으로 표현되는 반면 MySQL에서는 테이블, 행 및 열로 표현됩니다
* MySQL은 "Structured Query Language"를 지원하여 데이터베이스 시스템의 문자열을 쿼리하는 반면 MongoDB는 객체 지향 쿼리를 지원합니다. 따라서 MongoDB는 SQL 주입 공격이나 구문 분석을 제공하지 않습니다.
* 하나의 큰 차이는 MySQL은 MongoDB가 다차원 문서를 제공하는 동안 JOIN 연산을 지원하여 여러 테이블을 쿼리 할 수 ​​있습니다.
* MySQL은 단일 원자 트랜잭션 내에서 여러 연산을 지원하고 롤백 기능을 제공하지만 MongoDB에서는 트랜잭션을 지원하지 않습니다
* MongoDB의 가장 좋은 점 중 하나는 개발자가 미리 스키마를 정의 할 책임이 없으며 레코드를 즉시 생성 할 수 있다는 것입니다. MongoDB에서 어떤 두 컬렉션은 같은 필드를 가질 필요는 없지만 MySQL에서는 테이블의 모든 행이 같은 컬럼을 공유합니다
* 이 NoSQL 데이터베이스는 성능에 영향을 줄 수있는 JOIN 작업 및 기타 작업을 분배하므로 MongoDB의 성능은 관계형 데이터베이스보다 우수합니다.
* MySQL에 비해 MongoDB의 한 가지 이점은 MongoDB가 빠른 개발과 민첩한 스프린트를 제공한다는 것입니다. 즉, NoSQL 데이터를 미리 준비 할 필요가 없습니다
* RDBMS 데이터베이스는 Atomicity, Consistency, Isolation, Durability를 제공하며 MongoDB 데이터베이스에는 존재하지 않습니다.
* MongoDB는 Map Reduce 기능을 통해보다 쉽게 ​​확장 성을 제공합니다. 이는 개발자가 저가형 하드웨어를 사용해도 완전한 기능을 구현할 수 있음을 의미합니다.
* 관계형 데이터베이스는 응용 프로그램의 유효성을 인증하는 여러보고 도구를 제공하지만 MongoDB와 같은보고 도구는 없습니다.
* MongoDB는 비 관계형 데이터베이스 모델을 제공하므로 DB 설계자는 미완성 DB 모델을 사용하지 않고도 데이터베이스를 쉽게 생성 할 수 있으므로 개발 시간과 비용을 절약 할 수 있습니다.
* MySQL은 거대한 커뮤니티와 광범위한 테스트 및 안정성을 제공하는 매우 확고한 데이터베이스입니다. MongoDB는 아직 개발 단계에 있습니다.


``` MongoDB ```
- https://docs.mongodb.com/manual/tutorial/getting-started/

``` MySQL ```
- https://dev.mysql.com/doc/refman/5.7/en/

