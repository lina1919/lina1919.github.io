---
title: Network Layer의 2가지 Routing Protocol(Link State vs Distance Vector)
layout: single
author_profile: true
read_time: true
related: true
categories:
- Network
toc: true
toc_sticky: true
toc_label: 목차
description: Link State vs Distance Vector
meta_keywords: java
last_modified_at: '2023-11-19 23:00:00 +0800'
---
## Network Layer의 2가지 역할
1. data plane
	1. forwarding
2. control plane
	1. routing

### Control Plane
- 옛날에는 control plane과 data plane이 합쳐짐
- 최근에는 control plane과 data plane을 분리(SDN, CA)

## Routing Protocol
- routing은 network에서 매우 중요하게 다뤄지는 문제
- packet을 보낼 때 good path를 결정하는 것이 매우 중요

### Link State
- Dijkstra Algorithm
- 모든 경로가 알려져있다.
- oscillation(진동) 문제 발생 가능

### Distance Vector
- Bellman-Ford equation Algorithm(DP)
- 반복적(iterative), 비동기적
- 분산적(dv가 바뀔 때에만 이웃에게 고지)
- count to infinity problem 발생 가능
	- 파급력 MAX
	- bad news travels slow

 > TOPCIT에 이걱 관련 문제가 나왔는데 기억이 안나는 것을 보고 충격받아 정리합니다
![](https://i.imgur.com/wpJVqh0.jpg)
