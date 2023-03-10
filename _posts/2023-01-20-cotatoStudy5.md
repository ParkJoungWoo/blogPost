---
layout: post
title: "JPA 개요와 Web layer"
subtitle: 스프링 2주차
categories: spring
tags: [spring]
---
⚠️ 본 게시물은 `스프링 부트와 AWS로 혼자 구현하는 웹서비스`를 참고도서로 활용합니다. 저작권은 본 책의 저서에게 있음을 알립니다.
# JPA
RDBMS를 사용해 프로젝트를 수행한다고 생각해봅시다.

사람과 거주지라는 클래스를 만든다면 어떻게 작성하실건가요?

```java
public class Human{
    private String 주소;
    private String name;
    private int age;
}

public class Place{
    private String 주소;
    private String 아파트명;
}
```
저라면 위와 같은 POJO로 작성할 것 같습니다.\
주소라는 String이 Place의 PK이고 Human의 FK라는 것을 알 수 있고 테이블 생성에도 큰 무리가 없어 보입니다.\
하지만 위와 같은 클래스는 제가 놓친 자바의 기본적인 원리가 숨어 있습니다.

## 객체 지향 프로그래밍
바로 객체 지향적인 프로그래밍입니다. 위와 같은 프로그래밍이 가질 수 있는 문제는 두가지가 있습니다.

1. 클래스를 단순히 데이터 그릇으로만 사용한다.
2. 클래스를 테이블에 맞추는 기형적인 설계가 나타난다.

Java를 사용하고 객체 지향적인 프로그래밍을 한다면 객체의 행동과 의미가 잘 들어나야하는데 위와 같은 작성은 *값으로 단순히 프로그래밍*을 할수밖에 없습니다.

> AWS로 혼자 구현하는 웹 서비스 81p : 객체 지향 프로그래밍은 메세지를 기반으로 기능과 속성을 한 곳에서 관리하는 기술

## JPA의 등장
그래서 많은 서버 개발자들에게 객체 지향적인 프로그래밍이 가능하도록 Mapping을 해주는 ORM(Obejct Relation Mapping)이 등장합니다. 바로 `JPA`입니다.

위 기술을 통해 객체를 객체답게 다룰 수 있고 DB에 따른 SQL의존도도 낮출 수 있습니다.

인터페이스와 같이 객체가 달라지더라도 같은 인터페이스를 implement한다면 교체가 용이하다는 점을 생각하면 됩니다.

상세한 JPA 문법은 다음에 다루도록 하겠습니다.

## Web 계층
이제 프로젝트를 진행할 때 시작하는 법, DB와 소통하는 법을 알았습니다. 하지만 또 다른 문제가 있습니다.
> 클래스, 패키지명을 어떻게 정해야 하는가?

API를 만들기 위해서는 데이터 교환을 위한 DTO 클래스, API 요청을 받는 Controller, 트랜잭션 및 도메인 로직의 순서를 보장하는 Service 가 존재합니다.

- 기존에 클래스를 기준으로 인자를 작성했다면 DTO 클래스를 통해 필요한 데이터를 모아서 교환을 합니다.
- Controller일 경우 MVC 패턴과 비슷하게 요청을 받고 그에 필요한 기능을 호출합니다.
- Service는 트랜잭션 및 도메인의 순서를 결정할 뿐 비즈니스 로직을 처리하지 않습니다.
  - Service는 순서만 보장합니다. 실제 서비스 기능은 Domain에서 처리합니다.

프로젝트에서 오류를 줄이기 위해서 기능의 역할을 집중하는 것이 좋습니다. 
```
Web : 외부의 요청과 응답에 대한 전반적인 영역

Service : Controller와 Dao의 중간 영역, @Transaction이 사용되는 영역

Repository : Database에 접근하는 영역

DTO : 계층간의 데이터 교환을 위한 객체

Domain Model : 개발 대상을 단순화 시킨것
```



