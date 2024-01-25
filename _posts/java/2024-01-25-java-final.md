---
title: Java - final / finally / finalize 의 차이를 설명해주세요.
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
description: Java final / finally / finalize 의 차이
meta_keywords: java
last_modified_at: '2024-01-25 23:00:00 +0800'
---
1. final
    - 키워드를 명시하여 한번만 할당하고 싶을 때 사용
    - **final 필드는 선언할 때 그 즉시 초기화를 해주어야 하며, 한번 초기화한 이후로는 재생성(수정)이 불가능**
    - 변수에 적용하면 : 해당 변수의 값은 변경이 불가능해진다. 상수가 되기 때문
    - 변수의 참조에 적용하면 : 참조 변수가 힙 내의 다른 객체를 가리키도록 변경할 수 없다.
    - 메서드에 적용하면 : 해당 메서드를 상속받는 하위 클래스에서 오버라이딩 할 수 없다.(상속받는 하위 클래스에서도 변경이 되지 않아야 하는 메서드의 경우 final을 붙이면 된다.)
    - 클래스에 적용하면 : 해당 클래스를 다른 클래스가 상속받을 수 없다. 즉, final 클래스의 하위 클래스를 정의할 수 없다.
2. finally
    - try catch 블록 뒤에서 항상 실행될 코드 블록을 정의하기 위해 사용
    - try-catch블록과 함께 사용되며 예외가 던져지더라도 **항상 실행될 코드를 지정**하기 위해 사용된다. finally블록은 try와 catch블록이 전부!!! 실행된 후, 그리고 제어 흐름이 원래 지점으로 돌아가기 전에 실행된다.
3. finalize()
    - finalize() 메서드는 java garbage collector가 더 이상의 참조가 존재하지 않는 객체를 발견한 순간 호출하는 메서드
    - java.lang.Object클래스로 부터 상속받아 모든 클래스의 객체가 가지고 있는 메서드다. 즉, 만약 객체가 삭제되기 직전에 실행되어야 하는 로직같은게 있으면 Object 클래스에 정의된 finalize() 메서드를 오버라이딩하여 정의할 수 있다.
