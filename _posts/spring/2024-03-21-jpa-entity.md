---
title: JPA Entity 객체 생명주기
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
popular: true
categories:
- Spring
- JPA
toc: true
toc_sticky: true
toc_label: 목차
description: jpa entity
last_modified_at: 2024-03-21T02:00:00+08:00
---

![](https://i.imgur.com/Wcr2c9E.png)

비영속(new) : 순수한 객체 상태, 영속성 컨텍스트와는 관련이 없는 상태

영속(managed) : new 상태에서 persist() 메소드를 이용해 저장한 경우와 DB에 저장되어 있는 데이터를 find 메소드 또는 쿼리를 사용해 조회한 경우에 Entity 객체는 영속 상태가 된다.

준영속(detached) : 트랜잭션이 commit 되었거나, clear, flush 메소드가 실행된 경우 managed 상태의 객체는 모두 Detached 상태가 된다. 이 상태는 더 이상 DB와 동기화를 보장하지 않고 다시 Managed 상태로 만들기 위한 merged 메소드가 존재한다.

삭제(removed) : managed 상태의 객체를 remove 메소드를 이용해 삭제한 경우, removed 상태에 해당한다. 작업 단위가 종료되는 시점에 실제로 DB 테이블에 삭제가 동기화 된다. 이 상태에 객체는 종료되는 동시에 DB에서 삭제되므로 재사용하면 안된다.

실제 DB에 반영되는 시점이 ORM에서는 다르기 때문에 이를 이해하고 있어야 한다.
