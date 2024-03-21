---
title: "JPA 연관관계 매핑 "
excerpt: "JPA 연관관계 매핑에 대한 정리 글"

categories:
  - JPA
tags:
  - [Java, Spring, Jpa]

permalink: /categories/jpa/entity_mapping1/

toc: true
toc_sticky: true

date: 2023-09-16
last_modified_at: 2023-09-16
---


# 연관관계 매핑

<br>

### 공부를 위한 예제
* Member와 Team이라는 Entity들이 있다.
* Member는 하나의 Team에만 소속될 수 있다. (연관관계)
* Member와 Team은 다대일 단방향 관계이다. (하나의 Team에 다수의 Member가 소속된다.)
<br>

> 객체가 객체를 참조하는 방법으로 연관관계를 매핑하는 방법을 알아보자.

<br>
<br>


!["객체 지향 모델링"](/assets/images/posts_img/jpa/entity_mapping1.png)

```
@Entity
public class Member{

  @Id @GeneratedValue
  private Long id;

  @Column(name = "USERNAME")
  private String name;

  //@Column(name = "TEAM_ID")
  //private Long teamId;       //이 방법은 FK로 연관관계를 직접 매핑하는 방법

  @ManyToOne    //다대일 관계
  @JoinColumn(name = "TEAM_ID")
  private Team team;

}


  //  ...   실행 가능한 메인 클래스라고 가정 (초기화 코드 및 Team 클래스 생성 과정 생략)

  //팀 저장
  Team team = new Team();
  team.setName("TeamA");
  entityManager.persist(team);

  //회원 저장
  Member member = new Member();
  member.setName("member1);
  member.setTeam(team);   //단방향 연관관계 설정, 참조 저장
  entityManager.persist(member);

```
<br>
위와 같은 방식으로 참조를 통해 연관관계를 조회할 수 있다.
<br><br>


## 객체와 테이블의 관계 구성의 차이
양방향 연관관계를 공부하기 전에, 객체와 테이블의 연관관계 구성의 차이를 알아야한다. <br>
위 예제가 양방향 연관관계라고 가정하면, Member에서 Team으로의 연관관계 1개와 Team에서 Member으로의 연관관계 1개가 있어서 객체 레벨에서 보면 연관관계가 2개가 나온다.<br>
그러나 실제 DB에 저장되는 테이블을 살펴보면 연관관계는 1개로 충분하다.
> Foreign Key 하나로 두 테이블의 연관관계를 관리하기 때문(양쪽으로 Join 가능)
* 양방향이라는 것은 객체 레벨에서 참조되는 객체의 필드에 대상 객체의 참조를 추가하는 것 뿐이다.

<br><br>

## 양방향 연관관계
양방향 연관관계를 사용하여, 객체 Team에서 Member를 찾을 수 있게 만드려면? <br>


* Team에 member List 필드를 갖게 한다.

!["양방향 매핑"](/assets/images/posts_img/jpa/entity_mapping2.png)

```
//위에서 생략한 Team 클래스
@Entity
public class Team{    

  @Id @GeneratedValue
  private Long id;

  private String name;


  //매핑되는 Team 입장에서는 일대다 관계
  //mappedBy 속성을 사용해 Member의 "team" 필드에 의해 매핑되었다고 설정
  @OneToMany(mappedBy = "team")
  List<Member> members = new ArrayList<>();

}
```

<br><br>

## 연관관계 주인
양방향 관계에서는 **연관관계 주인**이라는 개념이 중요하다.<br>

만약 객체 둘 중 하나에 변동사항이 생기면, 테이블 안에서 연관관계를 관리하는 FK를 누구 기준으로 업데이트 해야하나???

> 객체 둘 중 **하나**가 외래 키를 관리해야 한다.

* 주인이 아닌쪽은 참조 필드를 수정할 수 없음, 주인의 참조 필드만이 테이블의 FK를 업데이트 가능

> 따라서 객체의 양방향 연관관계는 연관관계의 주인을 설정해줘야 한다.

<br>

**Tip : 연관관계 주인은 테이블에 FK를 가지고 있는 곳으로 지정하는 것이 좋다.** <br>

> 반대로 연관관계 주인을 설정할 경우, 객체의 변동사항에 대해 반대편 테이블에서 Update Query가 나가는 것을 볼 수 있다. 이것이 나중에 유지보수를 헷갈리게 한다.  

<br>
<br>




## 양방향 매핑 규칙

1. 객체의 두 관계중 하나를 연관관계의 주인으로 설정
2. **연관관계의 주인이 외래 키를 관리(등록, 수정)**
3. **주인이 아닌쪽은 읽기만 가능**
4. 주인은 mappedBy 사용 X
5. 주인이 아니면 mappedBy로 주인 지정

<br><br>

## 양방향 매핑 시 주의점

연관관계의 주인에게 참조 값(연관관계 대상)을 반드시 입력(초기화)해야 한다.<br> 포스트를 위한 시나리오에서 Team의 Member 리스트 필드에 member를 추가해봤자, 실제 테이블에 아무런 영향이 없다. <br>(실제 DB에서 Team 테이블에 Member를 유지하지 않기 때문)<br><br>


* Tip : 객체지향적으로 개발을 할 때는, 양쪽에 다 값을 지정하자.<br> 

> 중간에 1차 캐시를 업데이트 하는 코드를 명시적으로 넣지 않은 상태에서 객체(주인X)의 변동이 있으면, 1차 캐시에서 그대로 가져오기 때문에 업데이트 되지 않은 기존의 값을 불러올 위험이 있다.<br>
> DB에서 조회하는 것이 아닌, 1차 캐시에서 불러오는 순수 객체 상태를 고려하려면, 양쪽에 값을 설정하는 습관을 들이는 것이 좋다. (개발 단계에서 에러방지를 위해서)

```
team.getMembers().add(Member);

//Update Query와 1차 캐시 초기화 주석처리
//em.flush();  
//em.clear();   
//  위 3줄이 주석이 안되어 있으면, DB에서 새롭게 조회 했을 것이다. (이 경우 업데이트가 되지 않았다.)


Team findTeam = em.find(Team.class, team.getId()); 
List<Member> members = findTeam.getMembers();

for(Member m : members){
  System.out.println("m = " + m.getName());
}     //값이 나오지 않음
```
<br>

#### 정리
* 테이블은 Foregin Key 하나로 양쪽 Join이 가능해서, 방향이라는 개념이 없다.
* 객체는 참조용 필드가 있는 쪽으로만 참조 가능하다. 한쪽만 참조하면 단방향, 양쪽으로 참조하면 양방향이다.
* 연관관계의 주인은 외래 키를 관리하는 테이블과 매핑된 객체의 참조이다.
* 참조되는 객체의 참조 변동사항은 DB 테이블에 영향을 주지 않는다. 

#### Tip
* 우선 단방향으로 모두 설계하고, 나중에 필요할 때 객체에서의 양방향 관계만 추가하면 된다. (추가해도 테이블에는 영향을 미치지 않는다.)
* 외래 키를 갖고 있는 객체를 기준으로 연관관계 주인을 설정한다.


<br>
<br>

> 인프런 '김영한'님의 강의를 정리한 내용이므로, 개인 공부 목적으로 작성된 글입니다.
> <br>출처  <https://www.inflearn.com/course/ORM-JPA-Basic>