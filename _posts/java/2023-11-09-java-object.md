---
title: Java Object Class에 대해서 아시나요?
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
description: Java Object Class
meta_keywords: java
last_modified_at: '2023-11-09 23:00:00 +0800'
---
Object Class는 모든 클래스의 최상위 클래스이다.

즉, 모든 클래스는 Object 클래스를 상속 받는다라고 할 수 있는 것이다.

자바의 모든 클래스는 Object 클래스의 자식이거나 자손 클래스가 된다.

컴파일러가 자동으로 extends Object를 추가한다.

`class Student => class Student extends Object`

Object 클래스는 필드가 없고 메소드로 구성되어 있다.

이 메소드들은 모든 클래스들이 Object 클래스를 상속하므로, 모든 클래스에서 이용할 수 있다.

### 꼬리질문1 Object 클래스의 메소드들에는 어떤 것이 있나요?

1. equals
2. hashCode
    - 객체를 식별할 하나의 정수값(객체의 메모리 번지를 이용하여 해시코드를 만들어서 리턴)
3. toString
    - 객체를 문자열로 표현한 값을 return
