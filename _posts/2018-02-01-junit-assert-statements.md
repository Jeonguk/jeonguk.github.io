---
layout: post
title:  "junit4-assert-statements"
date:   2018-02-01 11:04:10 +0700
categories: [juni,testing,java]
---

# Junit4 Assert statements

* JUnit은 Assert 클래스를 통해 특정 조건을 테스트하는 정적 메서드를 제공합니다. 이러한 assert 문은 일반적으로 assert로 시작합니다. 오류 메시지, 예상 결과 및 실제 결과를 지정할 수 있습니다. 어설 션 방법은 테스트에서 반환 된 실제 값을 예상 값과 비교합니다. 비교가 실패하면 AssertionException을 던집니다.

``` Methods to assert test results ```

| Statement                                  | Description                                                 |
|--------------------------------------------|:------------------------------------------------------------|
| fail([message])                            | method fail. 테스트 코드가 구현되기 전에 코드의 특정 부분에 |
|                                            | 도달하지 않았는지 확인하거나 실패한 테스트를 수행하는 데    |
|                                            | 사용할 수 있습니다. message 매개 변수는 선택적입니다.       |
| assertTrue([message,] boolean condition)   | boolean 조건이 참인지 확인합니다.                           |
| assertFalse([message,] boolean condition)  | boolean 조건이 거짓인지 확인합니다.                         |
| assertEquals([message,] expected, actual)  | 두 값이 같은지 테스트합니다.                                |
|                                            | 참고 : 배열의 경우 참조가 배열의 내용이 아닌지 확인됩니다.  |
| assertEquals([message,] expected, actual   | float 또는 double 값이 일치하는지 테스트.                   |
| , tolerance)                               | 허용 오차는 같아야하는 소수점 이하 자릿수입니다.            |
| assertNull([message,] object)              | 객체가 null 인 것을 확인합니다.                             |
| assertNotNull([message,] object)           | 객체가 null이 아닌지를 판정합니다.                          |
| assertSame([message,] expected, actual)    | 두 변수가 동일한 객체를 참조하는지 확인합니다.              |
| assertNotSame([message,] expected, actual) | 두 변수가 서로 다른 객체를 참조하는지 확인합니다.           |
|                                            |                                                             |
