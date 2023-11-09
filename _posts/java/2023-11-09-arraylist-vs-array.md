---
title: Array vs ArrayList
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- JAVA
- 자료구조
toc: true
toc_sticky: true
toc_label: 목차
description: ArrayList와 Array의 차이
meta_keywords: java
last_modified_at: '2023-11-09 23:00:00 +0800'
---
- ArrayList와 Array는 거의 비슷한 개념이지만 몇가지 차이점들이 있습니다.

1. Java의 Array는 고정 길이.
   
    따라서, 정해진 길이의 배열을 모두 채우면, 새로운 데이터를 추가하고 싶을 경우 새로운 배열을 만들어주어야 한다. 하지만 ArrayList는 동적으로 배열길이를 조절 할 수 있다.
    
2. ArrayList는 Object 타입을 원소로 갖는다.
   
    만약 int의 배열을 원할 경우에 배열 대신 ArrayList를 이용하면 Boxing이 발생하게 된다. 이럴 경우 속도도 느리고 메모리를 더 많이 차지하게 된다. 왜냐하면 int는 값형인데 Object는 참조형이기 때문에 int를 object와 호환되는 타입으로 바꾸면서 Boxing을 하게 되고 참조를 만들게 되기 때문이다.
    
3. ArrayList는 Linked List기 때문에 연결고리를 유지하기 위한 메모리 부담이 있다.
   
    ArrayList는 크기를 자유롭게 변경할 수 있다는 장점이 있지만, 그만큼 속도나 메모리 사용량 측면에서는 부담스럽다. 또한 ArrayList는 모든 타입을 수용할 수 있는 Object 타입을 원소로 갖기 때문에 아무 것이나 넣을 수 있다는 장점이 있지만 반대로 꺼내서 이용할 때는 type-safe하지 않기 때문에 타입 캐스팅할 때 주의를 해야 한다.
