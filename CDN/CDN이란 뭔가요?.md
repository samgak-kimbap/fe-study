---
layout: post
title: CDN이란 뭔가요?
author: balancelife
date: 2025-04-15 12:34:00 +0900 
categories: [CDN]
banner:
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [CDN]
---


# 📦 CDN(Content Delivery Network)

## 📌 'Delivery'란?

CDN을 이해하기 전에 먼저 'Delivery(전송)'가 뭘 의미하는지 살펴보자.

> 웹페이지, 이미지, 동영상 등 다양한 콘텐츠를 **서버에서 사용자에게 전송**하는 행위

"인터넷으로 이미 서버와 클라이언트가 연결되어 있는데 왜 굳이 전송 기술이 필요할까?"  
하는 의문이 들 수 있지만, **많은 대형 서비스들은 CDN을 반드시 사용한다.**

왜냐하면, **서버도 결국 '컴퓨터'이기 때문**이다.  
예를 들어 누군가 넷플릭스를 켜면, 그 사용자의 컴퓨터는 넷플릭스를 제공하는 **서버 컴퓨터**에 요청을 보내고, 그 요청에 따라 서버가 영상을 보내준다.

하지만...

- 수많은 요청이 **한 지점**으로 몰리면 아무리 좋은 서버여도 과부하로 **속도 저하, 장애**가 발생한다.
- 이를 방지하고, 사용자에게 **빠르고 안정적으로 콘텐츠를 제공하기 위해** CDN을 사용한다.

---

## ✅ 정의

CDN은 **Content Delivery Network(콘텐츠 전송 네트워크)**의 약자로,  
**전 세계 여러 지역에 분산된 서버**를 통해 사용자에게 **정적 콘텐츠(이미지, CSS, JS 등)를 빠르게 전송**하는 기술이다.

> 핵심은 **'분산된 서버'**!

![](https://velog.velcdn.com/images/balancelife99/post/82b7ded0-d3de-4bee-a478-533640621a7f/image.png)

---

## ✅ 주요 목적

- **지연 시간(Latency) 감소**
- **웹사이트 성능 향상**
- **서버 부하 분산**
- **트래픽 급증 시 안정성 확보**

---

## 🌐 작동 원리

1. 사용자가 웹사이트에 접속하면,
2. 사용자와 가장 가까운 **CDN 서버(= Edge Server)**가 선택된다.
3. 해당 서버가 콘텐츠를 **이미 캐시하고 있다면 즉시 제공** (cache hit)
4. 없다면 → **원본 서버(Origin Server)**로부터 받아와 캐시하고, 사용자에게 제공 (cache miss)

---

## 🧱 구성 요소

| 구성 요소         | 설명                                       |
|------------------|--------------------------------------------|
| **Origin Server** | 콘텐츠가 실제로 저장된 원본 서버            |
| **Edge Server**   | 전 세계에 분산된 캐시 서버                  |
| **PoP**           | (Point of Presence) CDN 서버가 위치한 장소 |

---

## 💾 캐싱(Caching)

### 🔹 정적 캐싱
- 미리 Edge 서버에 저장해두는 방식
- 이미지, JS, CSS 등 자주 바뀌지 않는 콘텐츠에 적합

### 🔹 동적 캐싱
- 사용자의 요청 시 캐시를 확인
  - 있다면(cache hit) → 바로 응답
  - 없다면(cache miss) → 원본 서버에서 받아와 저장 후 응답
- 실시간 데이터에 유용

> 보통 이미지나 JS/CSS 같이 **무겁지만 자주 안 바뀌는 리소스는 정적 캐싱**이 효율적이다.

---

## 🚀 장점

- **속도 향상**: 지리적으로 가까운 서버에서 콘텐츠를 받아 빠르게 응답
- **서버 부하 감소**: 많은 요청을 원본 서버가 아닌 Edge 서버가 처리
- **가용성 증가**: 원본 서버가 다운되어도 Edge 서버가 임시 응답 가능
- **보안 강화**: DDoS 공격 방어, HTTPS 인증서 제공 등의 보안 기능 포함

---

## 💡 기업이 CDN 업체를 사용하는 이유

기업들이 직접 전 세계에 서버를 구축하고 운영하는 것은 **비용, 시간, 기술력** 측면에서 매우 부담스럽다.  
그래서 전문 CDN 업체를 이용하면 아래와 같은 이점이 있다:

- ✅ **비용 절감**: 인프라 구축과 유지보수 비용을 줄일 수 있음
- ✅ **빠른 전송 속도 확보**: 글로벌 분산 서버를 통해 로딩 속도 향상
- ✅ **트래픽 확장성**: 예상치 못한 대규모 트래픽에도 안정적인 대응 가능
- ✅ **전문 보안 대응**: CDN 업체들이 DDoS 등 다양한 보안 솔루션 제공
- ✅ **운영 효율성**: 개발자가 전송 속도, 캐싱 전략에 집중할 수 있음

> 💬 **즉, 본서버만 운영할 때보다 CDN 업체를 사용하면 성능, 안정성, 보안, 유지보수 측면에서 훨씬 유리하다.**

---

## 🌍 대표적인 CDN 서비스

- [Cloudflare](https://www.cloudflare.com/)
- [Akamai](https://www.akamai.com/)
- [Amazon CloudFront (AWS)](https://aws.amazon.com/cloudfront/)


등이 있다.


