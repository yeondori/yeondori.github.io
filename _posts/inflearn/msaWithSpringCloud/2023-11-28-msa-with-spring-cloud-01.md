---
title: "[강의] Spring Cloud로 개발하는 마이크로서비스 애플리케이션(MSA) - 01 Service Discovery "
excerpt: "인프런 강의 복습"
categories: [Inflearn, Spring Cloud로 개발하는 마이크로서비스 애플리케이션(MSA)]
tags: [Spring Cloud, MSA]
date: 2023-11-28
last_modified_at: 2023-11-28
render_with_liquid: false
---

---- 

# Spring Cloud로 개발하는 마이크로서비스 애플리케이션(MSA) [강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4?gad_source=1&gclid=CjwKCAiAmZGrBhAnEiwAo9qHiV1UdD8oI92U-7gfKB3rk-3hG9lMtbJH4htJYaOez-8_NbLY68I9xhoCd08QAvD_BwE)

## 섹션 1. Service Discovery

### Eureka Service Discovery 프로젝트 생성 

스프링 프로젝트로 discoveryservice를 생성한다. 이때 Eureka Server를 dependency로 추가해주고, application에는 @EnableEurekaServer 어노테이션을 추가해준다.

```java
package com.example.discoveryservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class DiscoveryserviceApplication {

    public static void main(String[] args) {
        SpringApplication.run(DiscoveryserviceApplication.class, args);
    }

}
```
application.properties 를 application.yml로 바꿔준 후, 다음과 같이 설정한다. 

```yml
server:
  port: 8761

spring:
  application:
    name: discoveryservice

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```

이때, `register-with-eureka` 옵션은 eureka 서버로부터 인스턴스들의 정보를 주기적으로 가져오는가를 의미한다.   
세팅을 완료한 후 어플리케이션을 실행하여 http://localhost:8761/ 에 접속하면, 다음과 같은 화면을 볼 수 있다!

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/6c030e43-d2ea-45e5-a093-d9fb227c84e3) 

### User Service 프로젝트 생성

spring 프로젝트로 user-service를 생성해본다. dependency로는 Eureka Discovery Client, Spring Web, Spring Boot DevTools, Lombok을 추가해준다.

```java
package com.example.userservice;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class UserServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }

}
```

application에 @EnableDiscoveryClient 어노테이션을 추가해준 뒤, 다음과 같이 application.yml 파일을 설정한다.

```yml
server:
  port: 9001

spring:
  application:
    name: user-service

eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defalutZone: http://127.0.0.1:8761/eureka
```

discovery service가 구동중인 상태에서 user Service 어플리케이션을 구동하면, 기존의 http://localhost:8761/ 에서 다음과 같이 user service를 확인할 수 있다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/00f8c07c-a16f-4930-93a0-af6178e1f60b)

## IntelliJ
이번에는 user service를 하나 더 실행해보자!

Run/Debug Configurations 옵션에서 좌측 상단에 복사 버튼을 누르면
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/068bf8ad-3435-41f4-8289-3cf7c0432a33)

다음과 같이 어플리케이션을 복제할 수 있다.
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/fb26a687-7ef7-49e6-bec4-80c13d39b18a)

어플리케이션을 동시에 실행시키면 다음과 같은 오류가 발생하는데, 이는 두 어플리케이션 모두 9001 포트를 사용하고 있기 때문이다. 
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/01ae35a8-ee6e-45da-ada2-a3b2b074a9a2)

다시 Configurations 옵션에 들어가 add VM options에서 다음과 같이 포트번호를 추가해주면 
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/4e19d9b6-35fc-4fc2-af33-0710328be0a8)

Eureka 대시보드에서 두 어플리케이션 모두 정상적으로 동작하고 있는 것을 확인할 수 있다.
![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/28113654-fc75-496c-aee5-6fdcd4fb8e02)

## Terminal
터미널에서 커맨드 라인을 통해 실행시키는 방법도 있다.  
우선 프로젝트가 maven으로 빌드되어 있기 때문에 `brew install mvn` 으로 maven을 설치해주었다.

프로젝트에서 pom.xml, src, target이 있는 디렉토리에서 `mvn spring-boot:run`을 실행시켜주면 현재 디렉토리에 있는 스프링 부트 프로젝트를 실행할 수 있다.   
그러나 이렇게 실행시켜주면, 위에서 9001번 포트를 사용하고 있으므로 오류가 발생하게 된다. 

`mvn spring-boot:run -Dspring-boot.run.jvmArguments='-Dserver.port=9003'` 
따라서 옵션으로 jvm argument의 서버 포트번호를 넘겨주어야 한다. 

대시보드를 확인해보면, 앞선 예시의 user service 두개를 포함해 총 세개의 user service가 기동된 것을 확인할 수 있다!

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/888329a1-35ca-445d-a7c5-66941ec8fae1)

로컬 환경의 터미널에서 서비스를 한개 더 기동시켜보자!

마찬가지로 프로젝트가 있는 디렉토리에서 `mvn clean`, `mvn compile package` 를 차례로 입력해주면 target폴더 내에 jar 파일이 생성된 것을 확인할 수 있다.  
`java -jar -Dserver.port=9004 ./target/user-service-0.0.1-SNAPSHOT.jar` 로 jar 파일을 직접 실행시켜주면, 

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/8c2a91ae-43cc-47df-a945-c6ff188865ef)

네 개의 인스턴스가 작동 중인 것을 확인할 수 있다. 

하나의 user service를 생성한 뒤 네 가지 방식으로 기동을 시켜보았다. 그러나 매번 실행 시 포트 번호를 부여하는 것이 좋은 방법은 아닐 것이다. 
-> 스프링 부트의 랜덤포트를 사용해보자!

### Random Port

user service의 application.yml 파일의 포트번호를 9001에서 0으로 변경해준다.  
이는 랜덤 포트를 사용하겠다는 의미로, 같은 서비스가 실행된다 하더라도 매번 포트번호가 랜덤하게 할당되어 포트 충돌을 의식하지 않아도 된다!

```yml
server:
  port: 0

spring:
  application:
    name: user-service

eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defalutZone: http://127.0.0.1:8761/eureka
```
intelliJ와 터미널에서 각각 userService를 실행시켜보면 기존의 포트번호를 부여하는 번거로움 없이도 두 서비스가 정상적으로 작동하게 된다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/2fbcd447-3507-4fc6-a683-49ce156a384a)

그러나 유레카 대시보드를 살펴보면 하나의 서비스만 표시되고 있다.   
이는 현재 작동 중인 ip주소와 application 이름, 포트번호를 표시하는 방식 때문인데, 이때 포트 번호는 동적으로 할당된 포트번호가 아닌 yml 파일에서 설정해 준 포트 번호로 표시하므로 이 방법을 바꿔주어야 한다.

위의 yml파일에 eureka 하단에 
`instance:
instance-id: ${spring.cloud.client.hostname}:${spring.application.instance_id:${random.value}}`를 추가해준다.

![image](https://github.com/yeondori/yeondori.github.io/assets/93027942/fd42be03-0e12-402a-9411-bf464b19cc2d)

두 개의 인스턴스가 모두 정상적으로 표시되는 것을 확인할 수 있다!