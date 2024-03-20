---
title: java final에 대해 설명해주세요
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
popular: true
categories:
- Java
toc: true
toc_sticky: true
toc_label: 목차
description: java final
last_modified_at: 2024-03-20T00:00:00+08:00
---
# Java final 에 대해서 설명해주세요

## final 클래스

final이 클래스 이름 앞에 사용되면 클래스를 **상속받을 수 없음** 을 지정한다.

```java
final class FinalClass {
  ...
}
class DerivedClass extends FinalClass { // 컴파일 오류
  ...
}

```

- FinalClass를 상속받아 DerivedClass를 만들 수 없다.

## final 메서드

메서드 앞에 final 속성이 붙으면 이 메서드는 더 이상 **오버라이딩할 수 없음** 을 지정한다.

```java
public class SuperClass {
  protected final int finalMethod() { ... }
}
class DerivedClass extends SuperClass { // SuperClass를 상속 받음
  protected int finalMethod() { ... } // 컴파일 오류
}

```

- 자식 클래스가 부모 클래스의 특정 메서드를 오버라이딩하지 못하게 하고 무조건 상속받아 사용하도록 하고자 한다면 final로 지정하면 된다.

## final 필드, 상수 정의

자바에서 상수를 정의하는 방법: 필드 멤버에 final 키워드를 덧붙여 상수를 정의한다.

```java
public class FinalFieldClass {
  final int ROWS = 10; // 상수 정의, 이때 초깃값(10)을 반드시 설정

  void f() {
    int[] intArray = new int[ROWS]; // 상수 활용
    ROWS = 30; // 컴파일 오류. final 필드 값은 상수으므로 변경할 수 없다.
  }
}

```

- final로 상수 필드를 정의할 때 **선언 시에 초깃값을 지정** 해야 한다.
- 상수 필드는 한 번 정의되면 값을 변경할 수 없다.
- final 키워드만을 사용하여 상수를 만들면 FinalFieldClass의 객체들만 사용할 수 있는 상수가 된다.

프로그램 전체에서 공유하여 사용할 수 있는 상수

- final 키워드 + static
- Ex) **public static final**

```java
class ShareClass {
  public static final double PI = 3.141592653589793;
}
/* ShareClass 내에서의 사용 */
double area = PI * radius * radius;
/* 다른 클래스에서의 사용 */
double area = ShareClass.PI * radius * radius;

```

###  **`final`** 변수가 있는 객체와 불변(immutable) 객체의 차이점은 무엇인가요?

- **답변**: **`final`** 변수는 한 번 할당되면 그 주소값이 변경되지 않지만, 객체 자체가 불변하는 것은 아닙니다. 객체의 상태(필드 값)는 여전히 변경될 수 있습니다. 반면, 불변 객체는 그 상태가 절대 변경되지 않습니다. 모든 필드가 **`final`**로 선언되어 있고, 객체가 생성된 후에는 어떠한 방식으로도 상태를 변경할 수 없는 객체를 불변 객체라고 합니다. **`final`** 변수는 불변 객체를 참조할 수 있지만, 불변 객체 자체가 되는 것은 아닙니다.
