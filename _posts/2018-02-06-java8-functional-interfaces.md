---
layout: post
title:  "java8-functional-interfaces"
date:   2018-02-06 11:46:10 +0700
categories: [java,language]
---

# Functional Interfaces in Java 8

* Java 8에있는 여러 기능적 인터페이스, 일반적인 사용 사례 및 표준 JDK 라이브러리에서의 사용법을 설명합니다.

### Lambdas in Java 8

* Java 8은 람다 식의 형태로 강력하고 새로운 구문 개선을 가져 왔습니다. 람다 (lambda)는 일급 언어 시민으로 취급 될 수있는 익명의 함수입니다 (예 : 메서드로 전달되거나 메서드에서 반환 됨).
* Java 8 이전에는 대개 하나의 기능을 캡슐화해야하는 모든 경우에 대해 클래스를 작성했습니다. 이것은 원시 함수 표현의 역할을하는 무언가를 정의하기 위해 불필요한 상용구 코드를 암시합니다.
* 이 가이드는 java.util.function 패키지에있는 특정 기능 인터페이스에 중점을 둡니다.

### Functional Interfaces

* 모든 functional interface 는 유익한 @FunctionalInterface 어노테이션을 갖도록 권장됩니다. 이는 이 인터페이스의 목적을 명확하게 전달할뿐만 아니라 주석이 달린 인터페이스가 조건을 만족시키지 않으면 컴파일러가 오류를 생성 할 수 있게합니다.
* SAM (Single Abstract Method)이있는 인터페이스는 기능 인터페이스이며 구현은 람다 식으로 처리 될 수 있습니다.
* Java 8의 기본 메소드는 추상적이 아니며 중요하지 않습니다. 기능적 인터페이스에는 여전히 여러 기본 메소드가 있을 수 있습니다. 함수의 설명서를 보면 이 사실을 알 수 있습니다.

### Functions

* 람다의 가장 단순하고 일반적인 경우는 하나의 값을 받아서 다른 값을 반환하는 메소드를 가진 함수 인터페이스입니다. 이 단일 인수의 함수는 인수의 유형과 반환 값에 의해 매개 변수화 된 Function 인터페이스로 표현됩니다.

```java
public interface Function<T, R> { … }
```

* 표준 라이브러리의 Function 유형의 사용법 중 하나는 Map.computeIfAbsent method 에서 값을 반환하지만 key 가 map 에없는 경우 값을 계산하는 Map.computeIfAbsent 메소드입니다. 값을 계산하기 위해 전달 된 Function 구현을 사용합니다.

```java
Map<String, Integer> nameMap = new HashMap<>();
Integer value = nameMap.computeIfAbsent("John", s -> s.length());
```

* 이 경우 값은 키에 함수를 적용하고 map 내부에 넣고 메소드 호출에서 반환하여 계산됩니다. 값을 전달 된 값과 반환 된 값 타입과 일치하는 메소드 참조로 대체 할 수 있습니다.

* 메소드가 호출되는 객체는 실제로 메소드의 암시 적 첫 번째 인수이며, 인스턴스 메소드 길이를 함수 인터페이스로 캐스팅 할 수 있습니다.

```java
Integer value = nameMap.computeIfAbsent("John", String::length);
```

* Function interface 에는 여러 함수를 하나로 결합하여 순차적으로 실행할 수있는 기본 작성 메서드가 있습니다.

```java
Function<Integer, String> intToString = Object::toString;
Function<String, String> quote = s -> "'" + s + "'";
 
Function<Integer, String> quoteIntToString = quote.compose(intToString);
 
assertEquals("'5'", quoteIntToString.apply(5));
```

* quoteIntToString 함수는 intToString 함수의 결과에 적용된 quote 함수의 조합입니다.


### Primitive Function Specializations

* 기본 유형은 제네릭 유형 인수가 될 수 없으므로 가장 많이 사용되는 기본 유형 double, int, long 및 인수 및 리턴 유형에서의 해당 조합에 대한 함수 인터페이스 버전이 있습니다.

  - IntFunction, LongFunction, DoubleFunction : 인수는 지정된 유형이며 반환 유형은 매개 변수화되어 있습니다.
  - ToIntFunction, ToLongFunction, ToDoubleFunction : 반환 유형이 지정된 유형이며 인수가 매개 변수화되어 있습니다.
  - DoubleToIntFunction, DoubleToLongFunction, IntToDoubleFunction, IntToLongFunction, LongToIntFunction, LongToDoubleFunction - 이름으로 지정된대로 기본 유형으로 정의 된 인수와 반환 유형을 모두 갖습니다.

* 예를 들어 짧지 만 바이트를 반환하는 함수에 대해 기본 기능 인터페이스가 없지만 직접 작성하지 못하게하는 기능은 없습니다.

```java
@FunctionalInterface
public interface ShortToByteFunction {
 
    byte applyAsByte(short s);
 
}
```

* 이제 ShortToByteFunction에 의해 정의 된 규칙을 사용하여 short 배열을 byte 배열로 변환하는 메소드를 작성할 수 있습니다.

```java
short[] array = {(short) 1, (short) 2, (short) 3};
byte[] transformedArray = transformArray(array, s -> (byte) (s * 2));
 
byte[] expectedArray = {(byte) 2, (byte) 4, (byte) 6};
assertArrayEquals(expectedArray, transformedArray);
```

### Two-Arity Function Specializations

* 두 개의 인수로 람다를 정의하려면 BiFunction, ToDoubleBiFunction, ToIntBiFunction 및 ToLongBiFunction과 같이 이름에 "Bi"키워드가 포함 된 추가 인터페이스를 사용해야합니다.
* BiFunction에는 인수와 반환 형식이 모두 생성되는 반면 ToDoubleBiFunction 및 기타 함수는 원시 값을 반환 할 수 있습니다.
* 표준 API에서이 인터페이스를 사용하는 일반적인 예 중 하나는 map의 모든 값을 계산 된 값으로 대체 할 수있는 Map.replaceAll 메소드에 있습니다.

* 급여에 대한 새로운 값을 계산하고 반환하기 위해 키와 이전 값을 받는 BiFunction 구현을 사용합시다.

```java
Map<String, Integer> salaries = new HashMap<>();
salaries.put("John", 40000);
salaries.put("Freddy", 30000);
salaries.put("Samuel", 50000);
 
salaries.replaceAll((name, oldValue) -> 
  name.equals("Freddy") ? oldValue : oldValue + 10000);
```

### Suppliers

* Supplier 기능 인터페이스는 인수를 취하지 않는 또 다른 Function specialization 입니다. 일반적으로 lazy 값 생성에 사용됩니다. 예를 들어, double 값을 제곱하는 함수를 정의합시다. 그것은 value 그 자체가 아니라이 value의 Supplier 를 받을 것입니다.

```java
public double squareLazy(Supplier<Double> lazyValue) {
    return Math.pow(lazyValue.get(), 2);
}
```

* 이를 통해 Supplier 구현을 사용하여 이 함수를 호출 할 때 lazy 인수를 생성 할 수 있습니다. 이 인수의 생성에 상당한 시간이 소요될 때 유용 할 수 있습니다. Guava의 sleepUninterruptibly 메소드를 사용하여 시뮬레이션합니다.

```java
Supplier<Double> lazyValue = () -> {
    Uninterruptibles.sleepUninterruptibly(1000, TimeUnit.MILLISECONDS);
    return 9d;
};
 
Double valueSquared = squareLazy(lazyValue);
```

* Supplier 의 또 다른 사용 사례는 시퀀스 생성을위한 논리를 정의하는 것입니다. 이를 증명하기 위해 정적 Stream.generate 메서드를 사용하여 피보나치 수의 스트림을 만듭니다.

```java
int[] fibs = {0, 1};
Stream<Integer> fibonacci = Stream.generate(() -> {
    int result = fibs[1];
    int fib3 = fibs[0] + fibs[1];
    fibs[0] = fibs[1];
    fibs[1] = fib3;
    return result;
});
```

* Stream.generate 메서드에 전달 된 함수는 Supplier functional interface를 구현합니다. Supplier 는 generator 로 유용하기 때문에 일반적으로 일종의 외부 상태가 필요합니다. 이 경우 그 상태는 피보나치의 마지막 두 시퀀스 번호로 구성됩니다.
* 이 상태를 구현하기 위해 람다 내부에서 사용되는 모든 외부 변수를 효과적으로 최종화 해야하기 때문에 두 개의 변수 대신 배열을 사용합니다.
* Supplier functional interface의 다른 specializations 에는 BooleanSupplier, DoubleSupplier, LongSupplier 및 IntSupplier가 있으며 반환 유형은 해당 primitive 입니다.

### Consumers

* Supplier 와는 달리 Consumer 는 포괄적 인 generified argument를 accepts 하고 아무 것도 반환하지 않습니다. side effects를 나타내는 함수입니다.

* 예를 들어, 이름 목록에 있는 모든 사람에게 인사말을 콘솔에 인쇄. List.forEach 메서드에 전달 된 lambda는 Consumer functional interface 를 구현합니다.

```java
List<String> names = Arrays.asList("John", "Freddy", "Samuel");
names.forEach(name -> System.out.println("Hello, " + name));
```

* primitive 값을 인수로받는 DoubleConsumer, IntConsumer 및 LongConsumer의 특수화 된 버전도 있습니다. 더 흥미로운 것은 BiConsumer 인터페이스입니다. 사용 사례 중 하나는 map의 항목을 반복하는 것입니다.

```java
Map<String, Integer> ages = new HashMap<>();
ages.put("John", 25);
ages.put("Freddy", 24);
ages.put("Samuel", 30);
 
ages.forEach((name, age) -> System.out.println(name + " is " + age + " years old"));
```

* specialized BiConsumer 버전의 또 다른 세트는 ObjDoubleConsumer, ObjIntConsumer 및 ObjLongConsumer로 구성되며 두 개의 인수 중 하나가 생성되고 다른 하나는 primitive 유형을 받습니다.

### Predicates

* 수학 논리에서 술어는 값을 받아 boolean 값을 반환하는 함수입니다.

* Predicate functional interface는 생성 된 값을 수신하고 boolean을 리턴하는 함수의 특수화입니다. Predicate 람다의 일반적인 사용 사례는 값 컬렉션을 필터링하는 것입니다.

```java
List<String> names = Arrays.asList("Angela", "Aaron", "Bob", "Claire", "David");
 
List<String> namesWithA = names.stream()
  .filter(name -> name.startsWith("A"))
  .collect(Collectors.toList());
```

* 위 코드에서 스트림 API를 사용하여 목록을 필터링하고 문자 "A"로 시작하는 이름 만 유지합니다. 필터링 로직은 Predicate 구현에 캡슐화됩니다.

* 이전 예와 마찬가지로 이 함수에는 기본 값을받는 IntPredicate, DoublePredicate 및 LongPredicate 버전이 있습니다.

### Operators

* Operator interface 는 동일한 값 유형을 수신하고 리턴하는 함수의 특별한 경우입니다. UnaryOperator 인터페이스는 단일 인수를 받습니다. Collections API의 사용 사례 중 하나는 목록의 모든 값을 동일한 유형의 계산 된 값으로 대체하는 것입니다.

```java
List<String> names = Arrays.asList("bob", "josh", "megan");
 
names.replaceAll(name -> name.toUpperCase());
```

* List.replaceAll 함수는 자리에있는 값을 대체하므로 void를 반환합니다. 목적에 맞추기 위해 목록의 값을 변환하는 데 사용되는 람다는 받은 결과 유형과 동일한 결과 유형을 반환해야합니다. 이 때문에 UnaryOperator가 유용합니다.
* 물론 name -> name.toUpperCase () 대신에 메서드 참조를 간단히 사용할 수 있습니다.

```java
names.replaceAll(String::toUpperCase);
```

* BinaryOperator의 가장 흥미로운 사용 사례 중 하나는 축소 작업 입니다. 모든 값의 합계에 정수 컬렉션을 집계하려 한다고 가정합니다. Stream API를 사용하면 콜렉터를 사용하여이 작업을 수행 할 수 있지만 보다 일반적인 방법은 reduce 메소드를 사용하는 것입니다.

```java
List<Integer> values = Arrays.asList(3, 5, 8, 9, 12);
 
int sum = values.stream()
  .reduce(0, (i1, i2) -> i1 + i2);
```

* reduce 메소드는 초기 누적 값과 BinaryOperator 함수를 수신합니다. 이 함수의 인수는 동일한 유형의 값 쌍이고 함수 자체에는 동일한 유형의 단일 값으로 이들을 조인하기위한 논리가 포함됩니다. 전달 된 함수는 연관성이 있어야합니다. 즉, 값 집계의 순서는 중요하지 않습니다. 즉 다음 조건이 충족되어야합니다.

```java
op.apply(a, op.apply(b, c)) == op.apply(op.apply(a, b), c)
```

* BinaryOperator 연산자 함수의 연관 속성을 사용하면 축소 프로세스를 쉽게 병렬 처리 할 수 ​​있습니다.
* 물론 DoubleUnaryOperator, IntUnaryOperator, LongUnaryOperator, DoubleBinaryOperator, IntBinaryOperator 및 LongBinaryOperator와 같은 기본 값과 함께 사용할 수있는 UnaryOperator 및 BinaryOperator의 특수화도 있습니다.

### Legacy Functional Interfaces

* 모든 Functional Interface가 Java 8에 등장하는 것은 아닙니다. 이전 Java 버전의 많은 인터페이스는 FunctionalInterface의 제한 조건을 준수하며 람다 (lambdas)로 사용될 수 있습니다. 가장 중요한 예는 동시성 API에서 사용되는 Runnable 및 Callable 인터페이스입니다. Java 8에서 이러한 인터페이스는 @FunctionalInterface 주석으로 표시됩니다. 이렇게하면 동시성 코드를 크게 단순화 할 수 있습니다.

```java
Thread thread = new Thread(() -> System.out.println("Hello From Another Thread"));
thread.start();
```



reference
- http://www.baeldung.com/java-8-functional-interfaces