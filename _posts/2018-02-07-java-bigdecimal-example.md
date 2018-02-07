---
layout: post
title:  "java-bigdecimal-example"
date:   2018-02-07 11:43:10 +0700
categories: [java,language]
---

# Java BigDecimal

* BigDecimal는 불변의, 임의 정밀도의 부호 첨부 10 진수입니다. java.lang.Math 클래스의 연산을 수행하며, 정밀도, 반올림, 마지막 위치 (ULP), 엔지니어링 표기법 값 등을 가져 오는 데 사용됩니다. BigDecimal을 인스턴스화하고 다음과 같이 값에 액세스 할 수 있습니다.

```java
BigDecimal b = new BigDecimal("215.87");
b.intValue(); 
```
* initialize BigDecimal using MathContext.

```java
MathContext mc = new MathContext(2, RoundingMode.CEILING);
BigDecimal b = new BigDecimal("215.87", mc); 
```

``` BigDecimal add() ```
- add() method adds two BigDecimal instances. 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class AddDemo {
	public static void main(String[] args) {
		BigDecimal b = new BigDecimal("234.43");
		BigDecimal bres = b.add(new BigDecimal("450.23"));
		System.out.println("Add: " + bres);
		//Using MathContext
		MathContext mc = new MathContext(2, RoundingMode.DOWN);		
		BigDecimal bdecMath = b.add(new BigDecimal("450.23"), mc);
		System.out.println("Add using MathContext: " + bdecMath);
	}
} 
```
- Output
```
Add: 684.66
Add using MathContext: 6.8E+2 
```

``` BigDecimal subtract() ```
- subtract() method subtracts the given BigDecimal from calling BigDecimal instance.
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class SubtractDemo {
	public static void main(String[] args) {
		BigDecimal b = new BigDecimal("50.345");
		BigDecimal bdec = b.subtract(new BigDecimal("34.765"));
		System.out.println("Subtract: " + bdec);
		//Using MathContext
		MathContext mc = new MathContext(2, RoundingMode.FLOOR);
		BigDecimal bdecMath = b.subtract(new BigDecimal("34.765"), mc);
		System.out.println("Subtract using MathContext: " + bdecMath);		
	}
} 
```
- Output
```
Subtract: 15.580
Subtract using MathContext: 15 
```

``` BigDecimal multiply() ```
- multiply() multiplies two BigDecimal instances. 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class MultiplyDemo {
	public static void main(String[] args) {
		BigDecimal bdec = new BigDecimal("50.23");
		BigDecimal bdecRes = bdec.multiply(new BigDecimal("30.44"));
		System.out.println("Multiply:" + bdecRes);
		//Using MathContext
		MathContext mc = new MathContext(2, RoundingMode.DOWN);
		BigDecimal bdecMath = bdec.multiply(new BigDecimal("30.44"), mc);
		System.out.println("Multiply using MathContext: " + bdecMath);		
	}
} 
```
- Output
```
Multiply:1529.0012
Multiply using MathContext: 1.5E+3 
```

``` BigDecimal divide() ```
- divide() method divides the calling BigDecimal by given BigDecimal instance. According to java doc "if the exact quotient cannot be represented (because it has a non-terminating decimal expansion) an ArithmeticException is thrown". 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class DivideDemo {
	public static void main(String[] args) {
		BigDecimal bdec = new BigDecimal("706");
		BigDecimal bdecRes = bdec.divide(new BigDecimal("20"));
		System.out.println("Divide: " + bdecRes);
		//Using MathContext
		MathContext mc = new MathContext(2, RoundingMode.FLOOR);	
		BigDecimal bdecMath = bdec.divide(new BigDecimal("20"), mc);
		System.out.println("Divide using MathContext: " + bdecMath);		
	}
} 
```
- Output
```
Divide: 35.3
Divide using MathContext: 35
```

``` BigDecimal divideToIntegralValue() ```
- It returns a BigDecimal whose value will be integer part of a/b. 
```java
import java.math.BigDecimal;
public class DivideToIntegralValueDemo {
	public static void main(String[] args) {
		BigDecimal bdec = new BigDecimal("706");
		BigDecimal bdecRes = bdec.divideToIntegralValue(new BigDecimal("20"));
		System.out.println("DivideToIntegralValue: " + bdecRes);
	}
} 
```
- Output
```
DivideToIntegralValue: 35 
```

``` BigDecimal remainder() ```
- It returns the BigDecimal as remainder of the operation a % b. We can also pass MathContext as an argument to get rounded result. 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class RemainderDemo {
	public static void main(String[] args) {
		BigDecimal bdec = new BigDecimal("700.588");
		MathContext mc = new MathContext(2, RoundingMode.CEILING);
		BigDecimal bdecMath = bdec.remainder(new BigDecimal("21.46"), mc);
		System.out.println("a % b using MathContext : " + bdecMath);
	}
} 
```
- Output
```
a % b using MathContext : 13.868 
```


``` BigDecimal divideAndRemainder() ```
- It returns an array of BigDecimal of the result of divide() and remainder() method for the two BigDecimal instances. 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class DivideAndRemainderDemo {
	public static void main(String[] args) {
		MathContext mc = new MathContext(2, RoundingMode.FLOOR);
		BigDecimal bdec = new BigDecimal("123.56");
		BigDecimal[] bdecRes = bdec.divideAndRemainder(new BigDecimal("23.43"), mc);
		System.out.println("a/b: " + bdecRes[0]);
		System.out.println("a % b: " + bdecRes[1]);
	}
} 
```
- Output
```
a/b: 5
a % b: 6.41 
```

``` BigDecimal pow() ```
- It calculates the power of a BigDecimal value and returns a BigDecimal. The exponent will be an integer. 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class PowDemo {
	public static void main(String[] args) {
		int exponent = 7;
		MathContext mc = new MathContext(2, RoundingMode.CEILING);
		BigDecimal bdec = new BigDecimal("12");
		BigDecimal bdecres = bdec.pow(exponent, mc);
		System.out.println("Pow: " + bdecres);
	}
} 
```
- Output
```
Pow: 3.6E+7 
```

``` BigDecimal abs() ```
- It returns a BigDecimal with absolute value of a given BigDecimal. We can also pass MathContext as an argument that will return result with rounding. 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class AbsDemo {
	public static void main(String[] args) {
		BigDecimal b1 = new BigDecimal("-234.54");
		System.out.println(b1.abs());
		BigDecimal b2 = new BigDecimal("100.124");
		System.out.println(b2.abs());
		//Using MathContext
		MathContext mc = new MathContext(2, RoundingMode.CEILING);
		BigDecimal b3 = new BigDecimal("654.75");
		System.out.println(b3.abs(mc));
	}
} 
```
- Output
```
234.54
100.124
6.6E+2 
```

``` BigDecimal negate() ```
- It returns a BigDecimal with negative value of the original value. 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class NegateDemo {
	public static void main(String[] args) {
		MathContext mc = new MathContext(2, RoundingMode.CEILING);
		BigDecimal bdec1 = new BigDecimal("-134.43", mc);
		System.out.println("negate() of -134.43 :"+bdec1.negate());
		BigDecimal bdec2 = new BigDecimal("155.33", mc);
		System.out.println("negate() of 155.33 :" + bdec2.negate());
	}
}
```
- Output
```
negate() of -134.43 :1.3E+2
negate() of 155.33 :-1.6E+2 
```

``` BigDecimal plus() ```
- It returns a BigDecimal with +this 
```java
import java.math.BigDecimal;
public class PlusDemo {
	public static void main(String[] args) {
		BigDecimal bdec1 = new BigDecimal("-134.43");
		System.out.println("plus() of -134.43 :"+bdec1.plus());
		BigDecimal bdec2 = new BigDecimal("155.33");
		System.out.println("plus() of 155.33 :" + bdec2.plus());
	}
} 
```
- Output
```
plus() of -134.43 :-134.43
plus() of 155.33 :155.33 
```

``` BigDecimal signum() ```
- It returns the signum function values of the BigDecimal . Signum function value for negative values is -1 and signum function for 0 is 0 and signum function for positive value is +1. 
```java
import java.math.BigDecimal;
public class SignumDemo {
	public static void main(String[] args) {
		//-1 for values < 0
		System.out.println("signum for -15.56: "+ new BigDecimal("-15.56").signum());
		//0 for value 0
		System.out.println("signum for   0.0: "+ new BigDecimal("0.0").signum());
		//1 for values > 0
		System.out.println("signum for  25.43: "+ new BigDecimal("25.43").signum());
	}
} 
```
- Output
```
signum for -15.56: -1
signum for   0.0: 0
signum for  25.43: 1 
```

``` BigDecimal scale() ```
- This method returns the scale of BigDecimal. 
```java
import java.math.BigDecimal;
public class ScaleDemo {
	public static void main(String[] args) {
		//If zero or positive, the scale is the number of digits to the right of the decimal point.
		System.out.println("scale for 0.0 : "+ new BigDecimal("0.0").scale());
		System.out.println("scale for 134.23: "+ new BigDecimal("134.23").scale());
		//If negative, the unscaled value of the number is multiplied 
		//by ten to the power of the negation of the scale.
		System.out.println("scale for -13.231: "+ new BigDecimal("-13.231").scale());
	}
} 
```
- Output
```
scale for 0.0 : 1
scale for 134.23: 2
scale for -13.231: 3 
```

``` BigDecimal precision() ```
- This method returns the precision of BigDecimal. 
```java
import java.math.BigDecimal;
public class PrecisionDemo {
	public static void main(String[] args) {
		System.out.println("precision for 0.0 : "+ new BigDecimal("0.0").precision());
		System.out.println("precision for 134.23: "+ new BigDecimal("134.23").precision());
		System.out.println("precision for -13.231: "+ new BigDecimal("-13.231").precision());
	}
} 
```
- Output
```
precision for 0.0 : 1
precision for 134.23: 5
precision for -13.231: 5 
```

``` BigDecimal unscaledValue() ```
- It calculates the unscaled value of a given BigDecimal. It computes this * 10^this.scale(). 
```java
import java.math.BigDecimal;
public class UnscaledValueDemo {
	public static void main(String[] args) {
		System.out.println("unscaledValue for 0.0 : "+ new BigDecimal("0.0").unscaledValue());
		System.out.println("unscaledValue for 134.23: "+ new BigDecimal("134.23").unscaledValue());
		System.out.println("unscaledValue for -13.231: "+ new BigDecimal("-13.231").unscaledValue());
	}
} 
```
- Output
```
unscaledValue for 0.0 : 0
unscaledValue for 134.23: 13423
unscaledValue for -13.231: -13231 
```

``` BigDecimal round() ```
- It rounds a BigDecimal according to a given MathContext. 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class RoundDemo {
	public static void main(String[] args) {
		MathContext mc = new MathContext(2, RoundingMode.CEILING);
		System.out.println("scale for 0.0 : "+ new BigDecimal("0.0").round(mc));
		System.out.println("scale for 134.23: "+ new BigDecimal("134.23").round(mc));
		System.out.println("scale for -13.231: "+ new BigDecimal("-13.231").round(mc));
	}
} 
```
- Output
```
scale for 0.0 : 0.0
scale for 134.23: 1.4E+2
scale for -13.231: -13 
```

``` BigDecimal scaleByPowerOfTen()  ```
- It returns the BigDecimal with the value (this * 10n). 
```java
import java.math.BigDecimal;
public class ScaleByPowerOfTenDemo {
	public static void main(String[] args) {
		BigDecimal b1 = new BigDecimal("12.5");
		System.out.println("Result: "+b1.scaleByPowerOfTen(4));
	}
} 
```
- Output
```
Result: 1.25E+5 
```

``` BigDecimal stripTrailingZeros() ```
- It strips tailing zeros and returns a BigDecimal with equal value of original BigDecimal. 
```java
import java.math.BigDecimal;
public class StripTrailingZerosDemo {
	public static void main(String[] args) {
		BigDecimal b = new BigDecimal("120000.00");
		System.out.println("Result: "+b.stripTrailingZeros());
	}
} 
```
- Output
```
Result: 1.2E+5 
```

``` BigDecimal compareTo() ```
- This method compares the BigDecimal with given BigDecimal and returns -1, 0 or 1 as this BigDecimal is numerically less than, equal to, or greater than given value. If the values of BigDecimal instances are equal but scales are different, they are considered equal for compareTo() method. 
```java
import java.math.BigDecimal;
public class CompareToDemo {
	public static void main(String[] args) {
		System.out.println(new BigDecimal("300.43").compareTo(new BigDecimal("150.12")));
		System.out.println(new BigDecimal("200.42").compareTo(new BigDecimal("350.56")));
		System.out.println(new BigDecimal("140.56").compareTo(new BigDecimal("140.21")));
	}
} 
```
- Output
```
1
-1
1 
```

``` BigDecimal equals() ```
- This method checks for equality of a BigDecimal with a given BigDecimal. If the values of BigDecimal instances are equal but scales are different, they are not considered as equal by equals() method. 
```java
import java.math.BigDecimal;
public class EqualsDemo {
	public static void main(String[] args) {
		System.out.println(new BigDecimal("300.34").equals(new BigDecimal("150.67")));
		System.out.println(new BigDecimal("140.78").equals(new BigDecimal("140.78")));
	}
} 
```
- Output
```
false
true 
```

``` BigDecimal min() and max() ```
- min() returns the BigDecimal with minimum value and max() returns the BigDecimal with maximum value between the two BigDecimal instances. 
```java
import java.math.BigDecimal;
public class MinMaxDemo {
	public static void main(String[] args) {
		System.out.println("Min: "+ new BigDecimal("300.34").min(new BigDecimal("150.87")));
		System.out.println("Max: "+new BigDecimal("300.34").max(new BigDecimal("150.87")));
	}
}
```
- Output
```
Min: 150.87
Max: 300.34 
```

``` BigDecimal toEngineeringString() ```
- It returns BigDecimal with Engineering notation 
```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;
public class ToEngineeringStringDemo {
	public static void main(String[] args) {
		MathContext mc = new MathContext(2, RoundingMode.CEILING);
		BigDecimal bdec = new BigDecimal("234.87", mc);
		System.out.println("Result: "+ bdec);
		System.out.println("Result with toEngineeringString(): "+bdec.toEngineeringString());
	}
}
```
- Output
```
Result: 2.4E+2
Result with toEngineeringString(): 240 
```

``` BigDecimal ulp() ```
- It returns BigDecimal with ULP(Unit in the last place) 
```java
import java.math.BigDecimal;
public class ULPDemo {
	public static void main(String[] args) {
		BigDecimal bdec = new BigDecimal("234.87");
		System.out.println("ULP of 234.87 : "+ bdec.ulp());
	}
} 
```
- Output
```
ULP of 234.87 : 0.01 
```

## Reference
- https://docs.oracle.com/javase/7/docs/api/java/math/BigDecimal.html