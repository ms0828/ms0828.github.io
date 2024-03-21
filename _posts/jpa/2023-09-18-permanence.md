---
title: "JPA 영속성 관리"
excerpt: "JPA의 영속성 개념 필기"

categories:
  - JPA
tags:
  - [Java, Spring, Jpa]

permalink: /categories/jpa/jpa_permanence/

toc: true
toc_sticky: true

date: 2023-09-18
last_modified_at: 2023-09-18
---
JPA 내부 동작을 이해하기 위해서 Transaction, 영속성에 대해 알아보자

<br>


## Transaction
JPA를 이용해서 데이터를 변경하는 작업 위에는 항상 Transaction 안에서 작업해야한다. (데이터베이스 커넥션으로 생각하면 된다.)
<br>

* 데이터 변경 시, 엔티티 매니저가 반환하는 Transaction을 시작해야 함.

```
EntityManager em = entityManagerFactory.createEntityManager();

EntityTransaction tx = em.getTransaction();

tx.begin();

//데이터 작업 수행

tx.commit();
```

<br><br>

## 영속성 컨텍스트
우리가 Entity를 DB에 저장할 때 entitymanager.persis(Entity) 로 저장하지만, 이는 사실 DB에 저장하는 것이 아니라 '영속성 컨텍스트'에 저장하는 것이다.

* 영속성 컨텍스트는 EntityManager를 통해서 접근할 수 있음

<br>

그럼 DB에 저장은 언제?
* Transaction이 Commit 하는 시점에 영속성 컨텍스트에 있는 데이터가 저장


<br><br>


## 엔티티의 생명주기
1. 비영속 
<br>영속성 컨텍스트와 상관없는 새로운 상태<br>
 > ex) 객체가 생성된 상태
<br>

2. 영속<br>
영속성 컨텍스트에 관리되는 상태<br> 
> ex) entityManager.persist()로 객체를 영속성 컨텍스트에 등록한 상태
<br>

3. 준영속<br>
영속성 컨텍스트에 저장되었다가 분리된 상태
> ex) entityManager.detach(객체)
<br>

4. 삭제<br>
객체가 삭제된 상태
> entityManager.remove(객체)

<br>
<br>

## 영속성 컨텍스트는 왜 존재하는가?

<br>

**1. 1차 캐시<br><br>**

매번 DB에 접근하지 않고 1차 캐시를 이용하여 접근 속도 및 성능 향상

![Alt text](/assets/images/posts_img/jpa/permanence.png)


<br>

**2. 동일성 보장**

같은 Transaction에서 Entity를 접근할 때, 객체들의 동일성을 보장한다.

```
Entity a = entityManager.find(Entity.class, "key1");
Entity b = entityManager.find(Entity.class, "key1");

System.out.println(a == b); //동일성 보장
```

<br>

**3. 트랜잭션을 지원하는 쓰기 지연**

데이터 변경 시 즉시 SQL이 실행되는 것이 아니라 "쓰기 지연 SQL 저장소"에 변경 내역에 관한 SQL문을 저장해뒀다가, Transaction 커밋 시점에 SQL을 실행한다.
> 최적화 측면에서의 이점

<br>

**4. 변경 감지(Dirty Checking)**

데이터 변경 시, update를 따로 실행하지 않아도, 영속성 컨텍스트에서 데이터 변경 내역을 추적한다. 그리고 커밋 시점에 변경 내역을 자동으로 처리한다. (3번 내용과 유사)

**5. 지연 로딩(Lazy Loading)**

<br>
<br>

## 플러시
Transaction을 커밋할 때, 실질적으로 변경 내역에 대해 모아뒀던 SQL을 실질적으로 실행하는 기능  

* 플러시 실행 시, 영속성 컨텍스트를 비우지 않는다.
* Transaction 작업 단위에서 실행된다.

<br>
<br>

> 인프런 '김영한'님의 강의를 정리한 내용이므로, 개인 공부 목적으로 작성된 글입니다.
> <br>출처  <https://www.inflearn.com/course/ORM-JPA-Basic>

