---
title: JVM이 무엇인지 설명해주세요(정의,메모리 구조,실행과정)
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
description: JVM
last_modified_at: 2024-03-21T12:00:00+08:00
---

## JVM이란?
JVM은 Java Virtual Machine의 줄임말로, OS와 Java Application 사이를 중재해주는 가상 머신입니다.

- 가상 머신이란 프로그램을 실행하기 위해 물리적 머신과 유사한 머신을 소프트웨어로 구현한 것
- ARM 아키텍쳐 같은 하드웨어가 레지스터 기반으로 동작하는 것에 반해 JVM은 **스택 기반**으로 동작함

[동작]

JVM은 크게 실행에 필요한 클래스를 Runtime Data Area로 링킹 하고 로딩을 해주는 ClassLoader, 실제 실행과 번역을 담당하는 Execution Engine, 별도의 쓰레드로써 힙 메모리의 참조되지 않는 객체를 삭제하는 Garbage Collector, 각 메모리 영역 및 쓰레드가 담겨있는 Runtime Data Area로 구성이 되어있습니다.

[ JVM의 기능 ] 

1. 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행
2. Java와 OS 사이의 중개자 역할을 수행하여 JAVA가 OS에 구애받지 않고 재사용을 가능하게 해줌
3. 가장 중요한 메모리 관리 Garbage Collection을 수행

## 자바 프로그램 실행 과정

![](https://i.imgur.com/TvciIPe.png)

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다.
   JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.

2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환시킨다.
3. Class Loader를 통해 class파일들을 JVM으로 로딩한다.
4. 로딩된 class파일들은 Execution engine을 통해 해석된다.
5. 해석된 바이트코드는 Runtime Data Areas 에 배치되어 실질적인 수행이 이루어지게 된다.

→ 이러한 실행과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC같은 관리작업을 수행한다.

## JVM의 메모리 구조
- 메서드(static) 영역
    - JVM이 실행되면서 생기는 공간
    - 모든 스레드에서 공유되는 공간
    - 클래스가 사용되면 해당 클래스의 파일(.class)을 읽어들여, 클래스에 대한 정보(바이트 코드)를 메서드 영역에 저장
    - 클래스와 인터페이스, 메서드, 필드, static 변수, final 변수 등이 저장되는 영역입니다.
- JVM 스택 영역
    - 스레드마다 존재하여 스레드가 시작할 때마다 할당
    - 지역변수, 매개변수, 연산 등과 같이 잠시 사용되고 필요가 없어지는 임시 데이터 저장
    - 메서드 호출 시마다 개별적 스택 생성
- JVM 힙 영역
    - 런타임 시 동적으로 할당하여 사용하는 영역
    - new 연산자로 생성된 객체와 배열 저장
    - 참조가 없는 객체는 GC(가비지 컬렉터)의 대상/GC가 처리하지 않는 이상 소멸되지 않음
    - 모든 스레드에서 정보 공유
- pc register
    - 스레드가 어느 명령어를 처리하고 있는지 그 주소를 등록
    - JVM이 실행하고 있는 현재 위치를 저장하는 공간
    - 쓰레드가 현재 실행할 스택 프레임의 주소를 저장
- Native Method Stack
    - Java가 아닌 다른 언어(c, c++)로 구성된 메서드를 실행이 필요할 때 사용되는 공간
