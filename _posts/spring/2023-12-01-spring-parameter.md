---
title: Spring에서 parameter를 받는 법
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
description: spring parameter
last_modified_at: 2023-12-01T00:00:00+08:00
---

### 1. **httpServletRequest.getParameter()**

아래소스처럼 `HttpServletRequest`의 getParameter() 메서드를 이용하여 파라미터값을 가져올 수 있다.

```java
@RequestMapping("/")
publicStringhome(HttpServletRequesthttpServletRequest){
Stringid=httpServletRequest.getParameter("id");

return"home";
}
```

### 2. @RequestParam

|이름|타입|설명|
|---|---|---|
|name, value (Alias for name)|String|파라미터 이름|
|required|boolean|해당 파라미터가 반드시 필수인지 여부, 기본값은 true|
|defaultValue|String|해당 파라미터의 기본값|

```java
@RequestMapping("/")
publicStringhome(@RequestParam(value="id",defaultValue="false")Stringid){
return"home";
}
```

### 3. @RequestBody

`@RequestBody`어노테이션을 사용할 경우 반드시 POST형식으로 응답을 받는 구조여야만 한다. 이를테면 JSON 이나 XML같은 데이터를 적절한 messageConverter로 읽을때 사용하거나, POJO형태의 데이터 전체로 받을경우에 사용된다. 단, 이 어노테이션을 사용하여 파라미터를 받을 경우 별도의 추가 설정(POJO 의 get/set 이나 json/xml 등의 messageConverter 등)을 해줘야 적절하게 데이터를 받을수가 있다.

```java
@PostMapping("/")
publicStringhome(@ReqeustBodyStudentstudent){
return"home";
}
```

### 4. @ModelAttribute

`@RequestParam`과 비슷한데 1:1로 파라미터를 받을경우는 `@RequestParam`를 사용하고, 도메인이나 오브젝트로 파라미터를 받을 경우는 `@ModelAttribute`으로 받을수 있다. 또한 이 어노테이션을 사용하면 검증(Validation)작업을 추가로 할수 있는데 예로들어 null이라던지, 각 멤버변수마다 valid옵션을 줄수가 있고 여기서 에러가 날 경우 BindException 이 발생한다.
