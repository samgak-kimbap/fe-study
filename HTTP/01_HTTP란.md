## HTTP란?

**HTTP(HyperText Transfer Protocol, 하이퍼텍스트 전송 프로토콜)**은 웹에서 데이터를 주고받을 때 사용하는 **통신 규약(Protocol)**입니다.  
우리가 브라우저에서 웹사이트를 방문할 때, HTTP를 사용하여 웹 서버와 데이터를 주고받습니다.

---

## HTTP의 주요 특징

1. **클라이언트-서버 구조**  
   - 클라이언트(예: 웹 브라우저)가 요청을 보내면, 서버가 응답을 반환하는 방식으로 동작합니다.

2. **무상태(Stateless)**  
   - 각 요청은 독립적이며, 서버는 이전 요청의 정보를 저장하지 않습니다.  
     *(단, 쿠키, 세션 등을 활용하면 상태를 유지할 수 있음)*

3. **TCP/IP 기반**  
   - HTTP는 기본적으로 TCP(Transmission Control Protocol) 위에서 동작하며, 안정적인 데이터 전송을 보장합니다.

4. **메시지 기반 통신**  
   - HTTP 요청(Request)과 응답(Response)은 각각 **메서드(Method), 헤더(Header), 본문(Body)** 등의 형식으로 구성됩니다.

---

## HTTP 요청(Request) 구성 요소

- **메서드(Method)**: `GET`, `POST`, `PUT`, `DELETE` 등 요청의 종류를 정의
- **URL**: 요청 대상 리소스 주소
- **헤더(Header)**: 요청에 대한 추가 정보 (예: 인증 정보, 데이터 형식 등)
- **본문(Body)**: 요청 시 전달할 데이터 (`POST`, `PUT` 요청에서 주로 사용)

### 예제 (GET 요청)

```http
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0
```

## HTTP 응답(Response) 구성 요소

- **상태 코드(Status Code)**: 요청 결과를 나타내는 숫자 코드 (200 OK, 404 Not Found 등)
- **헤더(Header)**: 응답 관련 추가 정보 (예: 콘텐츠 타입, 서버 정보 등)
- **본문(Body)**: 응답 데이터 (HTML, JSON 등)

### 예제 (응답)

```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1024

<html>
  <body>Hello, world!</body>
</html>
```

## HTTP vs. HTTPS
| 구분  | 설명 |
|------|------|
| HTTP | 데이터를 암호화하지 않고 전송 -> 보안에 취약 |
| HTTPS | SSL/TLS 암호화를 적용하여 보안 강화 |


## HTTP 메서드 종류
| 매서드  | 설명 |
|------|------|
| GET | 리소스 요청 (조회)|
| POST | 리소스 생성 |
| PUT | 리소스 전체 수정 |
| PATCH | 리소스 일부 수정 |
| DELETE | 리소스 삭제 |


## 요약
- HTTP는 웹에서 데이터를 주고받기 위한 프로토콜
- 클라이언트-서버 구조, 무상태성, TCP/IP 기반
- 요청(Request)과 응답(Response)으로 동작
- 보안을 위해 HTTPS 사용 권장