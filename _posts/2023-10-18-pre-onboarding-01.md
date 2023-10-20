---
title: "[프로젝트]원티드 프리온보딩 과제 수행 01 요구사항 정리 - 초기 환경 설정"
excerpt: "원티드 프리온보딩 과제 수행 과정 기록"
categories: [Spring, Pre-onboarding]
tags: [Spring]
date: 2023-10-18
last_modified_at: 2023-10-18
render_with_liquid: false
---
# [프리온보딩 인턴십 수행과제](https://bow-hair-db3.notion.site/1850bca26fda4e0ca1410df270c03409)

며칠간 이 수행 과제를 하느라 강의 수강, 백준 문제풀기, 개발 서적 읽기, 블로그 올리기가 모두 중단되었다...
~~내 잔디 돌려조,,,~~

**'할 수 있을 것 같은데?'** 로 시작했다가 **'할 수 있나..?'** 가 되고 결국 마감 기한에 겨우 맞춰서  **'한 건가...?'** 로 끝난 나의 첫 스프링 프로젝트. 지금 정리해두지 않으면 또 미뤄둘 것 같아서 제출하자마자 정리해보는 중!

프로젝트는 [🚀여기🚀](https://github.com/yeondori/wanted-pre-onboarding-backend)에서 확인할 수 있따.
요구사항과 시연 모두 조잡하지만 나름 README에 심혈을 기울여 작성해 두었다.
아래에서 언급하는 트러블슈팅 관련해서는 [[프로젝트]원티드 프리온보딩 과제 수행 03](https://yeondori.github.io/posts/pre-onboarding-03/) 여기서 한번에 다루고 있다!

## 과제 설명

### 👆🏻프로젝트 요구사항

아래의 서비스 개요 및 요구사항을 만족하는 API 서버를 구현한다.

#### 서비스 개요

- 본 서비스는 기업의 채용을 위한 웹 서비스이다.
- 회사는 채용공고를 생성하고, 이에 사용자는 지원한다.

#### 서비스 요구사항

1. 채용공고 등록
2. 채용공고 수정 (회사 id 이외 모두 수정 가능)
3. 채용공고 삭제
4. 채용공고 목록 가져오기

   ➡️ 사용자는 채용공고 목록을 아래와 같이 확인할 수 있다.

   > Example)
   > 데이터 예시이며, 필드명은 임의로 설정가능합니다.
   > {
   > "회사_id":회사_id,
   > "채용포지션":"백엔드 주니어 개발자",
   > "채용보상금":1000000,
   > "채용내용":"원티드랩에서 백엔드 주니어 개발자를 채용합니다. 자격요건은..",
   > "사용기술":"Python"
   > }
   >

   ➡️ 채용공고 검색 기능 구현(선택사항 및 가산점요소)
5. 채용 상세 페이지

   ➡️ 사용자는 채용상세 페이지를 확인할 수 있다.

   ➡️“채용내용”이 추가적으로 담겨있음

   ➡️해당 회사가 올린 다른 채용공고가 추가적으로 포함 **(선택사항 및 가산점요소).**
6. 사용자는 채용공고에 지원(선택사항 및 가산점요소)

   ➡️ 사용자는 채용공고 지원 (가점 요소이며, 필수 구현 요소가 아님)

   ➡️사용자는 1회만 지원 가능

### 개발 시 참조사항

- 요구사항(의도)을 만족시킨다면 다른 형태의 요청 및 리스폰스를 사용하여도 좋다.
- 필요한 모델: 회사, 사용자, 채용공고, 지원내역(선택사항)
  (이외 추가 모델정의는 자유)
- 필드명은 임의로 생성 가능.
- 회사, 사용자 등록 절차는 생략.
  (DB에 임의로 생성하여 진행)
- 로그인 등 사용자 인증절차(토큰 등)는 생략.
- Frontend 요소(html, css, js 등)는 개발 범위에 제외.
  (구현시 불이익은 없지만, 평가에 이점 또한 없습니다.)
- 명시되지 않은 조건 또한 자유롭게 개발 가능.

## 과제 수행 과정

과제 수행은 다음과 같은 순서로 진행했으며 이 순서대로 기록하겠다.

1. 구조 설계
2. 환경 구성, H2 데이터 베이스 연동
3. 엔티티 생성 및 초기 데이터 설정
4. Repository, Service, Controller 생성
5. 기능 추가
   - 공고 등록
   - 공고 상세보기
   - 상세보기에서 지원 기능 추가
   - 검색 기능
   - 공고 삭제
   - 공고 수정
   - 조회 기능
   - 상세보기에서 회사가 올린 다른 공고 보기
6. Github 업로드
7. README.md 작성
8. 제출 😂😂

~~기능의 순서가 뒤죽박죽인 이유는 붙잡고 있다가도 안되면 포기하고 다른 기능으로 갈아탔기 때문..~~

## 1.구조 설계

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/3ca3a3ce-ae7c-405f-93e9-8eeb0b345188)

모델과 계층 구조는 다음과 같다.
지원자 입장에서는 지원한 채용공고를 확인하고, 채용공고 입장에서는 채용공고를 올린 기업과, 채용공고에 지원한 지원자 목록을, 회사 입장에서는 등록한 채용공고 목록을 반환해야 하므로 다음과 같이 양방향 매핑으로 설계했다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/871c2bbe-8642-44d0-b25c-73f030d487d9)

어플리케이션은 다음과 같이 구성되어 있다. 이 부분도 모호하게 구상만해둔 상태로 프로젝트에 착수해 진행 중 재차 수정했다. 

## 2.환경 구성

프로젝트 개발 환경은 이렇게 구성했다.

- Language: Java
- Type: Gradle - Groovy
- Java: 17
- Packaging: Jar
- Spring Boot: 3.1.3
- Dependencies: Spring Web, Spring Data JPA, thymeleaf, H2 Database, Lombok

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/55d3532a-c7aa-4973-b290-178951d304a0)
H2 데이터베이스에서 pre-onboarding 이라는 데이터베이스를 생성한 뒤, application.properties 에 다음과 같은 사항들을 추가해 주었다.
참고로 데이터베이스 생성시에는 비밀번호를 반드시 넣어줘야 하고, 데이터베이스에 접속할 때마다 비밀번호를 입력하는 게 번거롭다면, 데이터베이스에 접속 후
`ALTER USER SA SET PASSWORD '';` 를 입력해주면 비밀번호를 입력하지 않아도 된다.


## 3.엔티티 생성 및 초기 데이터 생성

H2 데이터베이스 연동 후, Member, Company, JobPosting 클래스를 생성해주었고, @Entity 어노테이션을 추가한 뒤 프로젝트를 실행하면 테이블을 생성하는 것을 볼 수 있었다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/609c9965-439f-4086-8f71-fd111f0840e7)

Getter와 Setter를 생성하는 작업이 번거로워서 @Data 어노테이션을 붙여주었고, 데이터 구조에서처럼 채용공고-회사, 채용공고-멤버 간을 양방향으로 연결해주었는데, 이 부분에서 순환 참조 문제가 발생하여 @JsonIgnore 어노테이션을 추가해주었다.

초기데이터는 data.sql에 다음과 같이 작성해 주었는데

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/780f0c68-95d3-43b3-9a73-92eabddd61f6)

초반에 스프링에서 계속 쿼리를 인식하지 못하는 오류가 발생했다.

**application.properties**
```properties
spring.jpa.defer-datasource-initialization=true
spring.sql.init.mode: always
spring.sql.init.data-locations=classpath:data.sql
```

application.properties에 다음과 같은 항목도 작성해주었는데도 해결되지 않았는데 쿼리를 대문자로 작성하니 해결되었따...

## 4.Repository, Service, Controller 생성

[인프런: 스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술 강의 자료]
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/ca896544-41cc-43a5-99c7-5969ebb2a30a)

이런 구조로 프로젝트를 설계해야 하는데요...

### Repository

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/1f630536-657b-4796-8632-8fb63e17ae8e)

레포지토리는 JpaRepository를 extends 해주었다. 이렇게 하면 CRUD가 가능해진다!
PostRepository의 findSearchKeyword는 [기능 부분](https://yeondori.github.io/posts/pre-onboarding-02/)에서 설명할 예정. 

### Service

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/acadb22c-3fd9-424f-b267-bc1a64d54355)

핵심 비즈니스 로직들이 구현되어 있는 서비스! 데이터베이스에 데이터를 영속하기 위해 @Transactional 어노테이션을 붙여주었다. 

+ [@Transactional](https://blog.neonkid.xyz/289) 

### Controller

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/5a920339-e2da-43b0-bb6e-f2e35b19ae28)

멤버의 경우 채용공고를 보고 지원하는 정도의 권한만 있어서 컨트롤러는 Company와 Post만 생성해주었다.
@RestController 어노테이션을 사용하려고 했으나 JSON 형식의 리턴값이 아닌 html을 반환하고 싶어서 @Controller 어노테이션을 사용해주었다.