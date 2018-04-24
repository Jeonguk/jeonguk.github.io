---
layout: post
title:  "measure-elapsed-time-in-java"
date:   2018-04-24 17:42:10 +0700
categories: [java,language]
---

# Measure Elapsed Time in Java

---

### ava에서 경과 시간을 측정하는 방법, 표준 Java 클래스와 경과 시간을 측정하는 기능을 제공하는 외부 패키지

### Simple Measurements

---

- currentTimeMillis()

```
long start = System.currentTimeMillis();
// ...
long finish = System.currentTimeMillis();
long timeElapsed = finish - start;
```

> System.currentTimeMillis ()가 월 클럭 시간을 측정하므로 결과가 정확하지 않을 수 있습니다. 월 시계 시간은 여러 가지 이유로 변경 될 수 있습니다. 시스템 시간을 변경하면 결과에 영향을 줄 수 있거나 윤초가 결과에 영향을 미칩니다.

- nanoTime()

```
long start = System.nanoTime();
// ...
long finish = System.nanoTime();
long timeElapsed = finish - start;
```

> currentTimeMillis () 대신 nanoTime () 타임 스탬프를 가져 오는 데 사용되는 메서드. nanoTime ()은 분명히 시간을 나노초 단위로 반환합니다. 따라서 경과 시간이 다른 시간 단위로 측정되는 경우 적절하게 변환해야합니다.
예를 들어 밀리 초로 변환하려면 결과를 나노초 단위로 1.000.000으로 나눠야합니다.
nanoTime ()의 또 다른 함정은 나노초 정밀도를 제공하더라도 나노 초 해상도 (즉, 값이 얼마나 자주 업데이트되는지)를 보장하지 않는다는 것입니다.
그러나 해상도가 적어도 currentTimeMillis ()보다 우수하다는 것을 보장합니다.

### Java 8

---

- Instant Class

```
Instant start = Instant.now();
// CODE HERE        
Instant finish = Instant.now();
long timeElapsed = Duration.between(start, finish).toMillis();
```

- StopWatch

``` Maven Dependency ```

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.7</version>
</dependency>
```

-  Measuring Elapsed Time with StopWatch

> 먼저 클래스의 인스턴스를 가져와야 만 경과 시간을 간단히 측정 할 수 있습니다.
```
StopWatch watch = new StopWatch();
watch.start();
```
> 감시가 실행되면 벤치 마크 할 코드를 실행 한 다음 끝에 stop () 메소드를 호출하기 만하면됩니다. 마지막으로 실제 결과를 얻기 위해 getTime ()을 호출합니다.

```
watch.stop();
System.out.println("Time Elapsed: " + watch.getTime()); // Prints: Time Elapsed: 2501
```

> StopWatch에는 측정을 일시 중지하거나 다시 시작하는 데 사용할 수있는 몇 가지 추가 도우미 메서드가 있습니다. 벤치 마크를 좀 더 복잡하게 만들 필요가있을 때 도움이 될 수 있습니다.
마지막으로 클래스가 스레드로부터 안전하지 않다는 점을 유의하십시오.

