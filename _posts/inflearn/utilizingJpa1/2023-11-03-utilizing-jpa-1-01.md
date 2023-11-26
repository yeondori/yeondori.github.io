---
title: "[강의] 실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발 - trouble shooting"
excerpt: "인프런 강의 복습"
categories: [Inflearn, 실전! 스프링 부트와 JPA 활용1]
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

## 섹션 2. 도메인 분석 설계 
### 실행 에러
강의를 따라 엔티티를 생성하고 연관관계를 매핑한 후 어플리케이션을 실행하는 과정에서 다음과 같은 오류가 발생했다.

```java
2023-11-09T01:03:37.277+09:00 ERROR 66368 --- [  restartedMain] o.s.boot.SpringApplication               : Application run failed

org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'entityManagerFactory' defined in class path resource [org/springframework/boot/autoconfigure/orm/jpa/HibernateJpaConfiguration.class]: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.MappingException: Could not instantiate id generator [entity-name=jpabook.jpashop.domain.OrderItem]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1770) ~[spring-beans-6.0.13.jar:6.0.13]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:598) ~[spring-beans-6.0.13.jar:6.0.13]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:520) ~[spring-beans-6.0.13.jar:6.0.13]
at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:325) ~[spring-beans-6.0.13.jar:6.0.13]
at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:234) ~[spring-beans-6.0.13.jar:6.0.13]
at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:323) ~[spring-beans-6.0.13.jar:6.0.13]
at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199) ~[spring-beans-6.0.13.jar:6.0.13]
at org.springframework.context.support.AbstractApplicationContext.getBean(AbstractApplicationContext.java:1166) ~[spring-context-6.0.13.jar:6.0.13]
at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:940) ~[spring-context-6.0.13.jar:6.0.13]
at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:616) ~[spring-context-6.0.13.jar:6.0.13]
at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:146) ~[spring-boot-3.1.5.jar:3.1.5]
at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:738) ~[spring-boot-3.1.5.jar:3.1.5]
at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:440) ~[spring-boot-3.1.5.jar:3.1.5]
at org.springframework.boot.SpringApplication.run(SpringApplication.java:316) ~[spring-boot-3.1.5.jar:3.1.5]
at org.springframework.boot.SpringApplication.run(SpringApplication.java:1306) ~[spring-boot-3.1.5.jar:3.1.5]
at org.springframework.boot.SpringApplication.run(SpringApplication.java:1295) ~[spring-boot-3.1.5.jar:3.1.5]
at jpabook.jpashop.JpashopApplication.main(JpashopApplication.java:10) ~[classes/:na]
at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:na]
at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:77) ~[na:na]
at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:na]
at java.base/java.lang.reflect.Method.invoke(Method.java:568) ~[na:na]
at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:50) ~[spring-boot-devtools-3.1.5.jar:3.1.5]
Caused by: jakarta.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.MappingException: Could not instantiate id generator [entity-name=jpabook.jpashop.domain.OrderItem]
at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:421) ~[spring-orm-6.0.13.jar:6.0.13]
at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.afterPropertiesSet(AbstractEntityManagerFactoryBean.java:396) ~[spring-orm-6.0.13.jar:6.0.13]
at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.afterPropertiesSet(LocalContainerEntityManagerFactoryBean.java:352) ~[spring-orm-6.0.13.jar:6.0.13]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1817) ~[spring-beans-6.0.13.jar:6.0.13]
at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1766) ~[spring-beans-6.0.13.jar:6.0.13]
... 21 common frames omitted
Caused by: org.hibernate.MappingException: Could not instantiate id generator [entity-name=jpabook.jpashop.domain.OrderItem]
at org.hibernate.id.factory.internal.StandardIdentifierGeneratorFactory.createIdentifierGenerator(StandardIdentifierGeneratorFactory.java:230) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.id.factory.internal.IdentifierGeneratorUtil.createLegacyIdentifierGenerator(IdentifierGeneratorUtil.java:126) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.mapping.SimpleValue.createGenerator(SimpleValue.java:414) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.internal.SessionFactoryImpl.lambda$createGenerators$1(SessionFactoryImpl.java:414) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at java.base/java.util.stream.ForEachOps$ForEachOp$OfRef.accept(ForEachOps.java:183) ~[na:na]
at java.base/java.util.stream.ReferencePipeline$2$1.accept(ReferencePipeline.java:179) ~[na:na]
at java.base/java.util.HashMap$ValueSpliterator.forEachRemaining(HashMap.java:1779) ~[na:na]
at java.base/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:509) ~[na:na]
at java.base/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499) ~[na:na]
at java.base/java.util.stream.ForEachOps$ForEachOp.evaluateSequential(ForEachOps.java:150) ~[na:na]
at java.base/java.util.stream.ForEachOps$ForEachOp$OfRef.evaluateSequential(ForEachOps.java:173) ~[na:na]
at java.base/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234) ~[na:na]
at java.base/java.util.stream.ReferencePipeline.forEach(ReferencePipeline.java:596) ~[na:na]
at org.hibernate.internal.SessionFactoryImpl.createGenerators(SessionFactoryImpl.java:413) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.internal.SessionFactoryImpl.<init>(SessionFactoryImpl.java:249) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.boot.internal.SessionFactoryBuilderImpl.build(SessionFactoryBuilderImpl.java:444) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.jpa.boot.internal.EntityManagerFactoryBuilderImpl.build(EntityManagerFactoryBuilderImpl.java:1458) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.springframework.orm.jpa.vendor.SpringHibernateJpaPersistenceProvider.createContainerEntityManagerFactory(SpringHibernateJpaPersistenceProvider.java:75) ~[spring-orm-6.0.13.jar:6.0.13]
at org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean.createNativeEntityManagerFactory(LocalContainerEntityManagerFactoryBean.java:376) ~[spring-orm-6.0.13.jar:6.0.13]
at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.buildNativeEntityManagerFactory(AbstractEntityManagerFactoryBean.java:409) ~[spring-orm-6.0.13.jar:6.0.13]
... 25 common frames omitted
Caused by: org.hibernate.HibernateException: Could not fetch the SequenceInformation from the database
at org.hibernate.engine.jdbc.env.internal.ExtractedDatabaseMetaDataImpl.sequenceInformationList(ExtractedDatabaseMetaDataImpl.java:307) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.engine.jdbc.env.internal.ExtractedDatabaseMetaDataImpl.getSequenceInformationList(ExtractedDatabaseMetaDataImpl.java:151) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.id.enhanced.SequenceStyleGenerator.getSequenceIncrementValue(SequenceStyleGenerator.java:568) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.id.enhanced.SequenceStyleGenerator.configure(SequenceStyleGenerator.java:216) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.id.factory.internal.StandardIdentifierGeneratorFactory.createIdentifierGenerator(StandardIdentifierGeneratorFactory.java:224) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
... 44 common frames omitted
Caused by: org.h2.jdbc.JdbcSQLSyntaxErrorException: Column "start_value" not found [42122-214]
at org.h2.message.DbException.getJdbcSQLException(DbException.java:502) ~[h2-2.1.214.jar:2.1.214]
at org.h2.message.DbException.getJdbcSQLException(DbException.java:477) ~[h2-2.1.214.jar:2.1.214]
at org.h2.message.DbException.get(DbException.java:223) ~[h2-2.1.214.jar:2.1.214]
at org.h2.message.DbException.get(DbException.java:199) ~[h2-2.1.214.jar:2.1.214]
at org.h2.jdbc.JdbcResultSet.getColumnIndex(JdbcResultSet.java:3492) ~[h2-2.1.214.jar:2.1.214]
at org.h2.jdbc.JdbcResultSet.getLong(JdbcResultSet.java:745) ~[h2-2.1.214.jar:2.1.214]
at com.zaxxer.hikari.pool.HikariProxyResultSet.getLong(HikariProxyResultSet.java) ~[HikariCP-5.0.1.jar:na]
at org.hibernate.tool.schema.extract.internal.SequenceInformationExtractorLegacyImpl.resultSetStartValueSize(SequenceInformationExtractorLegacyImpl.java:110) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.tool.schema.extract.internal.SequenceInformationExtractorLegacyImpl.lambda$extractMetadata$0(SequenceInformationExtractorLegacyImpl.java:54) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.tool.schema.extract.spi.ExtractionContext.getQueryResults(ExtractionContext.java:50) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.tool.schema.extract.internal.SequenceInformationExtractorLegacyImpl.extractMetadata(SequenceInformationExtractorLegacyImpl.java:39) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
at org.hibernate.engine.jdbc.env.internal.ExtractedDatabaseMetaDataImpl.sequenceInformationList(ExtractedDatabaseMetaDataImpl.java:291) ~[hibernate-core-6.2.13.Final.jar:6.2.13.Final]
... 48 common frames omitted


Process finished with exit code 0
```

원인은 다음과 같다. 

> 오류는 Hibernate의 SequenceStyleGenerator를 사용하여 엔터티의 기본 키를 생성하려고 시도할 때 발생할 수 있습니다. 
> SequenceStyleGenerator는 데이터베이스 시퀀스를 사용하여 고유 식별자를 생성하는 방식 중 하나입니다. 
> 그러나 이 오류는 H2 데이터베이스에서 "start_value" 열을 찾지 못하면서 발생합니다.

#### 해결 
H2 콘솔 창에서 `SELECT H2VERSION() FROM DUAL;` 를 실행해 버전을 확인했더니 1.4.200 이었고, build.gradle 에서는 2.1.214로 버전이 설정되어 있었다.
build.gradle에서 h2 버전을 1.4.200 로 설정해주었더니 해결되었다.


## 섹션 4. 회원 도메인 개발
### 실행 에러 
회원 서비스 테스트를 실행시키는 데 다음과 같은 오류가 발생했다.

```java
org.junit.runners.model.InvalidTestClassError: Invalid test class 'jpabook.jpashop.service.MemberServiceTest':
  1. The class jpabook.jpashop.service.MemberServiceTest is not public.
  2. Test class should have exactly one public constructor

	at org.junit.runners.ParentRunner.validate(ParentRunner.java:525)
	at org.junit.runners.ParentRunner.<init>(ParentRunner.java:92)
	at org.junit.runners.BlockJUnit4ClassRunner.<init>(BlockJUnit4ClassRunner.java:74)
	at org.springframework.test.context.junit4.SpringJUnit4ClassRunner.<init>(SpringJUnit4ClassRunner.java:137)
	at org.springframework.test.context.junit4.SpringRunner.<init>(SpringRunner.java:49)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at java.base/jdk.internal.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:77)
	at java.base/jdk.internal.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.base/java.lang.reflect.Constructor.newInstanceWithCaller(Constructor.java:499)
	at java.base/java.lang.reflect.Constructor.newInstance(Constructor.java:480)
	at org.junit.internal.builders.AnnotatedBuilder.buildRunner(AnnotatedBuilder.java:104)
	at org.junit.internal.builders.AnnotatedBuilder.runnerForClass(AnnotatedBuilder.java:86)
	at org.junit.runners.model.RunnerBuilder.safeRunnerForClass(RunnerBuilder.java:70)
	at org.junit.internal.builders.AllDefaultPossibilitiesBuilder.runnerForClass(AllDefaultPossibilitiesBuilder.java:37)
	at org.junit.runners.model.RunnerBuilder.safeRunnerForClass(RunnerBuilder.java:70)
	at org.junit.internal.requests.ClassRequest.createRunner(ClassRequest.java:28)
	at org.junit.internal.requests.MemoizingRequest.getRunner(MemoizingRequest.java:19)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:50)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater$1.execute(IdeaTestRunner.java:38)
	at com.intellij.rt.execution.junit.TestsRepeater.repeat(TestsRepeater.java:11)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:35)
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:232)
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:55)
```
어이없게도 그냥 public이 지워져있어서 그런 거였움,,

## 섹션 7. 웹 계층 개발
### 버전 에러

MemberForm에서 @NotEmpty 어노테이션을 붙여줘야 하는데 등록이 되지 않았다.
Validation Starter 의존성은 더 이상 웹 스타터 의존성에 포함되지 않으므로 Build.gradle 파일에 다음을 수동으로 추가해주니 해결 되었다.

`implementation 'org.springframework.boot:spring-boot-starter-validation'`

드디어 완강했다!