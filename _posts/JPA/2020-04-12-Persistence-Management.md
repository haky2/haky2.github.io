---
title: 영속성 관리
tags: 
    - jpa
    - persistence management
categories: JPA
layout: post
---

## 엔티티 매니저 팩토리와 엔티티 매니저
- 엔티티 매니저 팩토리는 여러 스레드가 동시에 접근해도 안전하므로 스레드 간에 공유해도 된다
- 엔티티 매니저는 여러 스레드가 동시에 접근하면 동시성 문제가 발생하므로 스레드 간에 절대 공유하면 안 된다
- 엔티티 매니저는 데이터베이스 연결이 꼭 필요한 시점까지 커넥션을 얻지 않는다 (ex.트랜잭션을 시작할 때)

 ---

## 영속성 컨텍스트
#### 영속성 컨텍스트란?
- 엔티티를 영구 저장하는 환경
- 엔티티 매니저를 통해서 영속성 컨텍스트에 접근 및 관리 할 수 있다
- 여러 엔티티 매니저가 같은 영속성 컨텍스트에 접근할 수도 있다

### 엔티티의 생명주기
![엔티티 생명주기](https://user-images.githubusercontent.com/35331310/79059442-0dc28c80-7cb5-11ea-8433-2cb064861c4a.png)

|상태|설명|
|---|---|
|비영속 (new/transient)|- 영속성 컨텍스트와 전혀 관계가 없는 상태<br/>- 엔티티 객체를 생성한 시점(저장 전)|
|영속 (managed)|- 영속성 컨텍스트가 관리하는 엔티티<br/>- 영속성 컨텍스트에 저장된 상태|
|준영속 (detached)|- 영속성 컨텍스트에 저장되었다가 분리된 상태<br/>- 영속성 컨텍스트에 의해 관리되었다가 더 이상 관리되지 않는 엔티티|
|삭제 (removed)|- 삭제된 상태|


### 영속성 컨텍스트의 특징
#### 영속성 컨텍스트와 식별자 값
- 영속성 컨텍스트는 엔티티를 식별자(@Id로 기본 키와 매핑한 값) 값으로 구분한다
- `영속 상태는 식별자 값이 반드시 존재해야 한다`

#### 영속성 컨텍스트와 데이터베이스 저장
- flush : JPA는 보통 트랜잭션을 커밋하는 순간 영속성 컨텍스트에 새로 저장된 엔티티를 데이터베이스에 반영한다

---

### 영속성 컨텍스트의 장점
#### 1. 1차 캐시
- 영속 상태의 엔티티는 모두 이곳에 저장된다
- 데이터 조회시 1차 캐시에서 먼저 엔티티를 찾고 없다면 데이터베이스에서 조회한다 (1차 캐시에 저장한 후에 엔티티 반환)

#### 2. 동일성 보장
- 1차 캐시에 있는 같은 엔티티 인스턴스를 반환하여 동일성을 보장한다

#### 3. 트랜잭션을 지원하는 쓰기 지연
- 엔티티 매니저는 트랜잭션을 커밋하기 직전까지 데이터베이스에 엔티티를 저장하지 않고 내부 쿼리 저장소에 모아둔다
- 트랜잭션을 커밋할 때 모아둔 쿼리를 데이터베이스에 보낸다

#### 4. 변경 감지
- 기존 데이터 수정 방식의 문제점은 비즈니스 로직을 분석하기 위해 SQL을 계속 확인해야 하며, 수정 쿼리가 많아진다. 직/간접적으로 비즈니스 로직이 SQL에 의존하게 된다
- 스냅샷 : JPA가 엔티티를 영속성 컨텍스트에 보관할 때(조회), 최초 상태를 복사해서 저장해둔 것
- flush가 호출되면 엔티티와 스냅샷을 비교하여 변경된 엔티티를 찾고, 변경된 엔티티가 있다면 수정 쿼리를 생성하여 쓰기 지연 SQL 저장소로 보내어 최종 트랜잭션 커밋을 한다
- `변경 감지는 영속성 컨텍스트가 관리하는 영속 상태의 엔티티에만 적용된다`
- JPA의 변경 감지 기본 전략은 엔티티의 모든 필드를 업데이트한다
    - 모든 필드를 사용하면 수정 쿼리가 항상 같아서, 애플리케이션 로딩 시점에 수정 쿼리를 미리 생성해두고 재사용할 수 있다
    - 데이터베이스에 동일한 쿼리를 보내면 데이터베이스는 이전에 한 번 파싱된 쿼리를 재사용할 수 있다
- 필드가 많거나 저장되는 내용이 너무 크면 수정된 데이터만 사용해서 동적으로 update sql을 생성하는 전략을 선택하면 된다 (@DynamicUpdate)
    - 컬럼이 30개 이상이 되면 동적 수정 쿼리가 빠르다고 한다 (성능 측정 후 사용)
    
#### 5. 지연 로딩

---

### 플러시
#### 플러시 호출 방법
- 직접 호출
- 트랜잭션 커밋 시 플러시 자동 호출
- JPQL 쿼리 실행 시 플러시 자동 호출

#### 플러시 모드 옵션
- FlushModeType.AUTO: 커밋이나 쿼리를 실행할 때 플러시 (기본값)
- FlushModeType.COMMIT: 커밋할 떄만 플러시

---

### 준영속
- 준영속 상태의 엔티티는 영속성 컨텍스트가 제공하는 기능을 사용할 수 없다 (1차 캐시 및 관련 SQL 제거)

#### 준영속 상태로 만드는 방법
- em.detach(entity): 특정 엔티티만 준영속 상태로 전환
- em.clear(): 영속성 컨텍스트를 완전히 초기화
- em.close(): 영속성 컨텍스트를 종료

#### 준영속 상태를 다시 영속상태로 변경하는 방법
- em.merge(entity): 준영속 상태의 엔티티를 받아서 그 정보로 새로운 영속 상태의 엔티티를 반환
- merge는 비영속 엔티티도 영속 상태로 만들 수 있다.
- 영속성 컨텍스트 조회 > 데이터 베이스 조회 > 새로운 엔티티를 생성해서 병합
- 병합은 식별자 값으로 엔티티를 조회할 수 있으면 불러서 병합하고 조회할 수 없음변 새로 생성해서 병합한다.
<br/>따라서 병합은 save or update 기능을 수행한다