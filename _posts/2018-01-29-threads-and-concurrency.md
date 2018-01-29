---
layout: post
title:  "Threads and Concurrency"
date:   2018-01-29 17:36:10 +0700
categories: [java,language]
---

# Threads and Concurrency

* 동시성은 (Concurrency) 여러 계산을 동시에 실행하는 프로그램의 기능입니다. 이것은 컴퓨터의 사용 가능한 CPU 코어 또는 동일한 네트워크 내의 다른 컴퓨터를 통해 계산을 분산시킴으로써 얻을 수 있습니다.
* 병렬 실행을보다 잘 이해하려면 프로세스와 스레드를 구별해야합니다. 프로세스는 운영 체제가 제공하는 실행 환경으로, 자체 리소스 집합 (예 : 메모리, 열린 파일 등)을 가지고 있습니다. 대조적으로 쓰레드는 프로세스 내에 존재하는 프로세스이며 프로세스의 다른 스레드와 리소스 (메모리, 열린 파일 등)를 공유합니다.
* 서로 다른 스레드간에 자원을 공유하는 기능은 성능이 중요한 요구 사항 인 작업에 보다 적합한 스레드를 만듭니다. 비록 성능상의 이유 때문에 동일한 기계 또는 동일한 네트워크 내의 다른 기계에서 실행중인 다른 프로세스간에 프로세스 간 통신을 구축 할 수는 있지만 스레드는 종종 단일 시스템에서 계산을 병렬화하도록 선택됩니다.
* Java에서 프로세스는 실행중인 Java Virtual Machine (JVM)에 해당하는 반면, 스레드는 동일한 JVM에 있으며 실행 중에 Java 응용 프로그램에 의해 동적으로 작성 및 중지 될 수 있습니다. 각 프로그램에는 적어도 하나의 스레드 인 주(main) 스레드가 있습니다. 이 메인 쓰레드는 각 자바 애플리케이션이 시작될 때 생성되며, 프로그램의 main () 메소드를 호출하는 것이다. 이 시점부터 자바 애플리케이션은 새로운 스레드를 생성하고 함께 사용할 수 있습니다.


> 현재 스레드에 대한 액세스는 JDK 클래스 java.lang.Thread의 정적 메소드 currentThread ()에 의해 제공됩니다.

```java
public class MainThread {
	
	public static void main(String[] args) {
		long id = Thread.currentThread().getId();
		String name = Thread.currentThread().getName();
		int priority = Thread.currentThread().getPriority();
		State state = Thread.currentThread().getState();
		String threadGroupName = Thread.currentThread().getThreadGroup().getName();
		System.out.println("id="+id+"; name="+name+"; priority="+priority+"; state="+state+"; threadGroupName="+threadGroupName);
    }
    
}
```

* 이 간단한 응용 프로그램의 소스 코드에서 알 수 있듯이 main () 메서드에서 직접 현재 스레드에 액세스하고 제공된 스레드에 대한 정보를 출력합니다.

```
id=1; name=main; priority=5; state=RUNNABLE; threadGroupName=main
```

* 출력 결과는 각 스레드에 대한 흥미로운 정보를 보여줍니다. 각 스레드에는 JVM에서 고유 한 식별자가 있습니다. 스레드의 이름은 실행중인 JVM (예 : 디버거 또는 JConsole 도구)을 모니터링하는 외부 응용 프로그램 내의 특정 스레드를 찾는 데 도움이됩니다. 둘 이상의 스레드가 실행될 때 우선 순위는 다음에 실행할 작업을 결정합니다.

* 스레드에 대한 진실은 모든 스레드가 실제로 동시에 실행되는 것은 아니지만 각 CPU 코어의 실행 시간이 작은 조각으로 나누어지고 다음 시간 조각이 우선 순위가 가장 높은 다음 대기중인 스레드에 할당된다는 것입니다. JVM의 스케줄러는 스레드가 다음에 실행할 스레드의 우선 순위에 따라 결정합니다.

* 우선 순위 옆에있는 스레드의 상태도 다음 중 하나 일 수 있습니다.

  - NEW : 아직 시작하지 않은 스레드는이 상태입니다.
  - RUNNABLE: Java 가상 머신으로 실행 중의 thread가이 상태에 있습니다.
  - BLOCKED: 모니터 잠금을 기다리는 동안 차단 된 스레드는이 상태입니다.
  - WAITING: 다른 스레드가 특정 작업을 수행하기 위해 무기한 대기중인 스레드가이 상태입니다.
  - TIMED_WAITING: 지정된 대기 시간까지 다른 스레드가 작업을 수행하기를 기다리는 스레드는이 상태입니다.
  - TERMINATED: 종료 한 스레드는이 상태입니다.


