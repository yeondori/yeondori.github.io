---
title: "[강의] 실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화 - trouble shooting"
excerpt: "인프런 강의 복습"
categories: [Inflearn, JPA-UTIL-2]
tags: [JPA]
date: 2023-11-17
last_modified_at: 2023-11-17
render_with_liquid: false
---

---- 
# 실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화 [강의](https://www.inflearn.com/course/lecture?courseSlug=%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-API%EA%B0%9C%EB%B0%9C-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94&unitId=24316&tab=curriculum)

김영한님의 인프런 강의(실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화) 역시 1편과 마찬가지로 따라 코딩하는 게 대부분인 강의라서 강의 내용을 [github](https://github.com/yeondori/inflearn-jpashop-2)에 정리해보기로 했다.

코딩하는 중에 발생하는 오류만 여기서 따로 다뤄보고자 한다!

## 프로젝트 생성
프로젝트를 옮기자마자 에러가 발생했다..

`java: warning: source release 17 requires target release 17` 와 같은 에러인데

기존에 알던대로 Build 부분이나

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/e152fc8e-d8ba-49a9-8068-673b12fb8dde)

자바 컴파일러 부분을 확인해봐도 이상이 없었다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/46b45659-1642-4931-87be-279de4c27f8f)


정말정말 바보같게도 File -> Project Structure에서 SDK 버전이 호환되지 않는 것을 확인할 수 있었다~~! 

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/3566a0ea-8774-49a3-9992-43d1897245e1)



