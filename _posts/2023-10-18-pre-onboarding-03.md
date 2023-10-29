---
title: "[프로젝트]원티드 프리온보딩 과제 수행 03 트러블 슈팅 및 정리"
excerpt: "원티드 프리온보딩 과제 수행 과정 기록"
categories: [Project, Wanted-pre-onboarding]
tags: [Spring]
date: 2023-10-18
last_modified_at: 2023-10-20
render_with_liquid: false
---
# [프리온보딩 인턴십 수행과제](https://bow-hair-db3.notion.site/1850bca26fda4e0ca1410df270c03409)

프로젝트는 [🚀여기🚀](https://github.com/yeondori/wanted-pre-onboarding-backend)에서 확인할 수 있다.

## 과제 수행 과정

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

## Github 업로드와 README 작성

나는 IntelliJ Ultimate 버전을 사용 중인데 현재 로컬에서 작업하던 프로젝트도 Github에 push 가 가능하다고 해서 도전해 보았지만 first fetch 오류가 발생했다.

그래서 Github에서 Repository를 생성하고 git clone을 한 뒤, 해당 폴더에 내 프로젝트를 옮겨주고 push 했다.

과제 가산점 요소 중 하나가 커밋 컨벤션이었는데 난생 처음 들어보고.. 앞으로의 프로젝트에 참고하겠습니다.

그리고 제일 어려운 README 작성하기
이건 다른 멋쟁이 개발자들의 README 를 열심히 보고 참고하며 발전시키는 방법 밖에 없다. 

## 프로젝트 트러블 슈팅 모음 

### data.sql 초기 데이터 설정 관련 
data.sql에서 쿼리를 대문자로 작성해줘야 한다. 이래도 이상이 있을 경우에는 application.properties에 다음을 추가해보자. 

```properties
spring.jpa.defer-datasource-initialization=true
spring.sql.init.mode: always
spring.sql.init.data-locations=classpath:data.sql
```

또한 초기 데이터를 설정할 때, 

`INSERT INTO JOB_POSTING (COMPANY_ID, POST_ID ,JOB_POSITION, RECRUITMENT_COMPENSATION, RECRUITMENT_DETAILS, TECHNOLOGY_USED) VALUES (1L, 1L,'백엔드 주니어 개발자', 1000000, '원티드랩에서 백엔드 주니어 개발자를 채용합니다. 자격요건은..', 'Python');` 

처럼 채용공고의 아이디를 1로 설정해 주었는데, 서버에서 채용공고를 등록할 때 오류가 발생했다.(404 Not Found) 
뒤로 갔다가 다시 채용공고를 등록하는 경우부터는 정상적으로 작동했는데, 초기 데이터와 id를 생성하는 @GeneratedValue 사이에서 충돌이 있었던 듯 하다. 
1234L과 같은 id로 변경해주었더니 해결되었다. 회사와 멤버 아이디 또한 프로젝트를 확장한다면 바꿔줘야 할 듯 싶다.

### 순환 참조

분명 JPA 강의에서 최근까지도 들었던 내용이다. 그때만해도 순환참조? 너무 부주의해서 일어나는 거 아닌가? 했는데

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/25630680-3d8f-4a3e-afcd-8790ad86c3f5)

안녕하세요 정부주의라고 불러주십쇼.

이 부분은 해당 객체 필드들은 @JsonIgnore 어노테이션을 붙여 해결해주었다. ~~해결된 건가?~~ 
아무튼 이 부분은 좀 더 공부하고 수정해봐야겠다.

### @Controller 와 @RestController

RESTful 강의에서 JPA를 사용할 때는 @RestController를 붙인다고 배웠는데요... 

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/e80b6dba-a1b2-4341-8f0f-3519dff31e05)
근데 또 템플릿을 사용하려면 @RestController 가 아닌 @Controller 를 사용해야 했다.

근데 그러면 또 @ResponseBody를 붙여줘야 하는 메소드들이 있었고... 하여튼 간에 정리해서 올려야 할 게 늘었다. 야호


### 바보같은 실수 모음

- 데이터가 바뀌지 않는 문제가 있었는데 영속성을 깜빡했다 ~~깜빡할 게 따로 있지~~ @Transactional 
- 검색 기능에서 검색 결과가 있는 검색어를 입력해도 검색 결과가 아무 것도 반환되지 않았다. logger까지 확인했는데도 분명히 검색 결과가 있는데 말이지..
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/876b232c-00d9-4257-9ce6-0f0751051bfe)
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/65ffafb5-8894-4df3-8b90-5555d98b59d9)
알고보니 html에서 애먼 데이터를 불러오고 있었다는 ~~훈훈한~~ 결말
- 기타 자잘구레한 문법 실수들 ...  특히 html 관련 오류가 많았는데  이건 내친구 지피티에게 당근과 채찍을 번갈아주며 동고동락한 끝에 해결됐다.


## 느낀 점 

느낀 점이라고 말하긴 모호하고 부끄럽지만, 

1. 설계를 명확히 하자 

   -> 객체지향의 사실과 오해 책에서 왜 그렇게 수십번 강조했는지 알겠다. 
      너무 많이 강조하시는 거 아닌가 하고 건방지게 생각할 뻔했는데, 분명 아는데 막상 설계할 때는 고려하지 못한 점들이 매우 많다. 설계가 부정확하고 모호하니 개발도 뒤죽박죽 엉망진창이었고 다시 바로 잡는데도 시간이 꽤 소요됐다. 


2. 인생은 실전이다

   -> 분명히 강의에서 배운 내용들인데도 막상 해보려니까 머드라.. 싶다. 예를 들어 계층 구조를 명세하지 않은 채로 프로젝트를 진행하다가 정신 차려보니 컨트롤러에서 레포지토리의 메소드를 가져다 쓰는 경우가 발생하곤 했다. 강의 들을 때는 분명히 완벽하게 이해했다고 생각한 개념들도 오백번 찾아봤다. 인생은 점말 실전이구나


3. 프론트엔드를 생각하지 않을 수 없다

   -> 이건 그냥 내 욕심인 것 같지만 과제에서 프론트엔드 측 요소는 평가하지 않는다고 말했는데도 자꾸 신경이 쓰였다. 그리고 오히려 이 부분을 고려하며 프로젝트를 진행하니 모호했던 설계도 나름의 윤곽이 갖춰졌다... 도와준 챗GPT 선생님께 무한한 감사


4. ~~꺼진 불씨도 다시보자~~ 끝난 프로젝트도 다시 보자

   -> 내 생각보다 더 코드가 지저분했다. 불필요한 주석이나 변수들도 있었고 오류가 있는 부분들도 있었다. 분명 당시엔 나름 더 이상 고칠 게 없다고 생각했는데 말야


5. 기록 필수 
   
   -> 정신 사나울까봐 프로젝트 진행 과정을 따로 정리해두진 않고 노트에 적어두었는데 한번에 올리려니까 너어무 힘들고 오랜 시간이 걸린다. 미리미리 하자 제발

## Develop

보완할 점이나 더 추가하고 싶은 기능

1. Test 
   
   -> 아직 테스트가 미숙해 시간상 JUNIT 테스트를 진행하지 못하고 매번 직접 서버를 구동해 확인했는데, 매우매우 부적절한 방식임을 알고 있음😭😭. 이건 꼭 해보자


2. 사용자, 기업 인증 및 검증 절차 추가하기

3. Entity 연관관계, 메소드 개선

4. 불필요한 코드 정리 및 변수명 통일 작업

5. 도메인, 계층 구조 설계 검토 + DTO

+ 사용한 계층 구조에 대해서 각 계층이 어떤 역할이며, 어떤 객체가 어떤 계층에 담겨야 하고 그렇게 되었을 때 뭐가 좋은지?

## 앞으로 더 공부해볼 것

1. @RestController vs @Controller  


2. Git 커밋 인벤션 [참고 블로그](https://velog.io/@shin6403/Git-git-%EC%BB%A4%EB%B0%8B-%EC%BB%A8%EB%B2%A4%EC%85%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)

3. Gitignore


++ 천재 개발자 최야호씨에게 무한한 감사