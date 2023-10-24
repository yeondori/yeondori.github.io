---
title: "[Git] Git Commit Convention"
excerpt: "2023.10.23"
categories: [1Day-1Keyword, Git]
tags: [Commit, git]
date: 2023-10-23
last_modified_at: 2023-10-23
render_with_liquid: false
---

---- 
**Keyword**

하루에 하나씩 키워드를 정해 공부해보려고 한다.

일단은 왜 이 키워드를 공부하려고 하는가(why), 그래서 이 키워드가 무엇인가(what), 어떻게 적용해나갈 것인가(how)로 나눠 정리하려고 한다!

----- 

# Why?

[🚀첫 스프링 프로젝트 🚀](https://github.com/yeondori/wanted-pre-onboarding-backend) 를 진행하며 커밋 컨벤션을 처음 들어보았다. 

이전에는 커밋 메시지의 중요성을 알지 못해 "upload", "modify" 식으로 간략하게 작성해 커밋해 주었고, 여러 파일에 변경 사항이 있어도 한꺼번에 커밋을 하곤 했다.

어차피 나만 보는 코드인데! 라고 생각하기도 했고 한번 변경하고 나면 다시 돌아보지 않았다. 

프로젝트 요구사항 중 가산점 요소에 커밋 컨벤션이 있어 처음 알게 되었을 때도, 이미 로컬에서 작업한 뒤 레포지토리에 한꺼번에 커밋해 둔 상태여서 다음부터 사용해봐야지 하고 넘겼었다.

야호가 커밋 컨벤션에 관해 다음과 같은 피드백을 해주었고,

- 작업하고 있는 작업을 롤백할 수 있는 단위로 버전 관리하는 것이므로 커밋 컨벤션을 해야 Git을 쓰는 의미가 있다.
- 커밋 단위가 어떤 흐름으로 어떻게 코드를 짜나갔는지를 보여주는 지표가 된다.
- 컴파일이 깨지지 않는, 동작되는 가장 작은 단위로 커밋하자.


다른 사람들의 레포지토리와 커밋 메시지를 보고 그제서야 커밋 컨벤션의 중요성을 체감할 수 있었다.

심지어 인텔리제이 ultimate을 사용 중인데 이렇게 변경사항이 있는 단위로 커밋할 수 있다는 것을 이제서야 알았다..!

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/05fc3375-487c-40da-bf3c-05ff77e0ee73)

터미널에서 무조건 `git add .` , `git commit -m ` 을 냅다 입력하던 과거를 반성하고 있다. 


# What?

그래서 커밋 컨벤션이 무엇이며 어떻게 커밋 메시지를 작성해야 하는가? 

주로 미국의 교육기관인 Udacity의 [Udacity Git Commit Message Style Guide](https://udacity.github.io/git-styleguide/)를 참고한다.

가이드에 따르면, 커밋 메시지는 다음과 같은 3개의 파트로 구성되어 있다.


> type: Subject<br><br>(Option)body<br><br>(Option)footer


## Type

다음과 같은 타입들이 존재한다.

| type     | 설명                           |
|:---------|:-----------------------------|
| feat     | 새로운 기능                       |
| fix      | 버그 수정                        |
| docs     | 문서 변경 사항                     |
| style    | 서식 지정, 세미콜론 누락 등 코드 변경 없음    |
| refactor | 코드 리팩토링                      |
| test     | 테스트 추가, 테스트 리팩토링             |
| chore    | 빌드 작업, 패키지 관리자 구성 등 업데이트, 코드 변경 없음 |

덧붙여, [[Git] Commit Message Convension (협업을 위한 git 커밋컨벤션)](https://velog.io/@msung99/Git-Commit-Message-Convension)와 [[Git/Github] Commit Convention이란?](https://kdjun97.github.io/git-github/commit-convention/) 에서 기타 타입들도 소개하고 있다.

| type                                   | 설명                                                    |
|:---------------------------------------|:------------------------------------------------------|
| Add                                    | 코드나 테스트, 예제, 문서등의 추가 생성                               |
| Improve : 향상이 있는 경우. 호환성, 검증 기능, 접근성 등 |
| Implement                              | 코드가 추가된 정도보다 더 주목할만한 구현체를 완성시켰을 때                     |
| Move                                   | 코드의 이동이 있는 경우                                         |
| Updated                                | 계정이나 버전 업데이트가 있을 때 사용. 주로 코드보다는 문서나, 리소스, 라이브러리 등에 사용 |
| Comment                                | 필요한 주석 추가 및 변경                                        |
| design                                 | CSS 등 사용자 UI 디자인 변경                                   |
| init                                   | 프로젝트 초기 생성                                            |
| rename                                 | 파일 혹은 폴더명을 수정하거나 옮기는 경우                               |
| remove                                 | 파일을 삭제하는 작업만 수행하는 경우                                  |

## Subject

제목은 50자 이하여야 하며 대문자로 시작하고 마침표로 끝나서는 안 된다.
명령형 어조를 사용하며 과거형을 붙이지 않는다.
 
`fixed -> fix`

## Body

모든 커밋이 본문을 보증할 만큼 복잡하지는 않으므로 선택 사항이며, 커밋에 약간의 설명과 컨텍스트가 필요한 경우에만 사용한다. 
커밋의 방법이 아닌 내용과 이유를 설명하려면 본문을 사용한다.
본문 작성 시 제목과 본문 사이에 빈 줄을 필수로 작성해야 하며, 한 줄의 길이는 72자 이내로 제한한다.

## Footer

바닥글은 선택 사항이며 이슈 트래커 ID를 참조하는 데 사용된다.

덧붙여, [Git | git 커밋 컨벤션 설정하기](https://velog.io/@shin6403/Git-git-%EC%BB%A4%EB%B0%8B-%EC%BB%A8%EB%B2%A4%EC%85%98-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0) 에 따르면 꼬리말에도 다음의 규칙이 있다.

꼬리말은 "유형: #이슈 번호" 형식으로 사용하며 여러 개의 이슈 번호를 적을 때는 쉼표(,)로 구분한다.

이슈 트래커 유형은 다음 중 하나를 사용한다.

|유형|                                 |
|:-|:--------------------------------|
|Fixes| 이슈 수정중 (아직 해결되지 않은 경우)          |
|Resolves| 이슈를 해결했을 때 사용                   |
|Ref| 참고할 이슈가 있을 때 사용                 |
|Related to| 해당 커밋에 관련된 이슈번호 (아직 해결되지 않은 경우) |

이외에도 아래 명령어와 함께 이슈 번호를 기입하면, commit 시 이슈 종료가 가능하다.  resolved, close, closes, closed, fix, fixed, resolve, resolves, resolved 
  
`Fixes: #45 Related to: #34, #23`

# How?

앞으로 어떻게 적용할 것인가! 
버전 관리와 가독성을 위해 커밋 컨벤션에 맞춰 커밋을 하도록 하겠읍니다...