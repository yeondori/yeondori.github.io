---
title: "[강의] 실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발 - trouble shooting"
excerpt: "인프런 강의 복습"
categories: [Inflearn, JPA-UTIL1]
tags: [JPA]
date: 2023-11-03
last_modified_at: 2023-11-03
render_with_liquid: false
---

---- 
# 실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발 [강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)

김영한님의 인프런 강의(실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발)는 따라 코딩하는 게 대부분인 강의라서 강의 내용을 [github](https://github.com/yeondori/inflearn-jpashop)에 정리해보기로 했다.

따라 코딩하는 중에 발생하는 오류만 여기서 따로 다뤄보고자 한다!

## 섹션 1. 프로젝트 환경설정
### 테스트 에러
h2데이터 베이스를 연동한 뒤, Member 클래스와 레퍼지토리를 생성해 주었고, 이에 대한 테스트인 MemberRepositoryTest를 수행하는 것이 1장의 내용이었다. 

**MemberRepositoryTest.java**
```java
@RunWith(SpringRunner.class)
@SpringBootTest
@ContextConfiguration(classes = SpringRunner.class)
public class MemberRepositoryTest {

    @Autowired MemberRepository memberRepository;

    @Test
    @Transactional
    public void testMember() throws Exception {
        //given
        Member member = new Member();
        member.setUsername("memberA");

        //when
        Long saveId = memberRepository.save(member);
        Member findMember = memberRepository.find(saveId);

        //then
        assertThat(findMember.getId()).isEqualTo(member.getId());
        assertThat(findMember.getUsername()).isEqualTo(member.getUsername());
    }
}
```

그러나 이런 오류가 발생했고

`org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: Unable to create requested service [org.hibernate.engine.jdbc.env.spi.JdbcEnvironment] due to: Unable to determine Dialect without JDBC metadata (please set 'javax.persistence.jdbc.url', 'hibernate.connection.url', or 'hibernate.dialect')`


#### 시도

1. application.yml에 `database-platform: org.hibernate.dialect.H2Dialect` 추가
2. Settings에서 Build & Run 설정 Gradle에서 IntelliJ로 변경
3. H2 버전 2.1.214로 변경
4. @ContextConfiguration(classes = SpringRunner.class) 추가 [참조](https://velog.io/@gillog/VSCode-jUnit-Test-%EC%8B%A4%ED%96%89-%EC%95%88%EB%90%A0-%EB%95%8C-Could-not-detect-default-configuration-classes-for-test-class)
5. build.gradle	`implementation 'jakarta.persistence:jakarta.persistence-api:3.1.0'` 추가 [참조]()
6. Member 클래스의 @GeneratedValue 어노테이션에 strategy = GenerationType.IDENTITY 명시 - **해결!**

@GeneratedValue 어노테이션은 기본적으로 자동으로 ID를 생성하는데, H2 데이터베이스의 경우 @GeneratedValue와 함께 어떤 시퀀스를 사용할 것인지 명시하지 않아 발생하는 문제였던 것 같다. 
@GeneratedValue 어노테이션에 strategy 속성을 GenerationType.IDENTITY로 명시해주었더니 해결되었다.

사실 이것만의 문제는 아닌 것 같고 예제 코드에 있던 MVCC 설정이나 테스트 환경, 버전 차이 등이 복합적으로 엉켜 발생했던 문제인 것 같다.




