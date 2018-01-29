---
layout: post
title:  "Hamcrest matchers tutorial"
date:   2018-01-29 11:09:10 +0700
categories: [java,testing]
---

# Hamcrest matchers

## ``` Hamcrest ```

* Hamcrest는 matcher 객체를 만들기위한 프레임 워크입니다. 이러한 정규 표현식 객체는 predicates 이며 특정 조건에서 만족 될 수있는 규칙을 작성하는 데 사용됩니다. 이들은 자동화 된 테스트에서 가장 자주 사용되지만 데이터 유효성 검사와 같은 다른 시나리오에서도 사용할 수 있습니다. Hamcrest는 간단한 JUnit asserts 이상의 단계를 밟아 우리가 매우 구체적이고 판독 가능한 검증 코드를 만들 수있게 해줍니다.

* Hamcrest는 테스트를 매우 쉽게 읽을 수 있도록 고안되었습니다. 정적 메서드를 자유롭게 사용하여 작성하고 이해하기 쉬운 assertion grammar 를 만듭니다. JUnit 및 Mockito와 함께 사용하면 '한 가지를 테스트하는'좋은 단위 테스트의 속성을 충족하는 명확하고 간결한 테스트를 작성할 수 있습니다.

* 호출 된 String이 있고 이것을 테스트하기 위해 JUnit이 선언 할 수있는 다른 예상되는 문자열과 같음을 테스트하려고합니다.

```java
assertEquals(expected, actual);
```

* Hamcrest에서는 JUnit assertThat (valueUnderTest, matcher) 메소드를 사용합니다. 이 방법은 항상 Hamcrest assert 기초를 형성합니다. 우리는 테스트중인 값이 정규 표현식을 만족한다고 생각합니다. hamcrest에서 위의 테스트를 다시 작성하려면 다음과 같이 작성할 수 있습니다.

```java
assertThat(actual, equalTo(expected));
```

* Hamcrest는 matchers가 실패 할 때 자세한 결과를 생성하여 예상 값과 실제 값을 지정하여 테스트가 실패해야하는 이유를 파악하는 데 도움을줍니다. 다음 테스트 사례를 살펴보십시오.

```java
@Test
public void test_failed() throws Exception {
    // Given
    Integer number = 7;

    // Then
    assertThat(number, greaterThan(10));
}
```

* 분명히 이 테스트는 실패하지만 Hamcrest는 실패에 대한 자세한 정보를 제공합니다.

```java
java.lang.AssertionError: 
Expected: a value greater than <10>
     but: <7> was less than <10>
```

# Example usage
## Meet the Matchers

```
import static org.hamcrest.Matchers.*
```

### Simple Matchers

``` any() ```

* 지정된 유형의 모든 변수와 일치합니다.

```java
@Test
public void test_any() throws Exception {
    // Given
    String myString = "hello";
    
    // Then
    assertThat(myString, is(any(String.class)));		
}
```

``` anything() ```

```java
@Test
public void test_anything() throws Exception {
    // Given
    String myString = "hello";
    Integer four = 4;
    
    // Then
    assertThat(myString, is(anything()));
    assertThat(four, is(anything()));
}
```

``` arrayContaining() ```

* 
배열의 다양한 matchers, 배열의 길이는 matchers의 수와 일치해야하며, 순서는 중요합니다. 배열에는 정규 표현 엔진에 입력 된 순서대로 모든 항목이 포함되어 있습니까?

```java
@Test
public void test_arrayContaining_items() throws Exception {
    // Given
    String[] strings = {"why", "hello", "there"};
    
    // Then
    assertThat(strings, is(arrayContaining("why", "hello", "there")));
}
```

* 배열에 matcher의 입력리스트와 순서가 일치하는 항목이 있습니까?

```java
@Test
public void test_arrayContaining_list_of_matchers() throws Exception {
    // Given
    String[] strings = {"why", "hello", "there"};
    
    // Then
    java.util.List<org.hamcrest.Matcher<? super String>> itemMatchers = new ArrayList<>();
    itemMatchers.add(equalTo("why"));
    itemMatchers.add(equalTo("hello"));
    itemMatchers.add(endsWith("here"));
    assertThat(strings, is(arrayContaining(itemMatchers)));
}
```

* 배열에 입력 vararg matcher와 일치하는 항목이 순서대로 포함되어 있습니까?

```java
@Test
public void test_arrayContaining_matchers() throws Exception {
    // Given
    String[] strings = {"why", "hello", "there"};
    
    // Then
    assertThat(strings, is(arrayContaining(startsWith("wh"), equalTo("hello"), endsWith("here"))));
}
```

``` arrayContainingInAnyOrder() ```

* 배열에 대한 다양한 matchers, 배열의 길이는 matchers의 수와 일치해야하지만 순서는 중요하지 않습니다.
* 배열에 주어진 모든 항목이 포함되어 있습니까?

```java
@Test
public void test_arrayContainingInAnyOrder_items() throws Exception {
    // Given
    String[] strings = { "why", "hello", "there" };

    // Then
    assertThat(strings, is(arrayContainingInAnyOrder("hello", "there", "why")));
}
```

* 배열에 Matchers의 입력 컬렉션과 일치하는 항목이 포함되어 있습니까?

```java
@Test
public void test_arrayContainingInAnyOrder_collection_of_matchers() throws Exception {
    // Given
    String[] strings = { "why", "hello", "there" };

    // Then
    Set<org.hamcrest.Matcher<? super String>> itemMatchers = new HashSet<>();
    itemMatchers.add(equalTo("hello"));
    itemMatchers.add(equalTo("why"));
    itemMatchers.add(endsWith("here"));
    assertThat(strings, is(arrayContainingInAnyOrder(itemMatchers)));
}
```

* 배열에 입력 vararg matcher와 일치하는 항목이 포함되어 있습니까?

```java
@Test
public void test_arrayContainingInAnyOrder_matchers() throws Exception {
    // Given
    String[] strings = { "why", "hello", "there" };

    // Then
    assertThat(strings, is(arrayContainingInAnyOrder(endsWith("lo"), startsWith("the"), equalTo("why"))));
}
```

``` arrayWithSize() ```

* 배열이 특정 길이인지 확인하는 다양한 matchers.
* 입력 배열의 길이가 정확히 지정 되었습니까?

```java
@Test
public void test_arrayWithSize_exact() throws Exception {
    // Given
    String[] strings = { "why", "hello", "there" };

    // Then
    assertThat(strings, is(arrayWithSize(3)));
}
```

* 입력 배열의 길이가 지정된 길이와 일치합니까?

```java
@Test
public void test_arrayWithSize_matcher() throws Exception {
    // Given
    String[] strings = { "why", "hello", "there" };

    // Then
    assertThat(strings, is(arrayWithSize(greaterThan(2))));
}
```

``` closeTo() ```

* 값이 지정된 값의 예상 오차 범위 내에 있는지 확인하기 위해 Double 또는 BigDecimal과 함께 사용할 수있는 Matcher입니다.

* Double

```java
@Test
public void test_closeTo_double() throws Exception {
    // Given
    Double testValue = 6.3;

    // Then
    assertThat(testValue, is(closeTo(6, 0.5)));
}
```

* BigDecimal

```java
@Test
public void test_closeTo_bigDecimal() throws Exception {
    // Given
    BigDecimal testValue = new BigDecimal(324.0);

    // Then
    assertThat(testValue, is(closeTo(new BigDecimal(350), new BigDecimal(50))));
}
```
