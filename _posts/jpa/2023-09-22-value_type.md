---
title: "JPA 값 타입"
excerpt: "JPA 값 타입 필기"

categories:
  - JPA
tags:
  - [Java, Spring, Jpa]

permalink: /categories/jpa/jpa_value_type/

toc: true
toc_sticky: true

date: 2023-09-22
last_modified_at: 2023-09-22
---


## JPA의 데이터 타입
* 엔티티 타입<br>
  @Entity로 정의하는 객체<br>
  식별자가 있다.<br>생명 주기 관리를 가진다.

* 값 타입<br>
  int,Integer,String처럼 단순히 값으로 사용되는 자바 기본 타입<br>
  식별자가 없다.<br>생명주기를 엔티티에 의존한다.<br>공유하지 않는 것이 안전하다.<br>불변 객체로 만드는 것이 안전하다.
  * 기본 값 타입(자바에서 제공하는 기본 primitive Type) (값 복사 메커니즘으로 인한 공유 불가)
  * 임베디드 타입
  * 컬렉션 값 타입

<br><br>

## 임베디드 타입
기본 값 타입을 모아서 만든 복합 값 타입 (사용자가 직접 정의한 타입)

<br>

그럼 JPA가 이런 값을 테이블에 매핑할 때는 어떻게?

정의한 타입(클래스)에 @Embeddable을 표시하고,
사용하는 곳에 @Embedded를 표시

* 기본 생성자 필수

```
@Embeddable
public class Period{

  public Period(){
    //기본 생성자 필수
  }
  private LocalDateTime startDate;
  private LocalDateTime endDate;

  /*사용자가 기본 값 타입을 묶어 직접 정의한 클래스(임베디드 값 타입)*/

}

@Entity
public class Member{
  
  //...
  @Embedded private Period perid   
  //...
}
```

> 임베디드 타입을 포함한 모든 값 타입은, 소유자 Entity에 생명주기를 의존

> 실제 테이블에 어떤 변화가 일어나지는 않는다. 대신 임베디드 값 타입을 사용하면 사용자가 개발 시점에 객체지향적으로 개발할 수 있다는 장점이 있다.

> Tip : 잘 설계한 ORM Application은 매핑한 테이블 수보다 클래스 수가 더 많다.

<br><br>


## 불변 객체

### 값 타입 공유 참조의 문제
임베디드 값 타입 같은 **레퍼런스**를 여러 Entity가 공유할 때 주의 (primitive Type이 아닌 reference Type에 대한 주의점)
> 관련된 테이블의 임베디드 값의 필드가 모두 바뀐다. 

<br>

### Reference 형식의 값 타입은 불변 객체로
**불변 객체**<br>
위 문제점을 원천에 차단하는 객체,<br>
생성 시점 이후 절대 값을 변경할 수 없는 객체
  > Setter가 없는 객체

* 값 타입은 불변 객체로 설계해야함


<br><br>

### Reference Type의 값 비교 

* 동일성 비교(인스턴스의 참조 값을 비교) (==)
* 동등성 비교(인스턴스의 값 비교) (equlas(), hashCode() 재정의해서 사용)

<br><br>



## 값 타입 컬렉션
Ex: List < String >
<br>

컬렉션은 관계형 DB에 어떻게 매핑하여 저장하는가?

> 해당 Entity의 PK값과 값 타입 필드를 모두 PK로 묶어 별도의 테이블에 다대일 관계로 풀어낸다. 

<br>

Jpa로 매핑하는 방법
```
//...
@ElementCollection
@CollectionTable(name = "별도로 생성할 테이블 이름", joinColumns = @JoinColumn(name = "해당 Entity의 PK 필드명"))
@Column(name = "별도 테이블에 저장할 Column 이름")
private Set<String> myHashSet = new HashSet<>();


@ElementCollection
@CollectionTable(name = "tableName", joinColumns = @JoinColumn(name = "해당 Entity의 PK 필드명"))
private List<임베디드 값 타입> myList = new ArrayList<>();

//...
```
* 이렇게 생성된 별도의 테이블이 '다', 기존의 Entity가 '일' 관계이고, 별도의 테이블이 기존의 Entity를 가리키는 단방향 연관관계로 풀어냄

* 값 타입 컬렉션을 따로 persist 하지 않아도 JPA가 영속상태로 처리
  > 이유는 값 타입은 모두 종속 Entity에 생명주기를 의존하기 때문이다. (영속성전이 + 고아객체 기능을 가진다.)

* 값 타입 컬렉션은 지연로딩으로 설정됨

<br><br>

## 값 타입 수정
ReferenceType의 경우, Setter로 값을 수정하지말고, 완전히 새로운 값을 만들어 교체해야 한다.

<br>

## 값 타입 컬렉션 수정
컬렉션에 Remove()를 이용해서 기존의 값을 지우고, add()로 새로 추가해야한다. 

> 값 타입 컬렉션에 변경 사항이 생기면, 해당 Entity와 연관된 모든 데이터를 삭제하고, 현재 값 타입 컬렉션에 있는 값으로 모두 insert해 저장한다. 이러한 이유로 값 타입 컬렉션은 권장되지 않음

<br>
따라서 이러한 문제의 대안으로 컬렉션 대신에 값 타입을 담은 별도의 Entity를 만들고 여기에 일대다 단방향 관계를 풀어내서 사용한다.

<br>


```
//...
@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
@JoinColumn(name = "매핑할 Entity의 PK")
private List<컬렉션을 대체하기 위해 만든 Entity> entityList = new ArrayList<>();
//...
```



<br><br>

* 특정 Entity에 속해 있는 값 타입은 따로 persis하지 않아도 영속성 상태로 관리된다. (영속성 전이)

* 값 타입 컬렉션과 매핑되는 테이블은 모든 Column을 묶어 PK로 두기 때문에, null이나 중복저장이 안된다.

* 값 타입 컬렉션은 DB에서 data에 대한 추적이 어렵기 때문에 권장되지 않는다.

* 값 타입은 변경이 어렵다. 

* 식별자가 필요하고, 지속해서 값을 추적하고 변경해야하면 값 타입이 아닌 Entity를 사용해야 한다.


<br>
<br>

> 인프런 '김영한'님의 강의를 정리한 내용이므로, 개인 공부 목적으로 작성된 글입니다.
> <br>출처  <https://www.inflearn.com/course/ORM-JPA-Basic>
