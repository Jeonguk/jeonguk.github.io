---
layout: post
title:  "Lambda expressions in java"
date:   2018-01-20 21:37:10 +0700
categories: [java, language]
---

### Lambda expressions in java

* 람다 식은 Stream API의 데이터에서 파이프 라인 연산을 지원하는 것처럼 멀티 코어 환경의 병렬 처리 기능을 활용합니다.
* 기능 인터페이스에 의해 정의 된 메소드를 구현하는 데 사용되는 익명 메소드 (이름없는 메소드)입니다. 

### Functional interface

* Functional interface 는 오직 하나의 추상 메소드를 포함하는 인터페이스입니다.

* 자바 표준 Runnable 인터페이스의 정의를 살펴보면 run () 메서드를 정의하기 때문에 함수 인터페이스의 정의에 어떻게 부합하는지 알 수 있습니다.

* 아래 코드 샘플에서 computeName 메서드는 암시 적으로 추상화 된 메서드이며 정의 된 유일한 메서드이므로 MyName을 기능적 인터페이스로 만듭니다.

{% highlight java %}
interface MyName{
  String computeName(String str);
}
{% endhighlight %}

### The Arrow Operator

* 람다 식은 새로운 화살표 연산자 인 ->를 Java에 도입합니다. 람다 식은 두 부분으로 나뉩니다.

```
(n) -> n*n
```

* 왼쪽은 표현식에 필요한 매개 변수를 지정하며 매개 변수가 필요하지 않은 경우 비어있을 수도 있습니다.
* 오른쪽은 람다 식의 동작을 지정하는 람다 본문입니다. 
이 연산자에 대해 "될"것으로 생각하는 것이 도움이 될 수 있습니다. 예를 들어, "n은 n * n이됩니다"또는 "n은 n 제곱이됩니다".

* 함수형 인터페이스와 화살표 연산자 개념을 염두에두면 간단한 람다 식을 조합 할 수 있습니다.

{% highlight java %}
interface NumericTest {
	boolean computeTest(int n); 
}

public static void main(String args[]) {
	NumericTest isEven = (n) -> (n % 2) == 0;
	NumericTest isNegative = (n) -> (n < 0);

	// Output: false
	System.out.println(isEven.computeTest(5));

	// Output: true
	System.out.println(isNegative.computeTest(-5));
}
{% endhighlight %}

- Numeric Test

{% highlight java %}
interface MyGreeting {
	String processName(String str);
}

public static void main(String args[]) {
	MyGreeting morningGreeting = (str) -> "Good Morning " + str + "!";
	MyGreeting eveningGreeting = (str) -> "Good Evening " + str + "!";
  
  	// Output: Good Morning Luis! 
	System.out.println(morningGreeting.processName("Luis"));
	
	// Output: Good Evening Jessica!
	System.out.println(eveningGreeting.processName("Jessica"));	
}
{% endhighlight %}

- Greeting Lambda

* 위의 예제에서 morningGreeting 및 eveningGreeting 변수 6 및 7은 MyGreeting 인터페이스에 대한 참조를 만들고 다른 인사말 표현식을 정의합니다.
* 람다 식을 작성할 때 다음과 같이 식의 매개 변수 유형을 명시 적으로 지정할 수도 있습니다.

{% highlight java %}
MyGreeting morningGreeting = (String str) -> "Good Morning " + str + "!";
MyGreeting eveningGreeting = (String str) -> "Good Evening " + str + "!";
{% highlight java %}

### Block Lambda Expressions

* 지금까지 단일 표식 람다 샘플을 다루었습니다. 
화살표 연산자의 오른쪽에있는 코드가 블록 람다 (block lambdas)라고 알려진 둘 이상의 명령문을 포함 할 때 사용되는 다른 유형의 표현식이 있습니다.

{% highlight java %}
interface MyString {
	String myStringFunction(String str);
}

public static void main (String args[]) {
	// Block lambda to reverse string
	MyString reverseStr = (str) -> {
		String result = "";
		
		for(int i = str.length()-1; i >= 0; i--)
			result += str.charAt(i);
		
		return result;
	};

	// Output: omeD adbmaL
	System.out.println(reverseStr.myStringFunction("Lambda Demo")); 
}
{% endhighlight %}

### Generic Functional Interfaces

* 람다 식은 Generic 일 수 없습니다. 
그러나 람다 표현과 관련된 Functional interface는 가능합니다. 하나의 일반적인 interface를 작성하고 다음과 같이 여러 반환 유형을 처리 할 수 ​​있습니다.

{% highlight java %}
interface MyGeneric {
	T compute(T t);
}

public static void main(String args[]){

	// String version of MyGenericInteface
	MyGeneric<String> reverse = (str) -> {
		String result = "";
		
		for(int i = str.length()-1; i >= 0; i--)
			result += str.charAt(i);
		
		return result;
	};

	// Integer version of MyGeneric
	MyGeneric<Integer> factorial = (Integer i) -> {
		int result = 1;
		
		for(int i=1; i <= n; i++)
			result = i * result;
		
		return result;
	};

	// Output: omeD adbmaL
	System.out.println(reverse.compute("Lambda Demo")); 

	// Output: 120
	System.out.println(factorial.compute(5)); 

}

### Lambda Expressions as arguments

* 람다의 한 가지 일반적인 사용은 인수로 전달하는 것입니다.
* 대상 유형을 제공하는 모든 코드에서 사용할 수 있습니다. 
* 메소드에 인수로 실행 가능한 코드를 전달할 수 있기 때문에 이 흥미로운 사실을 알게되었습니다.
* 람다 식을 매개 변수로 전달하려면 기능 인터페이스 유형이 필수 매개 변수와 호환되는지 확인하십시오.

{% highlight java %}
interface MyString {
	String myStringFunction(String str);
}

public static String reverseStr(MyString reverse, String str){
  return reverse.myStringFunction(str);
}

public static void main (String args[]) {
	// Block lambda to reverse string
	MyString reverse = (str) -> {
		String result = "";
		
		for(int i = str.length()-1; i >= 0; i--)
			result += str.charAt(i);
		
		return result;
	};

	// Output: omeD adbmaL
	System.out.println(reverseStr(reverse, "Lambda Demo")); 
}
{% endhighlight %}
