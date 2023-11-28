---
title: "[강의] Spring Cloud로 개발하는 마이크로서비스 애플리케이션(MSA) - 02 API Gateway Service "
excerpt: "인프런 강의 복습"
categories: [Inflearn, Spring Cloud로 개발하는 마이크로서비스 애플리케이션(MSA)]
tags: [Spring Cloud, MSA]
date: 2023-11-28
last_modified_at: 2023-11-28
render_with_liquid: false
---

---- 

# Spring Cloud로 개발하는 마이크로서비스 애플리케이션(MSA) [강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4?gad_source=1&gclid=CjwKCAiAmZGrBhAnEiwAo9qHiV1UdD8oI92U-7gfKB3rk-3hG9lMtbJH4htJYaOez-8_NbLY68I9xhoCd08QAvD_BwE)

## 섹션 2. API Gateway Service

### API Gateway
클라이언트 측에서 마이크로서비스의 주소를 이용해 end point를 직접 호출할 경우에, 마이크로서비스의 주소값, 파라미터 인자 등이 변경되면 클라이언트 사이드의 어플리케이션도 수정 배포가 되어야 한다.  
단일 진입점을 가진 형태로 개발해야 할 필요성이 생겼고, **API Gateway** 를 통해 각각의 마이크로서비스로 요청되는 모든 정보에 대해 일괄적으로 처리할 수 있게 하여 변경, 갱신 작업을 수월하게 할 수 있다!

API Gateway 서비스로 다음의 작업을 수행할 수 있다.

-인증 및 권한 부여  
-서비스 검색 통합  
-응답 캐싱  
-정책,회로 차단기 및 Qos 다시 시도  
-속도 제한  
-부하 분산  
-로깅, 추적, 상관 관계  
-헤더, 쿼리 문자열 및 청구 반환  
-IP 허용 목록에 추가  

### Netflix Ribbon

Spring Cloud에서 MSA간 통신을 위해 사용하는 방법을 크게 두 가지로 구분할 수 있다.

1. **RestTemplate**

    전통적인 방식으로, 인스턴스를 생성해 접속하고자하는 서버의 주소(+ 포트번호)를 입력하고 메소드를 호출하는 방식이다. 


2. **Feign Client**
    
    Spring Cloud에서는 Feign Client api를 이용할 수 있다. 특정한 인터페이스를 생성하고, 인터페이스에서 웹으로 호출하고자 하는 추가적인 마이크로 서비스의 이름(Feign Client)을 등록한다.
    자유롭게 api를 호출해 사용할 수 있게 된다.

초창기의 Spring Cloud에서는 load balancer 역할을 해주는 별도의 서비스 프로젝트를 위해 Ribbon이라는 서비스를 제공했다.  
Ribbon은 클라이언트 측 내부에 구축해 사용하며, 마이크로서비스의 이름만으로도 api를 사용할 수 있게 된다.  
문제는 이 Ribbon 서비스가 비동기 처리가 잘 되지 않는 방식이라는 것이다. 최근에는 잘 이용하고 있지는 않다. 

## Netflix Zuul

API Gateway 역할을 하는 제품이다. 클라이언트는 Netflix zuul을 통해 서비스를 요청한다.
zuul 역시 Ribbon과 마찬가지로 Maintenance 상태이다. 

