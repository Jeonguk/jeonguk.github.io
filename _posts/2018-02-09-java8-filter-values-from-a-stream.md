---
layout: post
title:  "Java 8 Filter Null Values from a Stream"
date:   2018-02-09 10:15:10 +0700
categories: [java,language]
---

# Java 8 Filter Null Values from a Stream

* Java Stream은 합계 연산을 지원하는 elements sequence 입니다. 
스트림에서 elements 는 컬렉션, 배열 또는 I / O 리소스와 같은 다른 데이터 소스에서 필요할 때 계산되므로 요소가 저장되지 않습니다.
* 스트림을 사용하면 여러 작업을 연결할 수 있으므로 개발자는 스트림에서 필터링, 매핑, 매칭, 검색, 정렬 또는 축소를 적용 할 수 있습니다.
외부 반복 방식을 사용하는 콜렉션과 달리 스트림은 내부적으로 반복됩니다.

``` Java Stream Filter ```
* java.util.stream.Stream.filter () 메소드는 주어진 predicate 와 일치하는 스트림 요소를 리턴하는 중간 연산입니다.
참고, predicate 는 부울 값을 반환하는 메서드입니다.
Java에서 스트림에 null 값이 있으면 스트림 조작은 NullPointerException을 발생시킵니다.
개발자는 람다 식을 사용하여 스트림에서 null 값을 필터링하거나,

```java
stream.filter(str -> str != null);
```

* 또는 java.util.Objects 클래스가 제공하는 정적 nonNull () 메소드를 사용합니다. 
이 메소드는, 할당 할 수 있었던 참조가 null가 아닌 경우는 true를, 포함되지 않는 경우는 false를 돌려줍니다. 
정적 메소드는 두 가지 방식으로 실행됩니다.

> Using Lambda expression

```java
stream.filter(Objects::nonNull());
```

## Java 8 Filter Null Values from a Stream Example

* 다음은 개발자가 스트림에서 null 요소를 필터링 할 수있는 방법을 보여주는 Java 프로그램입니다
  - start by creating a list of string elements
  - add null values to the list along with some string elements
  - define a boolean condition and we use it to filter out the elements
  - Apply filter to remove the null elements
  - retrieve the filtered stream (having all non-null elements) and we print each element of the new list

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Objects;
import java.util.stream.Collectors;

public class FilterNullValueDemo {

	public static void main(String[] args) {

		List<String> cList = new ArrayList<>();
		cList.add("United States of America");
		cList.add("Ecuador");
		cList.add("Denmark");
		cList.add(null);
		cList.add("Seychelles");
		cList.add("Germany");
		cList.add(null);

		System.out.println("<!-----Original list with null values-----!>");		
		System.out.println(cList + "\n");

		// EXAMPLE #1 = Filter Null Values from a Stream Using 'Lambda Expressions'
		List<String> result = cList.stream().filter(str -> str != null && str.length() > 0).collect(Collectors.toList());
		System.out.println("<!-----Result after null values filtered-----!>");
		System.out.println(result + "\n");

		// EXAMPLE #2 = Filter Null Values from a Stream Using 'Method Reference'
		List<String> nonNullResult = cList.stream().filter(Objects::nonNull).collect(Collectors.toList());
		System.out.println("<!-----Result after null values filtered using nonNull-----!>");
		System.out.println(nonNullResult + "\n");

		// EXAMPLE #3 = Filter Null Values after Map intermediate operation
		List<String> mapNullResult = cList.stream().map(s -> s).filter(str -> str != null && str.length() > 0).collect(Collectors.toList());
		System.out.println("<!-----Result after null values filtered using Map intermediate operation-----!>");
		System.out.println(mapNullResult);
	}
}
```



