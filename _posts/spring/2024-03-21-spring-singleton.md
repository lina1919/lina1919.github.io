---
title: 싱글톤패턴에 대해 설명해주세요
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
popular: true
categories:
- Spring
- 디자인패턴
toc: true
toc_sticky: true
toc_label: 목차
description: 싱글톤패턴
last_modified_at: 2024-03-21T00:00:00+08:00
---

## 싱글톤 패턴이란?
- 싱글톤 패턴이란 인스턴스가 하나만 생성되는 것을 보장하는 디자인패턴
- 가장 원초적인 방법은 클래스 내부에서 private static final 키워드로 객체를 만들면 외부에서는 해당 클래스의 객체를 새로 생성할 수 없으므로 싱글톤 패턴 조건을 만족하게 됩니다.
- 또한 인스턴스가 절대적으로 한 개만 존재하는 것을 보증하고 싶을 때 사용함


### 싱글톤 패턴의 장점
- 효율적인 메모리 사용
- 주로 공통된 객체를 여러개 생성해서 사용해야하는 상황
        
    > 데이터베이스에서 커넥션풀, 스레드풀, 캐시, 로그 기록 객체 등 안드로이드 앱 : 각 액티비티 들이나, 클래스마다 주요 클래스들을 하나하나 전달하는게 번거롭기 때문에 싱글톤 클래스를 만들어 어디서든 접근하도록 설계
        


### 싱글톤 패턴의 단점
- 싱글톤 패턴을 구현 코드가 많이 들어간다.
- 의존관계상 클라이언트가 **구체 클래스에 의존** → **DIP**를 위반한다.
- 클라이언트가 **구체 클래스에 의존** → **OCP** 원칙 또한 위반할 가능성이 높다.
- **테스트**마다 데이터를 초기화를 해주어야 하므로 **까다롭다**.
- 내부 속성을 변경하거나 초기화 하기 어렵다.
- private 생성자로 **자식 클래스를 만들기 어렵다**.
- 결론적으로 **유연성이 떨어진다.**
- **Anti-패턴**으로 불리기도 한다.
- 멀티스레드 환경에서 동시성 문제 발생 가능성

## 스프링에서는 왜 모든 bean을 싱글톤 객체로 만들까?
스프링은 tomcat 웹서버를 사용하기에 멀티스레드 환경이다. 만약 싱글톤 객체를 사용하지 않는다면 멀티스레드 환경에서 무분별하게 계속 객체가 생성이 되어 엄청난 성능 저하 및 메모리 낭비가 발생한다.

### **싱글톤 컨테이너**

- 스프링에서는 사용자가 이렇게 일일이 구현하지 않고 싱글톤 패턴의 문제점들도 보완해주면서 싱글톤 패턴으로 클래스의 인스턴스를 사용하게 해줌
- 즉, **스프링컨테이너 자체가 하나의 인스턴스만을 보장**
- 스프링 빈 = 싱글톤으로 관리되는 빈

### **싱글톤 방식 설계 시 주의할 점**

- *무상태(stateless)*로 설계해야 한다!
- 특정 클라이언트에 **의존적인 필드가 있으면 안된다**
- 특정 클라이언트가 **값을 변경할 수 있는 필드가 있으면 안된다!**
- 공유 필드 대신에 자바에서 **지역변수, 파라미터, ThreadLocal** 등을 사용해야 한다.
- 스프링 빈의 필드에 **공유 값을 설정하면 정말 큰 장애가 발생할 수 있다**!!!

### **@Configuration과 싱글톤**

- @Bean만 사용해도 스프링 빈으로 등록되지만, 싱글톤을 보장하지 않는다.
- memberRepository() 처럼 의존관계 주입이 필요해서 메서드를 직접 호출할 때 싱글톤을 보장하지 않는다.
- 그러므로 스프링 설정 정보는 항상 @Configuration 을 사용하자.