---
layout: post
title: HTTP&HTTPS
author: MJ 
date: 2025-03-27 16:37:00 +0900 
categories: [HTTP]
banner:
  image: https://velog.velcdn.com/images/balancelife99/post/0fbf0ec8-bef0-484c-9668-1a74b4563f2e/image.png
  opacity: 0.618
  background: "#000"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold; text-decoration: underline"
  subheading_style: "color: gold"
tags: [HTTP, HTTPS]
---

![](https://velog.velcdn.com/images/balancelife99/post/0fbf0ec8-bef0-484c-9668-1a74b4563f2e/image.png)

HTTPS 를 알아보기 전 HTTP 를 간단하게 알아보자.

## HTTP

HTTP는 **H**yper**t**ext **T**ransfer **P**rotocol 이라고 클라이언트와 서버 간 통신을 위한 통신 프로토콜(규칙)이다.

사용자가 웹 사이트를 방문하면 사용자의 브라우저가 HTTP 요청을 전송하고 웹 서버는 HTTP 응답으로 응답한다.

자 그렇다면 HTTPS 란?

## HTTPS

HTTPS는 **H**yper**t**ext **T**ransfer **P**rotocol **S**ecure 로 뒤에 **S**ecure 가 더 붙었다.
HTTP에 데이터 암호화가 추가된 프로토콜이다. 

HTTP 에서는 80번 포트를 사용하여 데이터를 주고 받도록 약속되어있는 방면
HTTPS 에서는 443번 포트를 사용한다. 

## HTTP와 HTTPS

#### 보안
HTTP는 서버에서 브라우저로 전송되는 정보가 암호화되지 않는다는 문제가 있다.

즉 권한이 없는 사람이 데이터를 보려고 마음먹는다면 데이터를 쉽게 접근하고 볼 수 있다.

반면 HTTPS는 모든 데이터를 SSL(보안 소켓 계층)을 사용하여 암호화된 형태로 전송한다. 
제 3자가 네트워크를 통해 해당 데이터를 가로챌 수 없음을 확신할 수 있다!

신용카드 세부 정보 나 고객 개인정보 등 민감한 정보를 보호하기 위해 HTTPS를 선택해야한다.

#### 검색엔진
검색 엔진은 HTTP의 신뢰성이 더 낮기 때문에 HTTP 웹 사이트 콘텐츠의 노출 순위를 HTTPS 웹 페이집다 더 낮게 지정한다. 고로 HTTPS를 사용하면 검색엔진 최적화(SEO) 에 있어서도 유리하다.

#### 성능
HTTPS 웹 앱은 HTTP 웹 앱보다 로드속도가 더 빠르다.