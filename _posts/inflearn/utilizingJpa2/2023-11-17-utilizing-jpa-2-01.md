---
title: "[강의] 실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화 - trouble shooting"
excerpt: "인프런 강의 복습"
categories: [Inflearn, 실전! 스프링 부트와 JPA 활용2]
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

## 섹션 1. API 개발 기본

회원 등록 API를 구현하는 과정에서 Postman에 body를 넘겨줄 때마다 `400 Bad Request`가 발생했다. 
숨겨진 헤더는 그대로 두고 Content-Type application/json을 별도로 추가해줬는데 이게 인식이 안돼서 발생한 오류 같다.
숨긴 헤더에서 Content-Type을 다음과 같이 변경해주었더니 해결되었다. 
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/c0fd3dc9-ba45-42c1-a7c7-8e7ab46d44e8)


## 섹션 3. API 개발 고급 - 지연 로딩과 조회 성능 최적화

## 버전 호환 문제

강의 내용에서 hibernate5Module을 사용하는 예제가 있었는데 빌드에는 이상이 없었으나 다음과 같은 오류가 발생했다. 

```java
2023-11-21T14:23:20.112+09:00 ERROR 6562 --- [nio-8080-exec-1] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Handler dispatch failed: java.lang.NoClassDefFoundError: javax/persistence/Transient] with root cause

java.lang.ClassNotFoundException: javax.persistence.Transient
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:641) ~[na:na]
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:188) ~[na:na]
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:520) ~[na:na]
	at com.fasterxml.jackson.datatype.hibernate5.HibernateAnnotationIntrospector.hasIgnoreMarker(HibernateAnnotationIntrospector.java:69) ~[jackson-datatype-hibernate5-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.introspect.AnnotationIntrospectorPair.hasIgnoreMarker(AnnotationIntrospectorPair.java:321) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.introspect.AnnotationIntrospectorPair.hasIgnoreMarker(AnnotationIntrospectorPair.java:321) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.introspect.POJOPropertiesCollector._addFields(POJOPropertiesCollector.java:603) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.introspect.POJOPropertiesCollector.collectAll(POJOPropertiesCollector.java:445) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.introspect.POJOPropertiesCollector.getJsonValueAccessor(POJOPropertiesCollector.java:286) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.introspect.BasicBeanDescription.findJsonValueAccessor(BasicBeanDescription.java:258) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.BasicSerializerFactory.findSerializerByAnnotations(BasicSerializerFactory.java:393) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.BeanSerializerFactory._createSerializer2(BeanSerializerFactory.java:225) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.BeanSerializerFactory.createSerializer(BeanSerializerFactory.java:174) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.SerializerProvider._createUntypedSerializer(SerializerProvider.java:1503) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.SerializerProvider._createAndCacheUntypedSerializer(SerializerProvider.java:1451) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.SerializerProvider.findContentValueSerializer(SerializerProvider.java:789) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.impl.PropertySerializerMap.findAndAddSecondarySerializer(PropertySerializerMap.java:90) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.std.AsArraySerializerBase._findAndAddDynamic(AsArraySerializerBase.java:314) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.std.CollectionSerializer.serializeContents(CollectionSerializer.java:140) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.std.CollectionSerializer.serialize(CollectionSerializer.java:107) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.std.CollectionSerializer.serialize(CollectionSerializer.java:25) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.DefaultSerializerProvider._serialize(DefaultSerializerProvider.java:479) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ser.DefaultSerializerProvider.serializeValue(DefaultSerializerProvider.java:399) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ObjectWriter$Prefetch.serialize(ObjectWriter.java:1568) ~[jackson-databind-2.15.3.jar:2.15.3]
	at com.fasterxml.jackson.databind.ObjectWriter.writeValue(ObjectWriter.java:1061) ~[jackson-databind-2.15.3.jar:2.15.3]
	at org.springframework.http.converter.json.AbstractJackson2HttpMessageConverter.writeInternal(AbstractJackson2HttpMessageConverter.java:483) ~[spring-web-6.0.13.jar:6.0.13]
	at org.springframework.http.converter.AbstractGenericHttpMessageConverter.write(AbstractGenericHttpMessageConverter.java:103) ~[spring-web-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.mvc.method.annotation.AbstractMessageConverterMethodProcessor.writeWithMessageConverters(AbstractMessageConverterMethodProcessor.java:297) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor.handleReturnValue(RequestResponseBodyMethodProcessor.java:194) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.method.support.HandlerMethodReturnValueHandlerComposite.handleReturnValue(HandlerMethodReturnValueHandlerComposite.java:78) ~[spring-web-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:136) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:884) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:797) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1081) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:974) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1011) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:903) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at jakarta.servlet.http.HttpServlet.service(HttpServlet.java:564) ~[tomcat-embed-core-10.1.15.jar:6.0]
	at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:885) ~[spring-webmvc-6.0.13.jar:6.0.13]
	at jakarta.servlet.http.HttpServlet.service(HttpServlet.java:658) ~[tomcat-embed-core-10.1.15.jar:6.0]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:205) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:149) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:51) ~[tomcat-embed-websocket-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:174) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:149) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100) ~[spring-web-6.0.13.jar:6.0.13]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116) ~[spring-web-6.0.13.jar:6.0.13]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:174) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:149) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93) ~[spring-web-6.0.13.jar:6.0.13]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116) ~[spring-web-6.0.13.jar:6.0.13]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:174) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:149) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201) ~[spring-web-6.0.13.jar:6.0.13]
	at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:116) ~[spring-web-6.0.13.jar:6.0.13]
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:174) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:149) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:167) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:90) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:482) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:115) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:93) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:74) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:340) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:391) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:63) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:896) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1744) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:52) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1191) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61) ~[tomcat-embed-core-10.1.15.jar:10.1.15]
	at java.base/java.lang.Thread.run(Thread.java:833) ~[na:na]


```

현재 내 프로젝트의 스프링 부트 버전이 javax가 아닌 jakarta를 사용하는 버전이기 때문에 버전 문제라고 생각했고,
[사이트](https://github.com/FasterXML/jackson-datatype-hibernate)를 찾아보니 jakarta 버전이 따로 존재했다.

`implementation 'com.fasterxml.jackson.datatype:jackson-datatype-hibernate5` 를 	`implementation 'com.fasterxml.jackson.datatype:jackson-datatype-hibernate5-jakarta`로 바꿔 해결되었다. 

버전 관리를 자동으로 해줘서 강의 내용 그대로 따라하면 되는 줄 알았는데,,,  버전은 직접 확인하자~ 
강의 자료에도 있는 내용이었움!