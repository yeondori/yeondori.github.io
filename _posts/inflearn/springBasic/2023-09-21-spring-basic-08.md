---
title: "[강의] 스프링 핵심 원리 - 기본편 08 빈 생명주기 콜백"
excerpt: "인프런 강의 복습"
categories: [Inflearn, 스프링 핵심 원리 - 기본편]
tags: [Spring]
date: 2023-09-21
last_modified_at: 2023-09-21
render_with_liquid: false
---
# 스프링 핵심 원리 - 기본편 [강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)
김영한님의 인프런 강의(스프링 핵심 원리 - 기본편) 을 수강하면서 강의 내용을 일부 발췌해 요약한 글.

## **섹션 8** 빈 생명주기 콜백

### 빈 생명주기 콜백

데이터베이스 커넥션 풀(서버와 데이터 베이스를 올리기 전에 미리 연결), 네트워크 소켓(소켓을 미리 열어 둠)처럼 필요한 연결을 미리 해두거나 안전하게 정상적으로 종료 처리를 할 수 있도록!

스프링 빈은 `객체 생성 -> 의존 관계 주입` 의 라이프사이클을 가진다. (생성사 주입은 예외) 따라서 초기화 작업(객체 생성 작업X, 객체 안에 필요한 값이 다 연결되어 있고, 외부와 연결해 일을 시작할 수 있는 단계)은 의존관계 작업까지 모든 데이터가 세팅된 뒤 진행해야 한다. 스프링은 의존관계 주입이 완료되면 스프링 빈에게 콜백을 통해 초기화 시점을 알려주고, 빈이 종료될 때도 소멸 콜백을 하는 기능을 제공한다.

스프링 빈의 이벤트 라이프사이클 (싱글톤)
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/85ec75c6-fadd-4f4c-a54d-248b6204c8ca)

생명주기 콜백은 스프링 빈이 생성되거나 소멸되기 직전에 빈의 메서드를 호출해 줄 수 있는 기능으로, 크게 세 가지 방식이 있다.

- 인터페이스
- 빈 등록 초기화, 소멸 메서드
- 애노테이션

#### 인터페이스
`implements InitializingBean, DisposableBean` 후 afterPropertiesSet()와 destroy()를 override 한다.
afterPropertiesSet은 의존관계 주입이 끝나면(프로퍼티 세팅이 끝나면) 호출하는 것을 의미하며 destroy는 빈이 종료될 때 호출되는 것을 의미한다.

초창기에 나온 방식으로, 스프링 전용 인터페이스이므로 스프링에 의존한다. 메서드 이름을 변경할 수 없으며 코드를 고칠 수 없는 외부 라이브러리에는 적용할 수 없다는 단점이 있다. 

#### 빈 등록 초기화, 소멸 메서드

`@Bean(initMethod = "init 메서드 이름", destroyMethod = "close 메서드 이름")`

메서드 이름 자유롭게 변경 가능하며 스프링에 의존하지 않는다. 코드르 고칠 수 없는 외부 라이브러리에도 적용이 가능하다.
특히 destroyMethod는 기본값으로 (inferred) 추론 기능이 있어 close, shutdown 메서드를 자동으로 호출하는 기능이 있다. 추론 기능을 쓰기 싫다면 destroyMethod =""를 지정하면 된다.

#### 애노테이션 - 이 방법을 사용하면 된다!

@PostConstruct, @PreDestroy

스프링에서 권장하는 방법이고 매우 편리하다. javax(현재는 jakarta) 이므로 스프링이 아닌 다른 컨테이너에서도 동작하는 자바 표준이다. 그러나 외부 라이브러리에 적용은 불가하므로 이떄는 빈 등록 초기화, 소멸 메서드를 사용하면 된다.

