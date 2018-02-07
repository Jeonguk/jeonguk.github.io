---
layout: post
title:  "Transaction Management in Spring"
date:   2018-02-06 11:46:10 +0700
categories: [spring,java,database]
---

# Transaction Management in Spring

* 트랜잭션은 모든 작업을 실행해야하거나 실행하지 않아야하는 작업 단위입니다. 
* 거래의 중요성을 이해하려면 우리 모두에게 적용되는 예를 생각하십시오. "한 계정에서 다른 계정으로 금액 이전"-이 작업에는 적어도 두 단계 아래에 포함됩니다
  - 발신자의 계좌에서 잔액 차감
  - 금액을 수신자의 계정에 추가
* 이제 금액이 발신자의 계정에서 차감되지만 일부 오류로 인해 수신자 계좌로 전달되지 않는 상황을 생각해보십시오. 이러한 문제는 두 단계 모두가 성공적으로 수행되는 단일 작업 단위로 두 단계가 수행되는 트랜잭션 관리에 의해 관리되거나 실패한 사람이있는 경우 롤백해야합니다.
* 이해하는 데 중요한 4 가지 중요한 용어가 있습니다.
  - Atomic(원자성) - 위에서 설명했듯이 원자성은 트랜잭션 내의 모든 조작이 성공해야하거나 전혀 작동하지 않아야 함을 확인합니다.
  - Consistent(일관성) -이 특성은 트랜잭션이 완료되면 데이터가 일관성있는 상태에 있어야합니다.
  - Isolated(격리) -이 속성을 사용하면 여러 사용자가 동일한 데이터 집합에 액세스 할 수 있으며 각 사용자의 처리는 다른 데이터 집합과 격리되어야합니다.
  - Durable(내구성) - 일단 데이터가 손실되지 않도록 트랜잭션이 완료되면 트랜잭션 결과가 영구적이어야합니다.

## Spring Transaction Management Support
* Spring은 EJB와 비슷한 프로그래밍 방식의 선언적 트랜잭션을 지원.
  - 프로그래밍 방식의 트랜잭션 (Programmatic Transactions) - 프로그래밍 방식의 트랜잭션을 사용하면 모든 것이 성공했을 때 커밋하거나 잘못되었을 때 롤백하는 등의 트랜잭션 관리 코드를 비즈니스 로직과 함께 사용할 수 있습니다.
  - 선언적 트랜잭션 (Declarative Transactions) - 선언적 트랜잭션은 트랜잭션 관리 코드를 비즈니스 논리와 분리합니다. Spring은 트랜잭션 조언 (AOP 사용)을 사용하여 선언적 트랜잭션을 지원한다.

* 선언적 또는 프로그래밍 방식 트랜잭션을 선택하는 것은 편의성 vs 미세 제어입니다. 프로그래밍 방식은 트랜잭션 경계를 정밀하게 제어 할 수있는 반면 선언적 방식은 구성 파일을 사용하여 뛰어난 구성 가능성을 제공합니다.
* 응용 프로그램이 하나의 데이터 소스로 작업하는 경우에만 트랜잭션 객체의 커밋 및 롤백 메소드를 사용하여 트랜잭션을 관리 할 수 ​​있지만 J2EE 서버가 제공하는 트랜잭션 관리에 의존해야하는 여러 데이터 소스간에 트랜잭션을 수행 할 수 있습니다. Spring은 나중의 경우에 사용할 수있는 분산 (XA) 트랜잭션도 지원합니다.

## Choosing Transaction Manager
* 트랜잭션을 관리하는 대신 Spring은 트랜잭션 관리 책임을 플랫폼 특정 구현에 위임하는 여러 트랜잭션 관리자를 지원합니다. Plarform Transaction manager는 모든 트랜잭션 관리자 구현의 부모입니다.
* 트랜잭션 관리자 중 일부는 다음과 같습니다.
  - DataSource Transaction manager - 간단한 JDBC persistence mechanism 을 위해 DataSourceTransactionManager를 사용할 수 있습니다. DataSourceTransactionManager의 샘플 구성은 다음과 같습니다
```
<bean id=”transactionManager” 
    class=”org.springframework.jdbc.datasource.DataSourceTransactionManager>
    <property name=”dataSource” ref= “datasource” />
</bean>
```
  - Hibernate Transaction manager - 응용 프로그램이 Hibernate를 사용할 때 Hibernate 트랜잭션 관리자가 사용되어야한다. HibernateTransactionManager의 샘플 구성은 다음과 같습니다
```
<bean id=”transactionManager” 
        class=”org.springframework.orm.hibernate3.HibernateTransactionManager>
        <property name=”sessionFactory” ref= “sessionFactory” />
</bean>
```
  - Jdo Transaction manager - 아래 구성을 사용하여 Java 데이터 객체 트랜잭션 관리자를 사용.
```
<bean id=”transactionManager” 
    class=”org.springframework.orm.jdo.JdoTransactionManager>
    <property name=”persistanceManagerFactory” ref= “persistanceManagerFactory” />
</bean>
```
  - Jta Transaction manager - Google Transactions가 Java Transactions API 트랜잭션을 사용해야하는 것보다 여러 데이터 소스에 걸쳐있는 경우. 내부적으로 JTA 구현은 트랜잭션 책임을 처리합니다.
  - 아래의 설정을 사용하여 JTA 트랜잭션 관리자를 설정.
```
<bean id=”transactionManager” 
    class=”org.springframework.transaction.jta.JtaTransactionManager>
    <property name=”transactonManagerName” ref= “java:/TransactionManager” />
</bean>
```

## Programmatic Transaction Management
* Spring은 org. springframework.transaction package 에서 PlatformTransactionManager 인터페이스로 알려진 추상 트랜잭션 관리 단위의 도움으로 플랫폼 독립적 인 트랜잭션 관리자 API를 제공합니다.
* 이 인터페이스는 아래의 메소드를 정의합니다.
  - getTransaction(TransactionDefinition) - 이 메소드는 트랜잭션 (활성 트랜잭션 또는 새 트랜잭션 생성)을 리턴합니다.
  - void commit(TransactionStatus) - 이 메소드는 상태를 기반으로 트랜잭션을 커밋합니다.
  - void rollback(TransactionStatus) - 이 메소드는 상태를 기반으로 트랜잭션을 롤백합니다.
* TransactionStatus 인터페이스는 트랜잭션 상태를 가져 오는 여러 가지 메소드를 정의하고 트랜잭션 실행을 제어합니다.




