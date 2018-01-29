---
layout: post
title:  "Java 8 Consumer and Supplier"
date:   2018-01-29 18:44:10 +0700
categories: [java,language]
---

# Java 8 Consumer and Supplier

* 내장 함수 인터페이스 (Consumer <T> 및 Supplier <T>)에 대해 설명
* java.util.function 패키지에 속하는 기능 인터페이스 (즉, 하나의 추상 메소드가있는 인터페이스)


## Consumer

* Consumer <T>는 Java8에서 java.util.function 패키지로 도입 된 내장 함수형 인터페이스입니다. 소비자는 객체를 소비해야하는 모든 상황, 즉 입력으로 취해서 결과를 반환하지 않고 객체에서 일부 작업을 수행해야하는 상황에서 사용할 수 있습니다. 이러한 작업의 일반적인 예는 개체가 인쇄 기능에 대한 입력으로 사용되고 개체의 값이 인쇄되는 인쇄입니다. Consumer는 함수 인터페이스이기 때문에 람다 표현식이나 메소드 참조의 할당 대상으로 사용할 수 있습니다.

- Function Descriptor of Consumer<T>
  - Consumer’s function descriptor 는 T -> ()입니다. 이것은 유형 "T"의 객체가 반환 값없이 람다에 입력됨을 의미합니다.

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}
```

``` Advantage ```
- 객체가 입력으로 수행되고 조작이 수행되는 모든 시나리오에서 내장 함수 인터페이스인 Consumer <T>는 매번 새로운 기능 인터페이스를 정의 할 필요없이 사용할 수 있습니다.

## Supplier

* Supplier <T>는 Java8에서 java.util.function 패키지로 도입 된 내장 함수형 인터페이스입니다. 공급자는 입력이없고 출력이 예상되는 모든 상황에서 사용될 수 있습니다. 공급자는 기능적인 인터페이스이기 때문에 람다 표현식이나 메소드 참조에 대한 할당 대상으로 사용할 수 있습니다.

- Function Descriptor of Supplier<T>
  - Supplier’s Function Descriptor는 T -> ()입니다. 이것은 람다 정의에 입력이없고 리턴 출력이 "T"유형의 객체라는 것을 의미합니다. 

```java
@FunctionalInterface
public interface Supplier<T> {
    /**
     * Gets a result.
     * @return a result
     */
    T get();
}
```

``` Advantage ```
- 연산에 대한 입력이없고 출력을 반환 할 것으로 예상되는 모든 시나리오에서 새 기능 인터페이스를 매번 정의 할 필요없이 내장 기능 인터페이스 인 Supplier <T>를 사용할 수 있습니다.

## Java8 Consumer and Supplier Example


```java
import java.util.function.Consumer;

public class ConsumerTest {

	public static void main(String[] args) {

		System.out.println("E.g. #1 - Java8 Consumer Example\n");

		Consumer<String> consumer = ConsumerTest::printNames;
		consumer.accept("C++");
		consumer.accept("Java");
		consumer.accept("Python");
		consumer.accept("Ruby On Rails");
	}

	private static void printNames(String name) {		
		System.out.println(name);
	}
}
```

- output

```
E.g. #1 - Java8 Consumer Example

C++
Java
Python
Ruby On Rails
```


```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Supplier;

public class SupplierTest {

	public static void main(String[] args) {

		System.out.println("E.g. #2 - Java8 Supplier Example\n");

		List<String> names = new ArrayList<String>();
		names.add("Harry");
		names.add("Daniel");
		names.add("Lucifer");		
		names.add("April O' Neil");

		names.stream().forEach((item)-> {
			printNames(()-> item);
		});
	}

	private static void printNames(Supplier<String> supplier) {
		System.out.println(supplier.get());
	}
}
```

- output

```
E.g. #2 - Java8 Supplier Example

Harry
Daniel
Lucifer
April O' Neil
```


