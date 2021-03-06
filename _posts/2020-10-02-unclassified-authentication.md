---
layout: post
title: "[Web] Authentication"
date: 2020-10-02 19:02:00 +0900
categories: unclassified
---

# Authentication

> Def. 인증, 권한/신분 확인

## Web 에서의 Authentication

Web은 HTTP 프로토콜을 따른다. HTTP 프로토콜은 **Connectionless, Stateless**라는 특징을 가지고 있어, 응답 후 연결이 끊기게 되며 상태 정보를 기억하지 않는다. 따라서 각 HTTP 요청마다 주체(요청을 보내는 자)의 정보가 필수적이다. 따라서 HTTP 요청의 헤더에 인증 수단을 넣어 보내야 한다.
이 인증 수단이 어떻게 발전해 나아갔는지 정리해보도록 한다.

## 1. 정보 자체를 헤더에 넣는 방식

HTTP 요청 헤더에 사용자의 인증 정보를 담아 보내는 방식이다. (물론 실서비스에서는 절대 사용해서는 안된다) 클라이언트측에서 쿠키에 사용자의 인증 정보를 저장하고 있다가, 인증이 필요한 요청 시마다 요청 헤더에 담아 전송한다.

## 2. Session/Cookie

서버는 클라이언트로부터 온 인증 정보(id, pw)를 읽어 고유한 Session ID를 발급하고, FE에서는 이 Session ID를 쿠키에 저장해 인증이 필요한 요청마다 쿠키를 요청 헤더에 담아 전송하는 방식이다. 세션 저장소를 필요로 하고, 보통 Redis를 많이 사용한다.

### 순서

1. 사용자가 로그인을 하면 인증 정보를 담은 HTTP 요청을 보낸다.
2. 서버에서 인증 정보를 읽어 사용자의 존재 여부를 확인한 뒤, 고유한 Session ID를 발급해 세션 저장소에 저장하고 사용자에게 이 ID를 HTTP 응답에 넣어 전송한다.
3. 사용자는 응답으로 받은 Session ID를 쿠키에 저장하여, 인증이 필요한 요청마다 쿠키(Session ID)를 요청 헤더에 담아 보낸다.
4. 서버에서 쿠키를 받아 세션 저장소에 존재하는 지 확인한 후, 응답을 보낸다.

### 장점

- 사용자는 Session ID만을 가지고 있기 때문에 이 값이 노출되어도 계정 정보를 탈취당하지 않는다.
- Session ID로 세션 저장소를 확인하여 사용자를 확인하기 때문에 서버 자원 접근에 용의하다.

### 단점

- 장점 1번에서 말했던 Session ID를 탈취당한 상황에서, 이 ID를 이용해 요청을 보낸다면 사용자로 오인하여 정보를 주게 된다. 이를 세션 하이재킹 공격이라고 한다.
  - 해결책 1. HTTPS를 사용해 요청 자체를 탈취해도 안의 정보를 읽기 힘들게 한다.
  - 해결책 2. 세션에 유효시간을 넣어준다.
- 세션 저장소라는 추가적인 공간이 필요하다.

## 3. 토큰 기반 인증 방식 (JWT)

JWT(Json Web Token)는 인증에 필요한 정보들을 암호화시킨 토큰을 뜻한다. 사용자는 Access Token(JWT token)을 요청 헤더에 담아 서버로 전송한다.

### JWT

크게 세 가지 파트로 나뉜다.

1. **Header** Algorithm(암호화 알고리즘), Token type
2. **Payload** 서버에 보낼 Data. 일반적으로 user Id, 유효기간 등이 들어간다.
3. **Verify Signature** Base64 방식으로 인코딩한 Header, payload, Secret key를 더한 후 서명된다.

최종적으로 토큰은 다음과 같은 값을 가진다.

```js
Encoded Header + "." + Encoded Payload + "." Verify Signature
```

Header, Payload는 따로 암호화되지 않는다. (인코딩 될 뿐) 따라서 Payload에 비밀번호같은 중요한 정보가 들어가서는 안된다.
하지만 Verify Signature는 SECRET KEY를 알지 못하면 복호화할 수 없다. 따라서 각 사용자의 Verify Signature를 알지 못하면 토큰을 조작할 수 없다.

### 순서

1. 사용자가 로그인을 하면 인증 정보를 담은 HTTP 요청을 보낸다.
2. 서버에서 인증 정보를 읽어 사용자의 존재 여부를 확인한 뒤, 사용자의 고유한 ID와 기타 정보를 함께 Payload에 넣는다.
3. JWT의 유효기간을 설정한다.
4. 암호화할 SECRET KEY를 이용해 Access token을 발급하여 사용자에게 응답을 보낸다.
5. 사용자는 Access token을 받아 저장하여, 인증이 필요한 요청마다 토큰을 요청 헤더에 담아 보낸다.
6. 서버에선 해당 토큰의 Verify Signature를 Secret key로 복호화한 후, 조작 여부와 유효 기간을 확인한다.
7. 검증이 완료되면, Payload를 디코딩하여 사용자의 ID에 맞는 데이터를 가져온다.

### Session/Cookie 방식과의 차이점

Session/Cookie는 세션 저장소에 유저의 정보를 넣는 반면, JWT는 토큰 안에 유저의 정보들을 넣는다는 점이다. 클라이언트 입장에서는 다를 바가 없지만, 서버 측에서는 인증을 위해 암호화를 하냐, 별도의 저장소를 이용하냐의 차이가 발생한다.

### 장점

- 별도의 저장소 관리가 필요없다.
- 확장성이 뛰어나다. Oauth 같은 인증 시스템이 토큰을 기반으로 하기 때문에 접근이 가능하다.

### 단점

- 이미 발급된 JWT에 대해 돌이킬 수 없다. Session/Cookie의 경우 쿠키가 악의적으로 이용된다면 세션을 지우면 되지만, JWT는 발급된 뒤 유효기간이 완료될 때까지 사용이 계속 가능하다.
  - 해결책 1. Access Token의 유효기간을 짧게 하고 Refresh Token이라는 새로운 토큰을 발급한다.
- Payload 정보가 제한적이다. 암호화되지 않기 때문에 중요한 정보를 넣을 수 없다.
- JWT의 길이가 길다. 인증이 필요한 요청이 많을수록 서버의 자원낭비가 발생한다.

## Reference

https://tansfil.tistory.com/58
