---
title: filter vs interceptor
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
popular: true
categories:
- Database
toc: true
toc_sticky: true
toc_label: 목차
description: filter vs interceptor
last_modified_at: 2024-03-21T00:00:00+08:00
---
## 필터

필터(Filter)는 J2EE 표준 스펙 기능으로 클라이언트(Client)의 요청을 받은 Servlet/JSP가 수행되기 전, 후로 호출되어 수행되는 웹 컴포넌트입니다. Filter는 스프링(Spring) 범위 밖에서 처리됩니다. 즉, 스프링 컨테이너(Spring Container)가 아닌 톰캣(Tomcat)과 같은 서블릿 컨테이너(Servlet Container)에 의해 관리됩니다.

![https://i.imgur.com/D4WVT7n.png](https://i.imgur.com/D4WVT7n.png)

### 호출 시점

Filter는 DispatcherServlet이 실행되기 전 호출

### 설정 위치

web.xml (Web Application Server에 등록)

### 구현 방식

Filter는 web.xml에서 설정을 하면 구현 가능

### 용도

- 데이터 변환(다운로드 파일의 압축 및 데이터 암호화 등)을 할 때
- XSL/T를 이용한 XML 문서 변경을 할 때
- 사용자 인증을 할 때
- 자원 접근에 대한 로깅을 할 때

Filter에서 ServletRequest 혹은 ServletResponse를 교체할 수 있다. ex) HttpServletRequest의 body(ServletInputStream의 내용)를 로깅하는 것

## 인터셉터

인터셉터(Interceptor)는 Filter와 달리 스프링(Spring)에서 제공하는 기술입니다. 디스패처 서블릿(Dispatcher Servlet)이 컨트롤러(Controller)를 호출하기 전과 후에 요청(Request)과 응답(Response)을 참조하거나 가공할 수 있는 기능을 제공합니다. 즉, 컨트롤러(Controller)에 들어오는 요청 HttpRequest와 컨트롤러(Controller)가 응답하는 HttpResponse를 가로채는 역할을 합니다.

![https://i.imgur.com/PPijSe5.png](https://i.imgur.com/PPijSe5.png)

### 호출 시점

Interceptor는 DispatcherServlet이 실행된 후 호출

### 설정 위치

spring-sevlet.xml (Spring의 Context에 등록)

### 구현 방식

Interceptor는 설정과 메서드 구현이 모두 필요

### 용도

- 관리자만 접근할 수 있는 관리자 페이지에 접근하기 전에 관리자 인증
- 핸들러 메서드(Handler Method)에서 사용자의 권한을 체크해서 다른 동작을 시켜준다거나 할 때
- 뷰(View)를 렌더링하기 전에 추가 작업
- 스프링(Spring) 애플리케이션에서 전역적으로 전후처리 로직에서 예외를 사용하도록 할 때
- AOP 흉내 내기 가능

### 정리

| ---                             | ---                                                                                                                              | ---                                                                                                   |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
|                                 | 필터                                                                                                                             | 인터셉터                                                                                              |
| 관리 주체 컨테이너              | 웹 컨테이너                                                                                                                      | 스프링 컨테이너                                                                                       |
| request/response 조작 가능 여부 | O                                                                                                                                | X                                                                                                     |
| 용도                            | 공통된 보안 및 인증/인가 관련 작업, 모든 요청에 대한 로깅 또는 감사, 이미지/데이터 압축 및 문자열 인코딩, 스프링과 분리되는 기능 | 세부적인 보안 및 인증/인가 공통 작업, API 호출에 대한 로깅 또는 감사, 컨트롤러로 넘겨주는 데이터 가공 |
|                                 |                                                                                                                                  |                                                                                                       |
