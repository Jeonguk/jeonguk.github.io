---
layout: post
title:  "declare-private-and-protected-methods-in-interfaces"
date:   2018-02-06 09:46:10 +0700
categories: [java,language]
---

# Declare Private and Protected Methods in Interfaces

* Java 8이 도입되었을 때 인터페이스에서 기본 메소드를 사용할 수 있었습니다. 
이 기능의 주요 드라이버는 이전 인터페이스 버전에 대한 이전 버전과의 호환성을 유지하면서 인터페이스 확장을 허용하는 것이 었습니다. 
한 가지 예는 기존 Collection 클래스에 stream () 메서드를 도입 한 것입니다.
* 때로는 몇 가지 기본 메소드를 소개하고자 할 때 공통 코드베이스를 공유 할 수 있으며 인터페이스에서 개인 메소드를 사용할 수 있다면 좋을 것입니다. 
이렇게하면 코드를 재사용 할 수 있고 인터페이스를 사용 중이거나 구현중인 클래스에 코드가 노출되는 것을 방지 할 수 있습니다.
* 그러나 문제가 있습니다. 인터페이스에서 개인 및 보호 된 액세스가 Java 9로 연기되었습니다. 어떻게 Java 8에서 개인 인터페이스 메소드를 사용할 수 있습니까?

