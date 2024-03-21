---
title: Spring vs Spring Boot
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
description: Spring vs Spring Boot
last_modified_at: 2024-03-21T00:00:00+08:00
---
## Spring

- Spring은 스프링 프레임워크의 핵심 모듈을 모아서 만든 프레임워크
- Spring에서는 개발자가 직접 설정 파일을 작성하여 스프링 컨테이너를 구성하고, 필요한 빈 객체를 등록하고, 빈 객체 간의 의존성을 설정해야함
- Spring은 특정한 구성을 위해 추가적인 라이브러리와 설정이 필요

## Spring Boot

- Spring Boot는 스프링 프레임워크를 보다 쉽게 사용할 수 있도록 만든 프레임워크
- Spring Boot에서는 개발자가 설정 파일을 작성할 필요 없이, 프로젝트의 설정과 라이브러리 의존성을 자동으로 처리해주는 기능 제공
- 실행 가능한 JAR 파일을 만들 수 있다
- 내장 서버(톰캣)을 제공하여 웹 애플리케이션을 빠르게 실행할 수 있다
- Spring Boot는 Spring에서 제공하는 여러 기능들을 자동으로 설정하여 개발자가 보다 쉽게 사용할 수 있도록 해준다
- 예를 들어, Spring Boot는 스프링 MVC, 스프링 Data JPA, 스프링 Security 등의 기능을 자동으로 설정하며, 개발자가 별도로 설정 파일을 작성하지 않아도 사용할 수 있다. 또한, Spring Boot는 Actuator라는 모니터링과 관리를 위한 기능을 제공하여, 애플리케이션의 상태를 모니터링하고, 필요한 조치를 취할 수 있도록 해준다

Spring은 스프링 프레임워크를 보다 세밀하게 제어하고자 하는 경우에, Spring Boot는 빠르고 간단하게 스프링 애플리케이션을 개발하고자 하는 경우에 사용!
