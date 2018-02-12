---
layout: post
title:  "Micro Services Architecture Components"
date:   2018-02-12 14:40:10 +0700
categories: [architecture,msa,java]
---

# Micro Services Architecture Components

``` Netflix Eureka Server ```
* Netflix Eureka는 검색 서비스를 제공하는 Micro Service가 검색 클라이언트로 등록하는 검색 서버의 역할을합니다. Netflix Eureka는 외부와의 통신을 위해 외부에 REST 인터페이스를 제공합니다. Eureka에는 Eureka Client라는 또 다른 소프트웨어 모듈이 있습니다.이를 통해 Eureka Server와 서비스 검색을 교대로 수행합니다. Eureka Client에는 들어있는 클라이언트 요청을로드 밸런스하기 위해 내장 된 Load Balancer도 함께 제공됩니다.

``` Hystrix Server ```
* Hystrix는 소프트웨어 응용 프로그램의 완전한 실패를 방지하는 데 사용되는 내결함성 복원 시스템 역할을합니다. 이는 응용 프로그램이 문제없이 원활하게 실행될 때 회로가 닫힌 상태로 유지되는 일종의 회로 차단기 메커니즘을 제공함으로써 가능합니다. 애플리케이션에 계속해서 오류가 발생하면 Hystrix Server Circuit이 열리 며 Hystrix가 호출 서비스에 대한 추가 요청을 중지하고 대신 요청을 폴백 서비스로 전환합니다. 이 방법으로 매우 탄력적 인 시스템을 제공합니다.

``` Netfilx Zuul Server ```
* Netflix Zuul Server는 일종의 게이트웨이 서버로 작동하며 모든 클라이언트가이를 통과해야하므로 클라이언트에 대한 일종의 통합 인터페이스 역할을합니다. 클라이언트는 단일 통신 프로토콜을 사용하여 모든 마이크로 서비스와 통신하고 Zuul 서버는 다양한 마이크로 서비스를 해당 통신 프로토콜과 함께 호출해야합니다. Netflix Zuul에는 클라이언트로부터 들어오는 모든 요청의로드 균형을 조정하는 내장형 Load Balancer가 있습니다.

``` Netflix Ribbon ```
* Netflix 리본은 클라이언트에서 들어오는 모든 요청을로드 균형 조정하는 일종의 Load Balancer 역할을합니다. 다른 대체로드 균형 메커니즘을 사용하도록 구성 할 수 있지만 기본적으로 기본 라운드 로빈로드 균형 조정 전략을 사용합니다. Netflix Zuul Server에는 내장 된 Netflix 리본이 내장되어 있습니다. Netflix 리본을 독립적으로 사용하려면 Netflix 리본을 적절한 응용 프로그램에서 사용할 수 있도록 적절한 Maven 패키지를 제공해야합니다.

