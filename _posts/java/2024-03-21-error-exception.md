---
title: 예외 처리 방법 (Checked Exception, Unchecked Exception)
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
description: 예외
last_modified_at: 2024-03-21T17:00:00+08:00
---
## **1. Error vs Exception**

먼저 오류(Error)와 예외(Exception)의 차이는 무엇인가?

### Error
- 오류(Error)는 시스템에 비정상적인 상황이 생겼을 때 발생
    - 이는 시스템 레벨에서 발생하기 때문에 심각한 수준의 오류이다. 따라서 개발자가 미리 예측하여 처리할 수 없기 때문에, 애플리케이션에서 오류에 대한 처리를 신경 쓰지 않아도 됨
### Exception
- 오류가 시스템 레벨에서 발생한다면, 예외(Exception)는 개발자가 구현한 로직에서 발생
    - 즉, 예외는 발생할 상황을 미리 예측하여 처리할 수 있다. 즉, 예외는 개발자가 처리할 수 있기 때문에 예외를 구분하고 그에 따른 처리 방법을 명확히 알고 적용하는 것이 중요

## **2. 예외클래스**

![](https://i.imgur.com/G1TJGwe.png)


모든 예외클래스는 Throwable 클래스를 상속받고 있으며, Throwable은 최상위 클래스 Object의 자식 클래스이다.

Trowable을 상속받는 클래스는 Error와 Exception이 있다. Error는 시스템 레벨의 심각한 수준의 에러이기 때문에 시스템에 변화를 주어 문제를 처리해야 하는 경우가 일반적이다. 반면에 Exception은 개발자가 로직을 추가하여 처리할 수 있다.

Exception은 수많은 자식클래스를 가지고 있다. 그 중 RuntimeException에 주목

RuntimeException은 CheckedException과 UncheckedException을 구분하는 기준이다. Exception의 자식 클래스 중 RuntimeException을 제외한 모든 클래스는 CheckedException이며, RuntimeException과 그의 자식 클래스들을 Unchecked Exception이라 부른다. CheckedException과 UncheckedException에 대해 더 자세히 살펴보자.

## **3. Checked Exception과 Unchecked(Runtime) Exception**

![](https://i.imgur.com/XRmNU7U.png)


Checked Exception과 Unchecked Exception의 가장 명확한 구분 기준은 ‘꼭 처리를 해야 하느냐’이다. Checked Exception이 발생할 가능성이 있는 메소드라면 반드시 로직을 try/catch로 감싸거나 throw로 던져서 처리해야 한다. 반면에 Unchecked Exception은 명시적인 예외처리를 하지 않아도 된다. 이 예외는 피할 수 있지만 개발자가 부주의해서 발생하는 경우가 대부분이고, 미리 예측하지 못했던 상황에서 발생하는 예외가 아니기 때문에 굳이 로직으로 처리를 할 필요가 없도록 만들어져 있다.

또한 예외를 확인할 수 있는 시점에서도 구분할 수 있다. 일반적으로 컴파일 단계에서 명확하게 Exception 체크가 가능한 것을 Checked Exception이라 하며, 실행과정 중 어떠한 특정 논리에 의해 발견되는 Exception을 Unchecked Exception이라 한다. 따라서 컴파일 단계에서 확인할 수 없는 예외라 하여 Unchecked Exception이며, 실행과정 중 발견된다 하여서 Runtime Exception이라 하는 것이다.

예외발생시 트랜잭션의 roll-back 여부이다. 기본적으로 Checked Exception은 예외가 발생하면 트랜잭션을 roll-back하지 않고 예외를 던져준다. 하지만 Unchecked Exception은 예외 발생 시 트랜잭션을 roll-back한다는 점에서 차이가 있다. 트랜잭션의 전파방식 즉, 어떻게 묶어놓느냐에 따라서 Checked Exception이냐 Unchecked Exception이냐의 영향도가 크다. roll-back이 되는 범위가 달라지기 때문에 개발자가 이를 인지하지 못하면, 실행결과가 맞지 않거나 예상치 못한 예외가 발생할 수 있다. 그러므로 이를 인지하고 트랜잭션을 적용시킬 때 전파방식(propagation behavior)과 롤백규칙 등을 적절히 사용하면 더욱 효율적인 애플리케이션을 구현할 수 있을 것이다.

## **4. 예외 처리 방법**

예외를 처리하는 일반적인 방법 3가지이다. 예외 처리 방법에는 예외가 발생하면 다른 작업 흐름으로 유도하는 예외 복구와 처리를 하지 않고 호출한 쪽으로 던져버리는 예외처리 회피, 그리고 호출한 쪽으로 던질 때 명확한 의미를 전달하기 위해 다른 예외로 전환하여 던지는 예외 전환이 있다.

4.1. 예외 복구

```
int maxretry = MAX_RETRY;
while(maxretry -- > 0) {
    try {
        // 예외가 발생할 가능성이 있는 시도
        return; // 작업성공시 리턴
    }
    catch (SomeException e) {
        // 로그 출력. 정해진 시간만큼 대기
    }
    finally {
        // 리소스 반납 및 정리 작업
    }
}
throw new RetryFailedException(); // 최대 재시도 횟수를 넘기면 직접 예외 발생

```

예외복구의 핵심은 예외가 발생하여도 애플리케이션은 정상적인 흐름으로 진행된다는 것이다. 위 [리스트 1]은 재시도를 통해 예외를 복구하는 코드이다. 이 예제는 네트워크가 환경이 좋지 않아서 서버에 접속이 안되는 상황의 시스템에 적용하면 효율 적이다. 예외가 발생하면 그 예외를 잡아서 일정 시간만큼 대기하고 다시 재시도를 반복한다. 그리고 최대 재시도 횟수를 넘기면 예외를 발생시킨다. 재시도를 통해 정상적인 흐름을 타게 한다거나, 예외가 발생하면 이를 미리 예측하여 다른 흐름으로 유도시키도록 구현하면 비록 예외가 발생하였어도 정상적으로 작업을 종료할 수 있을 것이다.

4.2. 예외처리 회피

```
public void add() throws SQLException {
    ... // 구현 로직
}

```

위 [리스트 2]는 간단해 보이지만 아주 신중해야하는 로직이다. 예외가 발생하면 throws를 통해 호출한쪽으로 예외를 던지고 그 처리를 회피하는 것이다. 하지만 무책임하게 던지는 것은 위험하다. 호출한 쪽에서 다시 예외를 받아 처리하도록 하거나, 해당 메소드에서 이 예외를 던지는 것이 최선의 방법이라는 확신이 있을 때만 사용해야 한다.

4.3. 예외 전환

```
catch(SQLException e) {
   ...
   throw DuplicateUserIdException();
}

```

예외 전환은 위 [리스트 3]에서 처럼 예외를 잡아서 다른 예외를 던지는 것이다. 호출한 쪽에서 예외를 받아서 처리할 때 좀 더 명확하게 인지할 수 있도록 돕기 위한 방법이다. 어떤 예외인지 분명해야 처리가 수월해지기 때문이다. 예를 들어 Checked Exception 중 복구가 불가능한 예외가 잡혔다면 이를 Unchecked Exception으로 전환하여서 다른 계층에서 일일이 예외를 선언할 필요가 없도록 할 수도 있다.

예외를 잡고 아무런 처리도 하지 않는 것은 위험하다. 어떤 처리를 해야 하는지 모르더라도 무작정 catch 하고 무시하거나 throw 해버리는 행위는 신중해야한다.

### throw"와 "throws"는 자바 프로그래밍 언어에서 예외 처리와 관련된 키워드로, 각각 다른 목적과 사용 방식을 가집니다.

1. **`throw`**:
    - "throw" 키워드는 예외를 명시적으로 발생시키는 데 사용됩니다.
    - 예외가 발생할 때, 프로그램에서 예외를 생성하고 이를 던져서 예외가 발생한 곳에서 적절한 예외 처리 코드로 제어를 이동시킵니다.
    - 사용 예시: **`throw new Exception("예외 메시지");`**
2. **`throws`**:
    - "throws" 키워드는 메서드 정의 시 예외를 선언하는 데 사용됩니다.
    - 메서드가 특정 종류의 예외를 던질 수 있다는 것을 나타내며, 호출자가 해당 예외에 대한 처리를 해야 함을 알려줍니다.
    - 사용 예시: **`public void someMethod() throws SomeException {}`**

간단히 말하면, "throw"는 예외를 발생시키는 데 사용되고, "throws"는 메서드가 발생시킬 수 있는 예외를 선언하는 데 사용됩니다. 이 두 키워드는 예외 처리에 중요한 역할을 합니다.
