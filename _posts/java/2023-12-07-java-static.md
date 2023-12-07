---
title: Java static(정의/장단점)
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
description: Java static의 장당점과 정의를 설명합니다.
meta_keywords: java
last_modified_at: '2023-12-07 23:00:00 +0800'
---
## Static 키워드란?
- 데이터 영역에 저장되는 정적 변수
- 메모리 공간에 할당이 되어서 공유되는 변수
- Static 멤버 메소드 내에서는 static 멤버 변수와 static 멤버 method만 호출이 가능하다
- Static 클래스가 메모리에 올라가는 시점에 생성된다.
- 따라서 GC의 대상이 아니기 때문에 프로그램이 종료될 때까지 메모리에 할당된 채로 존재하게 된다.

## Static 키워드의 장단점
### 장점
1. 코드를 간결하게 작성할 수 있다.
2. 어디서든 접근이 가능하다.
	1. 어느 클래스든 static으로 선언된 변수나 메소드를 불러와 사용할 수 있다.
3. GC 오버헤드를 줄일 수 있다.

### 단점
1. 메모리 부족 현상이 나타날 수 있다.
	1. static으로 선언된 모든 것들은 프로그램이 시작될 때 메모리를 할당한다. 따라서 메모리를 해제하기 위해서는 프로그램을 종료해야하는데, 만약 무분별하게 static을 사용하게 된다면 GC가 메모리 해제를 하지 못해 메모리 부족 현상이 발생될 수 있다.
2. 객체지향적이지 않다.
	1. 객체 지향 개발요소 중 하나인 캡슐화를 위배한다.
3. static은 재사용성이 떨어진다.
	1. static으로 선언된 메소드는 interface를 구현하는데 사용될 수 없다.
	2. 따라서 객체지향 개발 특징 중 한가지인 재사용성을 살리지 못하기 때문에 객체지향개발에 방해가 된다.
4. Static은 Thread-safe하지 않다. 
