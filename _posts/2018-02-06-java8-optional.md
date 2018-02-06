---
layout: post
title:  "java8-optional"
date:   2018-02-06 13:53:10 +0700
categories: [java,language]
---

# Java 8 optional

### What is Optional in Java 8?
* Java 8에는 값이 존재하거나 존재하지 않을 수있는 상황을 처리 할 수있는 방법을 제공하는 새로운 java.util.Optional 클래스가 추가되었습니다.
* Optional 은 null 이 아닌 값을 포함 할 수도 있고 포함하지 않을 수도있는 컨테이너 오브젝트입니다.

### Why use Optional in Java ?
* 일반적으로 변수에 null 값을 할당하여 변수에 값이 없음을 나타냅니다. 그러나 역 참조로 값을 찾으려는 경우 NullPointerException이 자주 발생합니다.
이를 방지하기 위해 일반적으로 프로그램에서 변수를 사용하기 전에 변수가 비어 있지 않은지 확인하기 위해 빈번하게 NULL 검사를 코드에 추가합니다.
Optional은 그러한 상황을 다루기위한 더 나은 접근법을 제공합니다.

### How to create an Optional object in Java?
* Optional class스는 생성자를 정의하지 않습니다. 대신 다음 방법 중 하나를 사용하여 인스턴스를 만들 수 있습니다.

``` Optional.empty ```
```java
Optional<String> emptyString = Optional.empty();
```

``` Optional.ofNullable ```
* Optional.ofNullable () 메서드를 사용하면 null을 허용 할 수있는 선택적 요소를 만들 수 있습니다.
```java
Integer x = null;
Optional<Integer> optional = Optional.ofNullable(x);
```

``` Optional.of ```
* 다음과 같이 Optional.of ()를 사용하여 지정된 값으로 옵션을 만들 수 있습니다.
```java
Integer y = null;
Optional<Integer> optional = Optional.of(y);
```

## Accessing values from an Optional object

``` isPresent() and get() ```
* Optional 인스턴스에있는 값을 얻기 위해 get () 메서드를 호출 할 수 있습니다. 그러나 값에 값이 포함되어 있지 않으면 메서드는 NoSuchElementException을 throw합니다. 따라서 먼저 isPresent () 메소드를 사용하여 값이 존재하는지 확인한 다음 값이 사용 가능하면 get () 메소드를 호출해야합니다.
```java
Optional<String> emptyString = Optional.empty();

if(emptyString.isPresent())
    System.out.println(emptyString.get());
else
    System.out.println("emptyString has no value");
```
* 여기에는 값에 액세스하는 두 가지 메소드를 호출하는 오버 헤드가 있습니다. 대신 orElse (), orElseGet () 또는 orElseThrow ()와 같은 메소드를 사용할 수 있습니다.

``` orElse(defaultValue) ```
* 이 메서드는 값이 있으면 반환하고 그렇지 않으면 매개 변수로 제공된 기본값을 반환합니다.
```java
Optional<String> emptyString = Optional.ofNullable(null);

System.out.println(emptyString.orElse("Empty String"));
```
> Output:

> Empty String

``` orElseGet(getFunc) ```
* 이 메소드는 존재하는 경우는 값을 돌려주고, 그렇지 않은 경우는 getFunc로부터 취득한 값을 돌려줍니다.
```java
Optional<Employee> optionalEmp = Optional.empty();

Supplier<Employee> defaultEmp = new Supplier<Employee>() {

    @Override
    public Employee get() {
    Employee emp = new Employee();
    emp.setName("John Doe");
    return emp;
    }
};
```
> Output:

> John Doe

``` orElseThrow(excFunc) ```
* 이 메서드는 값이 있으면 반환하고 그렇지 않으면 excFunc에서 생성 된 예외를 throw합니다.
```java
Optional<Employee> optionalEmp = Optional.empty();
optionalEmp.orElseThrow(NoSuchElementException::new);
```
> Output:
 
> Exception in thread “main” java.util.NoSuchElementException


*  런타임에 Null 포인터 예외를 방지 할 수 있습니다.
* JDK 8은 또한 각각 Double, int 및 Long 값을 위해 특별히 설계된 OptionalLong, OptionalInt 및 OptionalLong을 제공합니다.
