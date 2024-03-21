---
title: JVM garbage collector의 동작방식에 대해 설명해주세요
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
popular: true
categories:
- Java
- JVM
toc: true
toc_sticky: true
toc_label: 목차
description: JVM garbage collector
last_modified_at: 2024-03-21T13:00:00+08:00
---

## Garbage Collector란?
동적으로 할당한 메모리 영역 중, 사용하지 않는 영역을 탐지해 해제하는 역할을 합니다.

Java에서 동적 변수를 할당하는 영역은 Heap 영역이므로, JVM의 GC의 작동 대상은 Heap 메모리입니다.

기존의 참조 변수와 참조 변수가 가리키는 동적변수 관계에서 참조 변수가 함수 종료에 의해 pop되고 나면 힙 영역에 동적 변수만 남는데, 이러한 변수를 Unreachable Object라고 표현하고, 이 변수가 GC의 관리 대상이 됩니다.

### GC의 장단점

- 메모리 누수 방지
- 해제된 메모리에 대해 접근하는 것을 방지
- 해제한 메모리를 다시 해제하는 것을 방지
- 개발자가 일일히 동적변수의 제거 선언을 하지 않아도 됨
- 그러나, 개발자가 GC가 언제 수행되는지 정확하게 알 수 없음
- 실행 중인 작업을 중단하고, 리소스를 GC 작업에 내줘야 하므로 오버헤드 발생


## JVM Garbage Collection의 동작 방식

1. Stop The World
    1. 가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업
    2. GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되고, GC가 완료되면 작업이 재개
    3. **parallel GC**
        1. java8에서 기본적으로 쓰이는 GC 방식
        2. multi core 환경에서 사용됨
        3. 여러 개의 thread로 GC를 실행하기 때문에 stop the world 시간이 짦음
    4. **G1 GC**
        1. java9부터 기본적으로 쓰이는 GC방식
        2. heap을 일정 크기의 region으로 잘게 나눠서 young generation, old generation으로 나눠서 활용
        3. 이때 region은 그때 그때 개수를 알아서 튜닝해준다.
        4. 이에 따라 stop the world 최소화 가능
2. Mark and Sweep
    1. Mark: 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
    2. Sweep: Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업
    3. 의도적으로 GC를 실행시켜야함.
        1. JVM에겐 이쯤에서 GC를 실행시키겠다는 나름의 기준이 존재함.
        2. 이 기준을 알기 위해서는 JVM의 heap 영역을 들여다봐야함.
        3. application 실행과 GC 실행이 병행됨
![https://blog.kakaocdn.net/dn/bL5Th5/btrPT1O6YaL/OhImuZZsGZF3L3NmcnY6Tk/img.png](https://blog.kakaocdn.net/dn/bL5Th5/btrPT1O6YaL/OhImuZZsGZF3L3NmcnY6Tk/img.png)


## Minor GC, Major GC
### Minor GC
Young 영역에서 발생하는 gc

### Major Gc
Old/perm 영역에서 발생하는 gc

JVM Heap 영역은 young, old, permanent 영역으로 나눌 수 있다.
Young 영역은 eden, survivor 0,1 로 나눠질 수 있다.
객체가 생성되면 eden 영역에 생성이 되고, Eden이 꽉 차면 survivor 영역으로 넘어가게 된다.
Survival 영역이 차면 mark&sweep 방식을 통해 다른 survivor 영역으로 옮겨지게 되는데, 이 과정이 반복되던 객체들은 old 영역으로 옮겨가게 된다.
Survival 영역에 속하지 않고 바로 old 영역으로 가는 경우도 있는데, 객체의 크기가 너무 큰 경우에 바로 이동시킨다.
