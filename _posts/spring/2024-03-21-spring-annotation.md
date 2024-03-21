---
title: Service, Controller, 그리고 Repository 어노테이션이 각각 어떤 역할을 수행하는지?
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
popular: true
categories:
- Spring
toc: true
toc_sticky: true
toc_label: 목차
description: 어노테이션
last_modified_at: 2024-03-21T00:09:00+08:00
---

1. **`@Service`** 어노테이션:
    - **`@Service`** 어노테이션은 비즈니스 로직을 처리하는 서비스 클래스에 붙입니다.
    - 이 클래스는 비즈니스 로직을 구현하며, 주로 서비스 계층에서 사용됩니다.
    - **`@Service`** 어노테이션을 사용하면 해당 클래스가 Spring 컨테이너에서 빈으로 관리되며, DI(Dependency Injection)를 통해 다른 빈들과 상호작용할 수 있습니다.
2. **`@Controller`** 어노테이션:
    - **`@Controller`** 어노테이션은 웹 애플리케이션에서 HTTP 요청을 처리하는 컨트롤러 클래스에 붙입니다.
    - 이 클래스는 클라이언트의 요청을 처리하고, 비즈니스 로직을 호출하여 응답을 생성합니다.
    - 주로 MVC(Model-View-Controller) 아키텍처에서 컨트롤러 역할을 합니다.
    - **`@Controller`** 어노테이션을 사용하면 Spring이 해당 클래스를 웹 애플리케이션 컨텍스트에서 인식하고 HTTP 요청을 처리할 수 있도록 합니다.
3. **`@Repository`** 어노테이션:
    - **`@Repository`** 어노테이션은 데이터 액세스 계층에서 데이터베이스와 상호작용하는 리포지토리 클래스에 붙입니다.
    - 주로 데이터베이스 연산(CRUD)을 수행하거나 JPA(Entity Manager)와 같은 영속성 프레임워크를 사용하는 클래스에 사용됩니다.
    - **`@Repository`** 어노테이션을 사용하면 Spring은 해당 클래스를 데이터 액세스 계층의 빈으로 등록하고, 데이터베이스 연동 작업을 수행할 때 예외를 런타임 예외로 변환해줍니다.
