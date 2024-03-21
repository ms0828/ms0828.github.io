---
title: "JPA 다양한 연관관계"
excerpt: "JPA의 다양한 연관관계에 대한 정리"

categories:
  - JPA
tags:
  - [Java, Spring, Jpa]

permalink: /categories/jpa/entity_relationship/

toc: true
toc_sticky: true

date: 2023-09-18
last_modified_at: 2023-09-18
---

이 포스팅은 앞에 "연관관계 매핑" 포스팅 시나리오를 기반으로 정리한 글<br><br>


## 연관관계 매핑시 고려사항
1. 다중성 (다대일, 일대다)
2. 단방향, 양방향
3. 연관관계 주인
<br><br>



## 다대일 [N:1]
@ManyToOne
* '다' 쪽에 연관관계 주인을 잡으면 편리하다.

  > 다대일, 일대다 연관관계 매핑의 경우, DB 설계상 '다' 쪽에 Foregin Key가 있어야 하기 때문<br>
  > (관계형 DB에서 다대일 관계에서는 '다'쪽의 테이블에 외래 키가 있다). 

<br>

### 다대일 - 단방향

관계형 DB에 Foregin Key가 있는 테이블과 매핑된 객체의 참조 필드에 @ManyToOne, @JoinColumn을 사용
<br>

```
@Entity
public class Member{

  //...

  @ManyToOne    //다대일 관계
  @JoinColumn(name = "TEAM_ID") //현재 매핑된 테이블의 Foregin Key를 관리하는 Column 이름을 지정
  private Team team;

}

```

<br>

## 다대일 - 양방향

참조 되는 객체 필드에 반대편 객체의 참조 필드를 추가한다.
<br>

```
@Entity
public class Team{    

  //...

  //매핑되는 Team 입장에서는 일대다 관계
  //mappedBy 속성을 사용해 Member의 "team" 필드에 의해 매핑되었다고 설정
  @OneToMany(mappedBy = "team")
  List<Member> members = new ArrayList<>();

}
```


<br><br>

## 일대다 [1:N]
@OneToMany

> '일'쪽의 객체가 Foregin Key에 대한 객체 참조를 관리한다고 하면?

테이블 관계에서는 일대다 관계가 항상 '다' 쪽에 외래 키가 있을 수 밖에 없다. <br>

이러한 차이 때문에 객체가 반대편 테이블의 외래 키를 관리하게 되는 구조<br>

> 권장되지 않는 연관관계 방식 (차라리 다대일 양방향 매핑을 사용하자)<br> 일대다 양방향은 공식적으로 존재하지 않음


<br>
<br>

![일대다 단방향](/assets/images/posts_img/jpa/entity_mapping3.png)


* Team 객체에서 Member를 추가하면, DB에서는 Member(FK를 갖고 있는)테이블에 Update Query가 실행됨

<br>
<br>

> JoinColumn 을 사용하지 않으면, Join 테이블 방식을 사용하기 때문에이 Join 테이블이 하나 더 생성된다. <br> 특수한 경우가 아니라면, JoinColumn을 사용하자.






<br><br>

## 일대일 [1:1]
@OneToOne

* 참조하는 테이블이나 참조되는 테이블 모두 둘 중 하나로 외래 키 선택

* 다대일 관계에서 외래 키에 데이터베이스 유니크 제약조건 추가
  > 유니크 제약조건으로 테이블의 1:1 관계가 성립이 된다. <br> 만약 유니크 제약조건 없이 설계하면, Application 개발에서 제약을 따로 관리하며 개발해야함

* 양방향일 경우, 외래 키를 가지고 있는 쪽이 연관관계 주인, 반대편은 mappedBy 적용

  > 일대일 단방향 관계에서 일대다 처럼 참조하는 객체가 Feregin Key에 대한 참조를 가지고, 참조되는 객체에 대한 테이블에서 Foreign Key를 가지는 것은 지원하지 않는다. 
  > * 만약, 주 객체가 대상 객체를 참조하는 방향이지만, 대상 객체 테이블에 Foreign Key를 가지게 설계하려면 양방향 관계로 설계해야함
 

<br>
<br>

## 다대다 [N:N]
@ManyToMany
@JoinTable(name = "연결 테이블 이름", joinColumns = ~, inverseJoinColumns = ~)
<br>

위 어노테이션을 적으면 Jpa가 연결 테이블을 추가해서 일대다, 다대일 관계로 DB를 설정해준다.
> 객체는 List와 같은 컬렉션으로 다대다 관계가 구현 가능하나, 관계형 DB는 테이블 2개로 다대다 관계를 표현할 수 없기 때문에 연결 테이블이 생성된다.

<br><br>

### 다대다 매핑의 문제점
위 어노테이션으로 Jpa가 연결 테이블을 생성하여 매핑은 해주지만, 연결 테이블에 매핑 정보 외에 추가적인 데이터가 들어올 수 없다.
* 이를 해결하려면, 연결 테이블용 Entity를 따로 추가해서 객체 레벨에서 일대다, 다대일 관계로 풀어내야 한다.

  > 다대다 관계는 실무에서 잘 사용되지 않음

<br>
<br>

> 인프런 '김영한'님의 강의를 정리한 내용이므로, 개인 공부 목적으로 작성된 글입니다.
> <br>출처  <https://www.inflearn.com/course/ORM-JPA-Basic>

