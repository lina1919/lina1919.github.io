---
title: HTTP 버전 별 차이에 대해 알려주세요
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
popular: true
categories:
- Network
toc: true
toc_sticky: true
toc_label: 목차
description: HTTP 버전 차이
last_modified_at: 2024-03-21T14:00:00+08:00
---
## HTTP란?
- Hyper Text Transfer Protocol
- 클라이언트(웹브라우저)와 서버간 데이터를 주고 받는 응용 계층의 프로토콜
- TCP 세션 기반의 데이터 전달

### HTTP 1.0

- **매번 요청마다 커넥션 수립 ( 매번 새로운 연결 )**
    - TCP 커넥션이 HTTP 요청마다 3-way Handshake와 TearDown 반복
    - 서버에 오버헤드 발생, 시간의 지연
- **커넥션 하나당 요청 하나와 응답 하나만 처리함**
- default로 사용되는 버전

### HTTP 1.1

- **커넥션 유지**
    - 한개의 TCP 세션을 통해 여러개의 컨텐츠 요청 가능
    - Pipelining 클라이언트는 응답에 상관없이 요청을 보내고 서버에서는 응답을 요청이 들어온 순서대로 보낸다. → HOL Blocking 발생
    - Mupltiple : TCP 다중 연결 , 많은 양의 objects를 검색하는 성능을 높임
- **헤더**
    - 호스트 헤더 (가상 호스팅 ) : 하나의 IP 주소에 여러 도메인 적용
    - proxy-authentication, proxy-authorization 헤더 추가 → 프록시가 사용자의 인증을 요구하는 것이 가능해짐
    - 요청과 응답이 많아지면서 중복되는 header 내용이 많아짐 → 불필요한 데이터를 주고 받아 네트워크 자원 낭비 발생

> 💡 HOL (Head Of Line) Blocking
> 
> 응답을 요청을 받은 순서대로 보내야 하기 때문에, 앞의 요청에 대한 응답이 지연되면 그 뒤의 응답에 대해 blocking이 발생할 수 있다.

### HTTP 2.0

1) **HTTP 메시지 전송 방식의 변화**

- 1.0의 text 형식 전달에서 요청, 응답 메시지는 프레임 단위로 나누어지고 **바이너리 형식**으로 인코딩으로 변경
- 파싱, 전송 속도가 빨라짐

1. HOL Blocking 문제 개선

- **멀티플렉스 스트림 (Multiplexed Streams) : 요청과 응답의 다중화**
    - 한 커넥션으로 동시에 여러개의 메시지를 주고 받고, 응답은 순서에 상관없이 stream으로 주고 받는다.
    - Stream Prioritization 응답에 대한 우선 순위를 정해 우선 순위가 높을 수록(먼저한 필요한 리소스 부터) 응답을 빨리한다.
    - 클라이언트가 요청하기 전에 필요하다고 예상되는 리소스를 서버에서 먼저 보내준다. 서버는 클라이언트의 요청에 대해 요청하지 않은 리소스를 마음대로 보내줄 수 있다.

![https://velog.velcdn.com/images/sweet_sumin/post/0c91d0dd-f4c5-48e5-bac3-64f119816361/image.png](https://velog.velcdn.com/images/sweet_sumin/post/0c91d0dd-f4c5-48e5-bac3-64f119816361/image.png)

1. Header 중복 해결

- **Header table과 Huffman Encoding 기법을 이용해 압축한다.** 클라이언트와 서버는 각각 Header Table을 관리하고 이전 요청과 동일한 필드는 table의 index만 보내고 변경되는 값은 Huffman Encoding 후 보냄으로써 Header의 크기를 경량화 한다. → 헤더의 크기를 줄여 페이지 로드 시간 감소

### HTTP 3

- 전송 계층에 스트림을 기본 구조로 도입하는 새 인터넷 전송 프로토콜인 UDP를 기반 QUIC을 사용한다. -- 새 스트림을 만들 때 추가적인 핸드쉐이크나 슬로우 스타트가 필요하지 않음 (스트림 연결과 암호화 스펙등을 포함한 모든 핸드쉐이크가 단일 요청/응답으로 끝난다) -- UIC 스트림은 독립적으로 전달되어 어떤 스트림에 패킷 손실이 있는 경우에도 다른 스트림에는 영향이 없음## HTTP 버전 별 차이에 대해 알려주세요

- Hyper Text Transfer Protocol
- 클라이언트(웹브라우저)와 서버간 데이터를 주고 받는 응용 계층의 프로토콜
- TCP 세션 기반의 데이터 전달

### HTTP 1.0

- **매번 요청마다 커넥션 수립 ( 매번 새로운 연결 )**
    - TCP 커넥션이 HTTP 요청마다 3-way Handshake와 TearDown 반복
    - 서버에 오버헤드 발생, 시간의 지연
- **커넥션 하나당 요청 하나와 응답 하나만 처리함**
- default로 사용되는 버전

### HTTP 1.1

- **커넥션 유지**
    - 한개의 TCP 세션을 통해 여러개의 컨텐츠 요청 가능
    - Pipelining 클라이언트는 응답에 상관없이 요청을 보내고 서버에서는 응답을 요청이 들어온 순서대로 보낸다. → HOL Blocking 발생
    - Mupltiple : TCP 다중 연결 , 많은 양의 objects를 검색하는 성능을 높임
- **헤더**
    - 호스트 헤더 (가상 호스팅 ) : 하나의 IP 주소에 여러 도메인 적용
    - proxy-authentication, proxy-authorization 헤더 추가 → 프록시가 사용자의 인증을 요구하는 것이 가능해짐
    - 요청과 응답이 많아지면서 중복되는 header 내용이 많아짐 → 불필요한 데이터를 주고 받아 네트워크 자원 낭비 발생

> 💡 HOL (Head Of Line) Blocking
> 
> 응답을 요청을 받은 순서대로 보내야 하기 때문에, 앞의 요청에 대한 응답이 지연되면 그 뒤의 응답에 대해 blocking이 발생할 수 있다.

### HTTP 2.0

1) **HTTP 메시지 전송 방식의 변화**

- 1.0의 text 형식 전달에서 요청, 응답 메시지는 프레임 단위로 나누어지고 **바이너리 형식**으로 인코딩으로 변경
- 파싱, 전송 속도가 빨라짐

1. HOL Blocking 문제 개선

- **멀티플렉스 스트림 (Multiplexed Streams) : 요청과 응답의 다중화**
    - 한 커넥션으로 동시에 여러개의 메시지를 주고 받고, 응답은 순서에 상관없이 stream으로 주고 받는다.
    - Stream Prioritization 응답에 대한 우선 순위를 정해 우선 순위가 높을 수록(먼저한 필요한 리소스 부터) 응답을 빨리한다.
    - 클라이언트가 요청하기 전에 필요하다고 예상되는 리소스를 서버에서 먼저 보내준다. 서버는 클라이언트의 요청에 대해 요청하지 않은 리소스를 마음대로 보내줄 수 있다.

![https://velog.velcdn.com/images/sweet_sumin/post/0c91d0dd-f4c5-48e5-bac3-64f119816361/image.png](https://velog.velcdn.com/images/sweet_sumin/post/0c91d0dd-f4c5-48e5-bac3-64f119816361/image.png)

1. Header 중복 해결

- **Header table과 Huffman Encoding 기법을 이용해 압축한다.** 클라이언트와 서버는 각각 Header Table을 관리하고 이전 요청과 동일한 필드는 table의 index만 보내고 변경되는 값은 Huffman Encoding 후 보냄으로써 Header의 크기를 경량화 한다. → 헤더의 크기를 줄여 페이지 로드 시간 감소

### HTTP 3

- 전송 계층에 스트림을 기본 구조로 도입하는 새 인터넷 전송 프로토콜인 UDP를 기반 QUIC을 사용한다. -- 새 스트림을 만들 때 추가적인 핸드쉐이크나 슬로우 스타트가 필요하지 않음 (스트림 연결과 암호화 스펙등을 포함한 모든 핸드쉐이크가 단일 요청/응답으로 끝난다) -- UIC 스트림은 독립적으로 전달되어 어떤 스트림에 패킷 손실이 있는 경우에도 다른 스트림에는 영향이 없음
