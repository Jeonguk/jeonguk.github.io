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

``` comparesEqualTo() ```

* 입력 값의 compareTo () 메서드를 사용하여 입력 일치 프로그램 값을 일치시키려는 Comparable 일치 프로그램을 만듭니다. compareTo () 메소드가 입력 일치 프로그램 값에 대해 0을 리턴하면 일치 프로그램이 일치하고 그렇지 않으면 일치하지 않습니다.

```java
@Test
public void test_comparesEqualTo() throws Exception {
    // Given
    String testValue = "value";

    // Then
    assertThat(testValue, comparesEqualTo("value"));
}
```

``` contains() ```

* 입력 Iterable에 값이 포함되어 있는지 확인하는 데 사용할 수있는 다양한 matchers입니다. 값의 순서는 중요하며 Iterable의 항목 수는 테스트중인 값의 수와 일치해야합니다.

* 입력 목록에 모든 값이 순서대로 포함되어 있습니까?

```java
@Test
public void test_contains_items() throws Exception {
    // Given
    List<String> strings = Arrays.asList("why", "hello", "there");

    // Then
    assertThat(strings, contains("why", "hello", "there"));
}
```

* 입력 목록에 입력 matchers 목록에있는 모든 matchers와 순차적으로 일치하는 항목이 있습니까?

```java
@Test
public void test_contains_list_of_matchers() throws Exception {
    // Given
    List<String> strings = Arrays.asList("why", "hello", "there");

    // Then
    List<org.hamcrest.Matcher<? super String>> matchers = new ArrayList<>();
    matchers.add(startsWith("wh"));
    matchers.add(endsWith("lo"));
    matchers.add(equalTo("there"));
    assertThat(strings, contains(matchers));
}
```

* 입력 목록에 입력 일치 프로그램과 일치하는 항목이 하나만 있습니까?

```java
@Test
public void test_contains_single_matcher() throws Exception {
    // Given
    List<String> strings = Arrays.asList("hello");

    // Then
    assertThat(strings, contains(startsWith("he")));
}
```

* 입력 목록에 입력 vararg matcher의 모든 matcher와 순차적으로 일치하는 항목이 있습니까?

```java
@Test
public void test_contains_matchers() throws Exception {
    // Given
    List<String> strings = Arrays.asList("why", "hello", "there");

    // Then
    assertThat(strings, contains(startsWith("why"), endsWith("llo"), equalTo("there")));
}
```

``` containsInAnyOrder() ```

* 입력 Iterable에 값이 포함되어 있는지 확인하는 데 사용할 수있는 다양한 matchers입니다. 값의 순서는 중요하지 않지만 Iterable의 항목 수는 테스트 할 값의 수와 일치해야합니다.

* 입력 목록에 모든 값이 순서대로 포함되어 있습니까?

```java
@Test
public void test_containsInAnyOrder_items() throws Exception {
    // Given
    List<String> strings = Arrays.asList("why", "hello", "there");

    // Then
    assertThat(strings, containsInAnyOrder("hello", "there", "why"));
}
```

* 입력리스트에 입력 matchers리스트의 모든 matcher와 어느 순서로도 일치하는 항목이 포함되어 있습니까?

```java
@Test
public void test_containsInAnyOrder_list_of_matchers() throws Exception {
    // Given
    List<String> strings = Arrays.asList("why", "hello", "there");

    // Then
    List<org.hamcrest.Matcher<? super String>> matchers = new ArrayList<>();
    matchers.add(equalTo("there"));
    matchers.add(startsWith("wh"));
    matchers.add(endsWith("lo"));		
    assertThat(strings, containsInAnyOrder(matchers));
}
```

* 입력 목록에 입력 vararg matcher의 모든 matcher와 일치하는 항목이 순서대로 포함되어 있습니까?

```java
@Test
public void test_containsInAnyOrder_matchers() throws Exception {
    // Given
    List<String> strings = Arrays.asList("why", "hello", "there");

    // Then
    assertThat(strings, containsInAnyOrder(endsWith("llo"), equalTo("there"), startsWith("why")));
}
```

``` containsString() ```

* 테스트 대상의 String가 지정된 부분 캐릭터 라인을 포함한 경우에 일치하는 Matcher입니다.

```java
@Test
public void test_containsString() throws Exception {
    // Given
    String testValue = "value";

    // Then
    assertThat(testValue, containsString("alu"));
}
```

``` empty() ```

* 입력 컬렉션 인 isEmpty () 메서드가 true를 반환하면 일치하는 Matcher.

```java
@Test
public void test_empty() throws Exception {
    // Given
    Set<String> testCollection = new HashSet<>();

    // Then
    assertThat(testCollection, is(empty()));
}
```

``` emptyArray() ```

* 입력 배열의 길이가 0 인 경우 일치하는 Matcher입니다.

```java
@Test
public void test_emptyArray() throws Exception {
    // Given
    String[] testArray = new String[0];

    // Then
    assertThat(testArray, is(emptyArray()));
}
```

``` emptyCollectionOf() ```

* 입력 컬렉션이 지정된 유형이고 비어있는 경우 일치하는 유형 보증 가해저입니다.

```java
@Test
public void test_emptyCollectionOf() throws Exception {
    // Given
    Set<String> testCollection = new HashSet<>();

    // Then
    assertThat(testCollection, is(emptyCollectionOf(String.class)));
}
```

```  emptyIterable() ```

* 입력 Iterable에 값이없는 경우 일치하는 Matcher입니다.

```java
@Test
public void test_emptyIterable() throws Exception {
    // Given
    Set<String> testCollection = new HashSet<>();

    // Then
    assertThat(testCollection, is(emptyIterable()));
}
```

``` emptyIterableOf() ```

* 입력 가능한 Iterable에 값이없고 지정된 유형 인 경우 일치하는 Typesafe Matcher입니다.

```java
@Test
public void test_emptyIterableOf() throws Exception {
    // Given
    Set<String> testCollection = new HashSet<>();

    // Then
    assertThat(testCollection, is(emptyIterableOf(String.class)));
}
```

``` endsWith() ```

* 입력 문자열이 지정된 하위 문자열로 끝나는 경우 일치하는 Matcher.

```java
@Test
public void test_endsWith() throws Exception {
    // Given
    String testValue = "value";

    // Then
    assertThat(testValue, endsWith("lue"));
}
```

``` equalTo() ```

* 입력 값이 지정된 테스트 값과 논리적으로 동일한 경우에 일치하는 Matcher입니다. 배열의 길이를 검사하고 입력 테스트 배열의 모든 값이 논리적으로 지정된 배열의 값과 같은지 확인하는 경우에도 Array에 사용할 수 있습니다.

* Single value.


```java
@Test
public void test_equalTo_value() throws Exception {
    // Given
    String testValue = "value";

    // Then
    assertThat(testValue, equalTo("value"));
}
```

* Array.

```java
@Test
public void test_equalTo_array() throws Exception {
    // Given
    String[] testValues = { "why", "hello", "there" };

    // Then
    String[] specifiedValues = { "why", "hello", "there" };
    assertThat(testValues, equalTo(specifiedValues));
}
```

``` equalToIgnoringCase() ```

* 대문자를 무시한 상태에서 입력 String 값이 지정된 String과 같은지 여부와 일치하는 Matcher.

```java
@Test
public void test_equalToIgnoringCase() throws Exception {
    // Given
    String testValue = "value";

    // Then
    assertThat(testValue, equalToIgnoringCase("VaLuE"));
}
```

``` equalToIgnoringWhiteSpace() ```

* 불필요한 공백을 무시하면서 입력 String 값이 지정된 String과 같은지 여부와 일치하는 Matcher. 앞과 뒤의 모든 공백은 무시되고 나머지 모든 공백은 단일 공백으로 축소됩니다.

```java
@Test
public void test_equalToIgnoringWhiteSpace() throws Exception {
    // Given
    String testValue = "this    is   my    value    ";

    // Then
    assertThat(testValue, equalToIgnoringWhiteSpace("this is my value"));
}
```

``` eventFrom() ```

* 입력 이벤트 오브젝트가 지정된 Source의 것 인 경우에 일치하는 Matcher. 지정된 하위 유형의 EventObeject도 허용 할 수 있습니다.

```java
@Test
public void test_eventFrom() throws Exception {
    // Given
    Object source = new Object();
    EventObject testEvent = new EventObject(source);

    // Then
    assertThat(testEvent, is(eventFrom(source)));
}
```

* 하위 유형이 지정되었습니다.

```java
@Test
public void test_eventFrom_type() throws Exception {
    // Given
    Object source = new Object();
    EventObject testEvent = new MenuEvent(source);

    // Then
    assertThat(testEvent, is(eventFrom(MenuEvent.class, source)));
}
```

``` greaterThan() ```

* 입력 테스트 값이 지정된 값보다 큰 경우 일치하는 일치 프로그램입니다.

```java
@Test
public void test_greaterThan() throws Exception {
    // Given
    Integer testValue = 5;

    // Then
    assertThat(testValue, is(greaterThan(3)));
}
```

``` greaterThanOrEqual() ```

```java
@Test
public void test_greaterThanOrEqualTo() throws Exception {
    // Given
    Integer testValue = 3;

    // Then
    assertThat(testValue, is(greaterThanOrEqualTo(3)));
}
```

``` hasEntry() ```

* 주어진 map 에 지정된 키와 값 또는 일치하는 항목과 일치하는 항목이 포함되어 있으면 일치하는 일치 연산자입니다.

* Actual Values

```java
@Test
public void test_hasEntry() throws Exception {
    // Given
    Integer testKey = 1;
    String testValue = "one";
    
    Map<Integer, String> testMap = new HashMap<>();
    testMap.put(testKey, testValue);

    // Then
    assertThat(testMap, hasEntry(1, "one"));
}
```

* Matchers

```java
@Test
public void test_hasEntry_matchers() throws Exception {
    // Given
    Integer testKey = 2;
    String testValue = "two";
    
    Map<Integer, String> testMap = new HashMap<>();
    testMap.put(testKey, testValue);

    // Then
    assertThat(testMap, hasEntry(greaterThan(1), endsWith("o")));
}
```

``` hasItem() ```

* 입력 Iterable에 지정된 값 또는 정규 표현식과 일치하는 항목이 하나 이상 있으면 일치하는 일치 연산자입니다.

* Actual Value

```java
@Test
public void test_hasItem() throws Exception {
    // Given
    List<Integer> testList = Arrays.asList(1,2,7,5,4,8);

    // Then
    assertThat(testList, hasItem(4));
}
```

* Matcher

```java
@Test
public void test_hasItem_matcher() throws Exception {
    // Given
    List<Integer> testList = Arrays.asList(1,2,7,5,4,8);

    // Then
    assertThat(testList, hasItem(is(greaterThan(6))));
}
```

``` hasItemInArray() ```

* 입력 배열에 지정된 값 또는 정규 표현식과 일치하는 항목이 하나 이상 있으면 일치하는 일치 연산자입니다.

* Actual Value

```java
@Test
public void test_hasItemInArray() throws Exception {
    // Given
    Integer[] test = {1,2,7,5,4,8};

    // Then
    assertThat(test, hasItemInArray(4));
}
```

* Matcher

```java
@Test
public void test_hasItemInArray_matcher() throws Exception {
    // Given
    Integer[] test = {1,2,7,5,4,8};

    // Then
    assertThat(test, hasItemInArray(is(greaterThan(6))));
}
```

``` hasItems() ```

* 입력 Iterable이 지정된 값 또는 일치하는 모든 것을 순서에 상관하지 않는 경우에 일치하는 Matcher입니다.

* Actual Values

```java
@Test
public void test_hasItems() throws Exception {
    // Given
    List<Integer> testList = Arrays.asList(1,2,7,5,4,8);

    // Then
    assertThat(testList, hasItems(4, 2, 5));
}
```

* Matchers

```java
@Test
public void test_hasItems_matcher() throws Exception {
    // Given
    List<Integer> testList = Arrays.asList(1,2,7,5,4,8);

    // Then
    assertThat(testList, hasItems(greaterThan(6), lessThan(2)));
}
```

``` hasKey() ```

* 입력 Map 이, 지정된 값 또는 정규 표현 엔진에 일치하는 키가 1 개 이상있는 경우에 일치하는 Matcher입니다.

* Actual Value

```java
@Test
public void test_hasKey() throws Exception {
    // Given
    Map<String, String> testMap = new HashMap<>();
    testMap.put("hello", "there");
    testMap.put("how", "are you?");

    // Then
    assertThat(testMap, hasKey("hello"));
}
```

* Matcher

```java
@Test
public void test_hasKey_matcher() throws Exception {
    // Given
    Map<String, String> testMap = new HashMap<>();
    testMap.put("hello", "there");
    testMap.put("how", "are you?");

    // Then
    assertThat(testMap, hasKey(startsWith("h")));
}
```

``` hasProperty() ```

* 입력 Object가 Bean 규칙을 충족하고 지정된 이름의 속성을 갖고 속성의 값이 선택적으로 지정된 일치 자와 일치하는 경우 일치하는 Matcher.

* Property Name

```java
@Test
public void test_hasProperty() throws Exception {
    // Given
    JTextField testBean = new JTextField();
    testBean.setText("Hello, World!");

    // Then
    assertThat(testBean, hasProperty("text"));
}
```

* Property Name and Value Matcher

```java
@Test
public void test_hasProperty_value() throws Exception {
    // Given
    JTextField testBean = new JTextField();
    testBean.setText("Hello, World!");

    // Then
    assertThat(testBean, hasProperty("text", startsWith("H")));
}
```

``` hasSize() ```

* 입력 Collection가 지정된 사이즈를 가지는 경우, 또는 사이즈가 지정된 정규 표현 엔진에 일치하는 경우에 일치하는 Matcher입니다.

* Actual Value

```java
@Test
public void test_hasSize() throws Exception {
    // Given
    List<Integer> testList = Arrays.asList(1,2,3,4,5);

    // Then
    assertThat(testList, hasSize(5));
}
```

* Matcher

```java
@Test
public void test_hasSize_matcher() throws Exception {
    // Given
    List<Integer> testList = Arrays.asList(1,2,3,4,5);

    // Then
    assertThat(testList, hasSize(lessThan(10)));
}
```

``` hasToString() ```

* 입력 Object의 toString () 메서드가 지정된 String 또는 입력 matcher와 일치하는 경우 일치하는 Matcher입니다.

* Atual Value

```java
@Test
public void test_hasToString() throws Exception {
    // Given
    Integer testValue = 4;

    // Then
    assertThat(testValue, hasToString("4"));
}
```

* Matcher

``` java
@Test
public void test_hasToString_matcher() throws Exception {
    // Given
    Double testValue = 3.14;

    // Then
    assertThat(testValue, hasToString(containsString(".")));
}
```

``` hasValue() ```

* 입력 Map에 지정된 값 또는 정규 표현식과 일치하는 값이 하나 이상 있으면 일치하는 일치 연산자입니다.

* Actual Value

```java
@Test
public void test_hasValue() throws Exception {
    // Given
    Map<String, String> testMap = new HashMap<>();
    testMap.put("hello", "there");
    testMap.put("how", "are you?");

    // Then
    assertThat(testMap, hasValue("there"));
}
```

* Matcher

```java
@Test
public void test_hasValue_matcher() throws Exception {
    // Given
    Map<String, String> testMap = new HashMap<>();
    testMap.put("hello", "there");
    testMap.put("how", "are you?");

    // Then
    assertThat(testMap, hasValue(containsString("?")));
}
```

``` hasXPath() ```

* 입력 XML DOM 노드가 입력 XPath 표현식을 충족시키는 경우 일치하는 일치 연산자.

* 노드에 입력 XPath 표현식과 일치하는 노드가 있습니까?

```java
@Test
public void test_hasXPath() throws Exception {
    // Given
    DocumentBuilder docBuilder = DocumentBuilderFactory.newInstance().newDocumentBuilder();
    Node testNode = docBuilder.parse(
            new InputSource(new StringReader("<xml><top><middle><bottom>value</bottom></middle></top></xml>")))
            .getDocumentElement();

    // Then
    assertThat(testNode, hasXPath("/xml/top/middle/bottom"));
}
```

* 노드에 입력 XPath 표현식과 일치하는 노드가 포함되어 있고 해당 노드가 지정된 정규 표현식과 일치하는 값을 가지고 있습니까?

```java
@Test
public void test_hasXPath_matcher() throws Exception {
    // Given
    DocumentBuilder docBuilder = DocumentBuilderFactory.newInstance().newDocumentBuilder();
    Node testNode = docBuilder.parse(
            new InputSource(new StringReader("<xml><top><middle><bottom>value</bottom></middle></top></xml>")))
            .getDocumentElement();

    // Then
    assertThat(testNode, hasXPath("/xml/top/middle/bottom", startsWith("val")));
}
```

* 노드에 입력 XPath 표현식과 일치하는 지정된 네임 스페이스의 노드가 포함되어 있습니까?

```java
@Test
public void test_hasXPath_namespace() throws Exception {
   // Given
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   docFactory.setNamespaceAware(true);
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Node testNode = docBuilder.parse(
           new InputSource(new StringReader("<xml xmlns:prefix='http://namespace-uri'><top><middle><prefix:bottom>value</prefix:bottom></middle></top></xml>")))
           .getDocumentElement();

   NamespaceContext namespace = new NamespaceContext() {
       public String getNamespaceURI(String prefix) {
           return "http://namespace-uri";
       }

       public String getPrefix(String namespaceURI) {
           return null;
       }

       public Iterator<String> getPrefixes(String namespaceURI) {
           return null;
       }
   };

   // Then
   assertThat(testNode, hasXPath("//prefix:bottom", namespace));
}
```

* 노드에 입력 된 XPath 표현식과 일치하고 지정된 정규 표현식과 일치하는 값을 가진 지정된 네임 스페이스에 노드가 포함되어 있습니까?

```java
@Test
public void test_hasXPath_namespace_matcher() throws Exception {
  // Given
  DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
  docFactory.setNamespaceAware(true);
  DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
  Node testNode = docBuilder.parse(
        new InputSource(new StringReader("<xml xmlns:prefix='http://namespace-uri'><top><middle><prefix:bottom>value</prefix:bottom></middle></top></xml>")))
        .getDocumentElement();

  NamespaceContext namespace = new NamespaceContext() {
     public String getNamespaceURI(String prefix) {
        return "http://namespace-uri";
     }

     public String getPrefix(String namespaceURI) {
        return null;
     }

     public Iterator<String> getPrefixes(String namespaceURI) {
        return null;
     }
  };

  // Then
  assertThat(testNode, hasXPath("//prefix:bottom", namespace, startsWith("val")));
}
```

``` instanceOf() ```

* 입력 객체가 지정된 유형인지 여부와 일치하는 Matcher.

```java
@Test
public void test_instanceOf() throws Exception {
  // Given
  Object string = "Hello, World!";

  // Then
  assertThat(string, instanceOf(String.class));
}
```

``` isEmptyOrNullString() ```

* 입력 문자열이 비어 있거나 null 일 때 일치하는 일치 연산자.

```java
@Test
public void test_isEmptyOrNullString() throws Exception {
  // Given
  String emptyString = "";
  String nullString = null;

  // Then
  assertThat(emptyString, isEmptyOrNullString());
  assertThat(nullString, isEmptyOrNullString());
}
```

``` isEmptyString() ```

* 입력 문자열이 비어있을 때 일치하는 Matcher.

```java
@Test
public void test_isEmptyString() throws Exception {
  // Given
  String emptyString = "";

  // Then
  assertThat(emptyString, isEmptyString());
}
```

``` isIn() ```

* 지정된 Collection 또는 Array 내에서 입력 항목이 발견 될 때 일치하는 Matcher.

```java
@Test
public void test_isIn() throws Exception {
  // Given
  Set<Integer> set = new HashSet<>();
  set.add(3);
  set.add(6);
  set.add(4);

  // Then
  assertThat(4, isIn(set));
}
```

``` isOneOf() ```

* 입력 객체가 지정된 객체 중 하나 일 때 일치하는 Matcher.

```java
@Test
public void test_isOneOf() throws Exception {
  // Then
  assertThat(4, isOneOf(3,4,5));
}
```

``` iterableWithSize() ```

* 입력 Iterable이 지정된 크기를 갖거나 지정된 크기 일치 자와 일치 할 때 일치하는 Matcher.

* Actual Value

```java
@Test
public void test_iterableWithSize() throws Exception {
  // Given
  Set<Integer> set = new HashSet<>();
  set.add(3);
  set.add(6);
  set.add(4);

  // Then
  assertThat(set, iterableWithSize(3));
}
```

* Matcher

```java
@Test
public void test_iterableWithSize_matcher() throws Exception {
  // Given
  Set<Integer> set = new HashSet<>();
  set.add(3);
  set.add(6);
  set.add(4);

  // Then
  assertThat(set, iterableWithSize(lessThan(4)));
}
```

``` lessThan() ```

* compareTo 메소드를 사용해, 입력 오브젝트가 지정된 값보다 작은 Comparable 오브젝트에 일치하는 Matcher입니다.

```java
@Test
public void test_lessThan() throws Exception {
  // Then
  assertThat("apple", lessThan("zoo"));
}
```

``` lessThanOrEqualTo() ```

* compareTo 메소드를 사용하여 입력 객체가 지정된 값보다 작거나 같은 Comparable 객체와 일치하는 Matcher.

```java
@Test
public void test_lessThanOrEqualTo() throws Exception {
   // Then
   assertThat(2, lessThanOrEqualTo(2));
}
```

``` not() ```

* 기존의 정규 표현식을 감싸고 그것을 반전시키는 정규 표현식

```java
@Test
public void test_not_matcher() throws Exception {
   // Then
   assertThat("zoo", not(lessThan("apple")));
}
```

* matcher 대신에 값을 사용할 때 not (equalTo (...))의 alias

```java
@Test
public void test_not_value() throws Exception {
   // Then
   assertThat("apple", not("orange"));
}
```

``` notNullValue() ```

* 입력 값이 null가 아닌 경우에 일치하는 Matcher입니다.

```java
@Test
public void test_notNullValue() throws Exception {
   // Then
   assertThat("apple", notNullValue());
}
```

``` nullValue() ```

* 입력 값이 null 인 경우 일치하는 Matcher.

```java
@Test
public void test_nullValue() throws Exception {
   // Given
   Object nothing = null;
  
   // Then
   assertThat(nothing, nullValue());
}
```

``` sameInstance() ```

* 입력 객체가 지정된 값과 동일한 인스턴스 일 때 일치하는 Matcher입니다.

```java
@Test
public void test_sameInstance() throws Exception {
   // Given
   Object one = new Object();
   Object two = one;

   // Then
   assertThat(one, sameInstance(two));
}
```

``` samePropertyValuesAs() ```

* 입력 Bean이 지정된 Bean과 동일한 특성 값을 가질 때 일치하는 Matchet. 즉, 테스트중인 Bean에 특성이 있으면 테스트 조건에 지정된 Bean과 동일한 값을 가져야합니다.

* Given the following Java class:

```java
public class Bean {

    private Integer number;
    private String text;

    public Integer getNumber() {
        return number;
    }

    public void setNumber(Integer number) {
        this.number = number;
    }

    public String getText() {
        return text;
    }

    public void setText(String text) {
        this.text = text;
    }
}
```

* We can write the following test:

```java
@Test
public void test_samePropertyValuesAs() throws Exception {
    // Given
    Bean one = new Bean();
    one.setText("text");
    one.setNumber(3);

    Bean two = new Bean();
    two.setText("text");
    two.setNumber(3);

    // Then
    assertThat(one, samePropertyValuesAs(two));
}
```

``` startsWith() ```

* 입력 문자열이 주어진 접두사로 시작하면 일치하는 Matcher.

```java
@Test
public void test_startsWith() throws Exception {
    // Given
    String test = "Beginnings are important!";

    // Then
    assertThat(test, startsWith("Beginning"));
}
```

``` stringContainsInOrder() ```

* 입력 문자열에 Iterable에서 반환 된 순서대로 지정된 Iterable의 하위 문자열이 포함되어 있으면 일치하는 Matcher.

```java
@Test
public void test_stringContainsInOrder() throws Exception {
    // Given
    String test = "Rule number one: two's company, but three's a crowd!";

    // Then
    assertThat(test, stringContainsInOrder(Arrays.asList("one", "two", "three")));
}
```

``` theInstance() ```

* 입력 객체가 지정된 값과 동일한 인스턴스 일 때 일치하는 Matcher입니다. 'sameInstance ()'와 동일하게 동작합니다.

```java
@Test
public void test_theInstance() throws Exception {
   // Given
   Object one = new Object();
   Object two = one;

   // Then
   assertThat(one, theInstance(two));
}
```

``` typeCompatibleWith() ```

* 입력 유형의 객체를 지정된 기본 유형의 참조에 지정할 수있는 경우 일치하는 일치 자입니다.

```java
@Test
public void test_typeCompatibleWith() throws Exception {
    // Given
    Integer integer = 3;

    // Then
    assertThat(integer.getClass(), typeCompatibleWith(Number.class));
}
```

## Simple Matchers Combining Other Matchers

> 다음 matcher는 주로 다른 matchers를 결합하기 위해 작동합니다.

``` allOf() ```

* 모든 Matchers가 일치 할 때 일치하는 Matcher는 Logical AND처럼 작동합니다. 개별 Matchers 또는 반복자 Matchers를 취할 수 있습니다.

* Individual Matchers

```java
@Test
public void test_allOf_individual() throws Exception {
    // Given
    String test = "starting off well, gives content meaning, in the end";

    // Then
    assertThat(test, allOf(startsWith("start"), containsString("content"), endsWith("end")));
}
```

* Iterable of Matchers

```java
@Test
public void test_allOf_iterable() throws Exception {
    // Given
    String test = "Hello, world!";

    List<Matcher<? super String>> matchers = Arrays.asList(containsString("world"), startsWith("Hello"));

    // Then
    assertThat(test, allOf(matchers));
}
```

``` anyOf() ```

* 입력 Matchers가 일치 할 때 일치하는 Matcher는 Logical OR처럼 작동합니다. 개별 Matchers 또는 반복자 Matchers를 취할 수 있습니다.

* Individual Matchers

```java
@Test
public void test_anyOf_individual() throws Exception {
    // Given
    String test = "Some things are present, some things are not!";

    // Then
    assertThat(test, anyOf(containsString("present"), containsString("missing")));
}
```

* Iterable of Matchers

```java
@Test
public void test_anyOf_iterable() throws Exception {
    // Given
    String test = "Hello, world!";

    List<Matcher<? super String>> matchers = Arrays.asList(containsString("Hello"), containsString("Earth"));

    // Then
    assertThat(test, anyOf(matchers));
}
```

``` array() ```

* 입력 배열의 요소가 지정된 Matchers를 사용하여 개별적으로 일치 할 때 일치하는 Matcher. Matchers 수는 배열의 크기와 같아야합니다.

```java
@Test
public void test_array() throws Exception {
    // Given
    String[] test = {"To be", "or not to be", "that is the question!"};

    // Then
    assertThat(test, array(startsWith("To"), containsString("not"), instanceOf(String.class)));
}
```

``` both() ```

* Matcher. 결합 가능한 matcher와 조합 해 사용되는 경우, 지정된 Matcher가 양쪽 모두 일치했을 때에 일치합니다.

```java
@Test
public void test_both() throws Exception {
    // Given
    String test = "Hello, world!";

    // Then
    assertThat(test, both(startsWith("Hello")).and(endsWith("world!")));
}
```

``` either() ```

* 지정된 조합이 일치하면 조합 가능한 matcher .or ()와 조합하여 사용되는 Matcher가 일치합니다.

```java
@Test
public void test_either() throws Exception {
    // Given
    String test = "Hello, world!";

    // Then
    assertThat(test, either(startsWith("Hello")).or(endsWith("universe!")));
}
```

``` is() ```

* 입력 매처가 일치 할 때 일치하는 Matcher는 편의를 위해 간단히 사용되며 어설 션을 영어와 같이 더 많이 읽습니다.

```java
@Test
public void test_is_matcher() throws Exception {
    // Given
    Integer test = 5;

    // Then
    assertThat(test, is(greaterThan(3)));
}
```

* 또한 not (...)와 not (equalTo (...))와 비슷한 is (equalTo (...))의 별칭으로 사용됩니다.

```java
@Test
public void test_is_value() throws Exception {
    // Given
    Integer test = 5;

    // Then
    assertThat(test, is(5));
}
```

``` describedAs() ```

* Matcher 다른 정규 표현 엔진의 실패 출력을 오버라이드 (override)하기 위해서 사용합니다. 사용자 정의 오류 출력이 필요할 때 사용됩니다. 인수는 실패 메시지, 원본 Matcher 및 자리 표시 자 % 0, % 1, % 2을 사용하여 실패 메시지로 포맷 될 모든 값입니다.

```java
@Test
public void test_describedAs() throws Exception {
    // Given
    Integer actual = 7;
    Integer expected = 10;

    // Then
    assertThat(actual, describedAs("input > %0", greaterThan(expected), expected));
}
```
