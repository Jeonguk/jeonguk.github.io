---
layout: post
title:  "Java 8 : Converting Anonymous Classes to Lambda Expressions"
date:   2018-02-14 13:24:10 +0700
categories: [java,language]
---

# Java 8 Converting Anonymous Classes to Lambda Expresssions

* 익명 클래스 (하나의 단일 메서드를 구현하는)를 람다 식으로 리팩터링하면 코드가 더 숙련되고 읽기 쉽습니다. 예를 들어, 다음은 Runnable 및 해당 람다 해당 클래스의 익명 클래스입니다.

```java
// using an anonymous class
Runnable r = new Runnable() {
  @Override
  public void run() {
    System.out.println("Hello");
  }
};

// using a lambda expression
Runnable r2 = () -> System.out.println("Hello");
```

``` Different scoping rules ```

* 익명 클래스와 람다 표현식 사이에는 다른 범위 지정 규칙이 있습니다. 예를 들어, 람다 식에서 this와 super는 어휘 적으로 범위가 지정됩니다. 즉,이 클래스는 둘러싼 클래스와 관련이 있지만 익명 클래스에서는 익명 클래스 자체에 상대적입니다. 마찬가지로 람다 식에서 선언 된 지역 변수는 둘러싸는 클래스에서 선언 된 변수와 충돌하지만 익명의 클래스에서는 둘러싸는 클래스의 변수를 섀도잉 할 수 있습니다. 다음은 그 예입니다.

```java
int foo = 1;
Runnable r = new Runnable() {
  @Override
  public void run() {
    // this is ok!
    int foo = 2;
  }
};

Runnable r2 = () -> {
  // compile error: Lambda expression's local variable foo cannot
  // redeclare another local variable defined in an enclosing scope.
  int foo = 2;
};
```

``` Overloaded methods ```

* 오버로드 된 메서드가있는 경우 람다 식을 사용하면 모호한 메서드 호출이 발생할 수 있으며 명시 적 캐스팅이 필요합니다. 다음은 그 예입니다.

```java
// Functional interface
interface Task {
  public void execute();
}

// Overloaded methods
public static void go(final Runnable r) {
  r.run();
}
public static void go(final Task t) {
  t.execute();
}

// Calling the overloaded method:

// When using an anonymous class, there is no ambiguity because
// the type of the class is explicit at instantiation
go(new Task() {
  @Override
  public void execute() {
     System.out.println("Hello");
  }
});

// When using a lambda expression, there is a compile error!
// The method go(Runnable) is ambiguous
go(() -> {
  System.out.println("Hello");
});

// This ambiguity can be solved with an explicit cast
go((Task)() -> {
  System.out.println("Hello");
});
```

