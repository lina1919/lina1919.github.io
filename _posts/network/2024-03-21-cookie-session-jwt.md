---
title: 쿠키, 세션, JWT에 대해 설명해주세요
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
description: 쿠키, 세션, JWT
last_modified_at: 2024-03-21T10:00:00+08:00
---
- 쿠키 : 사용자 정보와 같은 데이터를 브라우저에 저장하여 유지하고
- 세션 : 서버에서 해당 데이터를 보관
- JWT : JSON 형식의 토큰 기반 인증 시스템

## 쿠키 생성 과정

쿠키의 생성과 저장은 구현에 따라 다르지만, 원리는 동일합니다. 생성 과정부터 말씀드리면,

1. **서버가 클라이언트로부터 요청을 받았을 때**, 클라이언트에 관한 정보를 토대로 쿠키를 구성합니다.
2. **서버는 클라이언트에게 보내는 응답의 header에 쿠키를 담아 보냅니다.**
3. 클라이언트가 응답을 받으면, **브라우저는 쿠키를 Cookie directory에 저장**합니다.

추가로, 쿠키는 클라이언트(브라우저)에 **key-value 쌍의 파일 형태로 저장**됩니다. 즉, **유효시간 내에서는 브라우저가 종료 되어도 계속 유지**됩니다. 서버에서 **response header**에 **set-coockie**속성을 사용해서 Cookie를 만들어 보내고, 사용자가 따로 작업 하지 않아도 **브라우저가 쿠키를 request header에 담아서 서버에 전송**합니다.

## 세션

세션은 기본적으로 **쿠키를 이용하여 구현**됩니다. **Client를 구분**하기 위해 **각 Client에게 session ID를 부여**하고 **Client는 쿠키에 session ID를 저장**합니다. **사용자 정보를 브라우저에 저장하는 쿠키와 달리 세션은 서버측에 저장하여 관리**합니다. **세션은 유효시간을 두어 일정 시간 응답이 없다면 끊을 수 있고**, **브라우저가 종료될 때까지 인증상태를 유지**할 수 있습니다. **사용자 정보를 서버에 두기 때문에 쿠키보다 보안은 좋지만, 서버 자원을 차지하기 때문에 서버에 과부하**를 줄 수 있고 **성능 저하의 요인**이 될 수 있습니다.

## 쿠키와 세션을 사용하는 이유와 그 차이점에 대해 말해주세요

HTTP의 **connectionless(비연결성)**, **stateless(비상태성)**라는 특징 때문입니다. Client가 요청을 했을 때 그 요청에 맞는 응답을 보낸 후 연결을 끊고, Server는 Client에 대한 상태 정보를 유지하지 않기 때문에 알 수 없습니다. 하지만 쿠키와 세션을 적절하게 사용한다면, Client의 상태 정보를 임시 저장하여 특정 기간 동안 사용할 수 있습니다.

**만약 쿠키와 세션을 사용하지 않는다면**, 로그인 했음에도 불구하고 페이지를 이동할 때마다 계속 로그인을 해야하며, 장바구니, “오늘 더 이상 이 창을 보지 않음” 등의 편의성을 제공할 수 없습니다.

**쿠키**는 **클라이언트(브라우저) 로컬에 key-value 쌍으로 저장**되는 데이터 파일입니다. **유효시간 내에서는 브라우저가 종료되어도 계속 유지**됩니다.

**세션**은 **브라우저가 종료**되거나, **서버에서 해당 세션을 삭제할 수 있기 때문에 쿠키보다 보안성이 좋습니다.** 또한 **서버에 데이터를 저장하므로 서버 용량이 허용하는 한에서 제한 없이 데이터를 저장**할 수 있다는 장점이 있지만, **서버의 부하가 커진다는 단점**이 있습니다.

### 세션 없이 쿠키만 존재한다면? 세션이 보안도 좋은데 왜 쿠키를 사용할까요?

유출 문제가 발생했을 때 **서버에서 해당 세션을 제거함으로써 해당 상황을 모면하는 것이 불가능**할 것으로 보입니다. – **invalidate**

세션은 서버의 자원을 사용하기 때문에 서버가 느려질 수 있고, 서버 자원이 부족할 수 있습니다. **서버 자원의 낭비를 방지하고 속도를 높히기 위해 상황에 따라 쿠키를 사용**합니다.

## JWT에 대해 설명해주세요

JWT는 JSON 포맷을 이용하는 Claim 기반의 웹 토큰이며, 토큰 자체를 정보로 사용하는 Self-Contained 방식으로 정보를 안전하게 전달합니다.

JWT는 헤더(Header).내용(Payload).서명(Signature)로 구성되며 각 파트를 점(.)으로 구분합니다.

- 장점으로는 인증 정보에 대한 **별도의 저장소가 필요 없으며(서버의 무상태)** 자체적으로 **위변조**를 막을 수 있습니다. 또한 **다른 로그인 시스템에 접근 및 권한 공유가 가능하므로 확장성이 우수**합니다.
- 단점으로는, **쿠키와 세션보다 길이가 길기 때문에, 요청이 많아질 수록 네트워크 부하가 심해지며**, **Payload는 암호화되지 않기 때문에 중요 정보는 담을 수 없습니다.** 추가로 **토근은 유효기간이 만료될 때까지 계속 사용**할 수 있기 때문에, **탈취 시 대처가 어렵습니다.**

### JWT 토큰을 사용할 경우 길이가 길어지면 서버에 부하가 올 수 있다. 이 때 해결 방법은?

JWT (JSON Web Token)의 토큰 길이가 길어질 경우, 네트워크 트래픽과 서버 부하에 영향을 미칠 수 있습니다. 특히, 매 요청마다 토큰을 전송해야 하는 경우에는 이러한 부하가 더욱 중요해집니다 이러한 부하를 줄이기 위해서는 JWT 토큰의 길이를 줄여야 합니다

- 토큰 클레임 최소화
- GZIP이나 DEFLATE와 같은 압축 알고리즘 사용
- 서버 측에서 Redis나 Memcached와 같은 분산 캐시를 사용하여 토큰을 저장하고, 필요할 때 검색해서 사용
    - 토큰 ID를 생성하고 이 ID와 값을 Redis에 저장
    - 클라이언트에게는 토큰 ID만 전송하고, 서버는 이를 사용하여 Redis에서 토큰을 불러옴
- 토큰을 여러 부분으로 나누고 필요한 부분만 전송 : 헤더와 페이로드를 별도로 보내고, 필요한 경우 서버에서 병합합니다
- CDN 및 캐시 : 토큰을 캐싱하고 CDN을 통해 전송
    - 클라이언트가 토큰을 요청할 때 CDN은 해당 토큰을 캐싱
    - 동일한 토큰 요청이 발생할 때 캐시된 토큰 리턴

## 세션 기반 인증과 토큰 기반 인증의 차이에 대해 얘기해주세요

세션 기반 인증은 클라이언트로부터 요청을 받으면 클라이언트의 상태 정보를 저장하므로 Stateful한 구조를 가지고,

토큰 기반 인증은 상태 정보를 서버에 저장하지 않으므로 Stateless한 구조를 가집니다.

### **💡 그렇다면 Stateful한 세션 기반의 인증 방식을 사용하게 된다면 어떠한 단점이 있을까요?**

1. 서버에 세션을 저장하기 때문에 사용자가 증가하면 서버에 과부하를 줄 수 있어 확장성이 낮습니다.
    
2. 해커가 훔친 쿠키를 이용해 요청을 보내면 서버는 올바른 사용자가 보낸 요청인지 알 수 없습니다. (세션 하이재킹 공격)
    

### **💡 그렇다면 세션 기반 인증과 토큰 기반 인증은 각각 어느 경우에 적합한가요?**

단일 도메인이라면 세션 기반 인증을 사용하고, 아니라면 토큰 기반 인증을 사용하는 것이 적합하다고 생각합니다.

왜? - 세션을 관리할 때 사용되는 쿠키는 단일 도메인 및 서브 도메인에서만 작동하도록 설계되어 있기 때문에 여러 도메인에서 관리하는 것은 어렵습니다. (CORS 문제)

### **쿠키와 토큰을 사용할 때 CORS (Cross-Origin Resource Sharing) 문제를 어떻게 해결할 수 있을까요?**

CORS 문제는 클라이언트와 서버가 다른 도메인을 가지고 있을 때 발생하는 문제입니다. 토큰과 쿠키 둘 다 이 문제를 가질 수 있지만, 해결 방법이 조금 다릅니다.

1. **쿠키 사용시**:
    
    - 서버측에서 CORS 헤더를 설정하여 쿠키를 다른 도메인에서도 사용할 수 있도록 할 수 있습니다.
    - **`Access-Control-Allow-Credentials`**을 **`true`**로 설정해야 하며, 이 경우 **`Access-Control-Allow-Origin`**은 와일드카드(**)를 사용할 수 없습니다. 정확한 도메인을 명시해야 합니다.
2. **토큰 사용시**:
    
    - 토큰은 보통 HTTP 헤더에 넣어서 전달하기 때문에 CORS 문제를 해결하는 것이 상대적으로 쉽습니다.
    - 서버에서 **`Access-Control-Allow-Origin`** 헤더를 설정하면 특정 도메인 또는 여러 도메인에서의 접근을 허용할 수 있습니다.

CORS 문제를 해결하기 위해서는 서버에서 적절한 CORS 헤더를 설정해주는 것이 핵심입니다. 이렇게 하면, 클라이언트가 다른 도메인에서도 리소스에 접근할 수 있게 됩니다.

여러 도메인에서 서비스를 제공해야 한다면, 토큰 기반 인증이 CORS 문제를 해결하기 더 간편할 수 있습니다. 토큰은 HTTP 헤더를 통해 전달되므로, 쿠키와는 달리 다른 도메인에서도 쉽게 사용할 수 있습니다.