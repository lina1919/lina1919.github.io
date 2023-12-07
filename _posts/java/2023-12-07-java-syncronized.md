---
title: Java에서 동시성 문제를 해결하는 방법(synchronized, volatile, atomic class)
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- JAVA
toc: true
toc_sticky: true
toc_label: 목차
description: Java에서 동시성 문제를 해결하는 방법
meta_keywords: java
last_modified_at: '2023-12-07 23:00:00 +0800'
---
자바에서 동시성 문제를 해결하는데 3가지 방법이 있습니다.

### Synchronized :
- 안전하게 동시성을 보장할 수 있습니다. 특정 스레드는 `synchronized` 메서드에 접근 시 블록 전체에 lock을 건다. 따라서 해당 스레드가 블럭을 빠져나가기 전까지 다른 스레드들은 동기화 처리된 블록에 접근할 수 없다. 따라서 critical section의 크기및 실행시간에 따라 성능하락 및 자원낭비가 매우 심해지게 된다.

### Volatile :
**volatile 키워드를 붙인 자원은 read, write 작업이 CPU Cache Memory가 아닌 Main Memory에서 이뤄집니다.**
- `volatile` 변수를 사용하고 있지 않는 MultiThread 어플리케이션에서는 Task를 수행하는 동안 `성능 향상`을 위해 Main Memory에서 읽은 변수 값을 CPU Cache에 저장하게 됩니다.
- 기존의 스레드들은 변수에 대해 Read & Write의 작업을 수행할 때, 그 대상은 각 스레드가 할당받은 **CPU의 Cache Memory**라고 한다. (HW 성능을 위한 설계로 인한 것이라고 한다.) 그러나 Volatile 키워드가 붙은 변수를 대상으로 여러 스레드들이 Read & Write 작업을 수행할 때는 **작업의 대상으로 Main Memory로 삼는다는 것**이다.
- 성능상의 이유로 스레드들은 각각 할당받은 **CPU의 Cache Memory를 이용하게 되는데 이곳에 저장된 연산의 결과가 언제 Main Memory로 올라갈지 모른다고 한다.**
- 만약에 Multi Thread환경에서 `Thread가 변수 값을 읽어올 때` 각각의 CPU Cache에 저장된 값이 다르기 때문에 `변수 값 불일치 문제`가 발생하
- 즉, 자원을 저장하는 메모리는 하나가 되기 때문에 같은 공유자원에 대해 각각 메모리별로 다른 값을 가지는 경우가 없습니다.
- 따라서 하나의 Thread만 read & write하고 나머지 Thread가 read하는 상황에서 `가장 최신의 값을 보장합니다.`
     - 즉, 가시성을 보장합니다.
- 하지만 동시성을 보장하지 않습니다. **여러 스레드**에서 Main Memory에 있는 공유자원에 동시에 접근할 수 있으므로 여러 스레드에서 수정하게 되면, 계산값이 덮어씌워지게 되므로 **동시 접근 문제를 해결할 수 없**습니다.
    - 예를 들어 `++` 연산과 같이 원자성이 보장되지 않는 경우는 동기화를 보장하지 않는다.

### Atomic 클래스
- Atomic 클래스는 Non-Blocking 임에도 동시성을 보장하기 위해 CAS(compare-and-swap)를 이용하여 동시성을 하므로 여러 쓰레드에서 데이터를 write해도 문제가 없습니다. synchronized 보다 적은 비용으로 동시성을 보장할 수 있습니다.
    
    - **CAS 알고리즘이란 현재 스레드가 존재하는 CPU의 CacheMemory와 MainMemory에 저장된 값을 비교하여, 일치하는 경우 새로운 값으로 교체하고, 일치하지 않을 경우 기존 교체가 실패되고, 이에 대해 계속 재시도하는 방식입니다.**
    - **CPU가 MainMemory의 자원을 CPU Cache Memory로 가져와 연산을 수행하는 동안 다른 스레드에서 연산이 수행되어 MainMemory의 자원 값이 바뀌었을 경우 기존 연산을 실패처리하고, 새로 바뀐 MainMemory 값으로 재수행하는 방식입니다.**
