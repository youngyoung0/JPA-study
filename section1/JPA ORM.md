# JPA ORM

생성일: 2021년 12월 11일 오후 9:04

JPA

- Java Persistence API
- ORM 기술 표준

ORM

- Object-relational mapping(객체 관계 매핑)
- 객체는 객체대로 설계
- 관계형 데이터베이스는 관계형 데이터 베이스대로 설계
- ORM 프레임워크가 중간에서 매핑
- 대중적인 언어에는 대부분 ORM 기술이 존재

JPA를 왜 사용하는가

- 생산성
    - 저장 : jap.persist(member)
    - 조회 : Member member = jpa.find(memberId
    - 수정 : member.setName(”변경할 이름”)
    - 삭제 : jpa.remove(member)
- 유지보수
    - JPA : 필드만 추가하면된다, SQL은 JPA가 처리한다.
- 패러다임의 불일치 해결
    - JPA와 상속
    - JPA와 연관관계, 객체 그래프 탐색
    - JPA와 비교하기
- 성능
    - 1차 캐시와 동일성 보장
        - 같은 트랜잭션 안에서는 같은 엔티티를 반환 - 약간의 조회 성능 향상
        
        ```java
        String memberId = "100";
        Member m1 = jpa.find(Member.class, memberId); // SQL
        Member m2 = jpa.find(Member.class, memberId); // 캐시
        System.out.println(m1 == m2) // true
        ```
        
    - 트랜잭션을 지원하는 쓰기 지연
        - 트랜잭션을 커밋할 때까지 insert SQL을 모음
        - JDBC BATCH SQL 기능을 사용해서 한번에 SQL 전송
        
        ```java
        transaction.begin(); // 트랜잭션 시작
        
        em.persist(memberA);
        em.persist(memberA);
        em.persist(memberA);
        // 여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.
        ...
        // 커밋하는 순간 데이터베이스에 INSERT SQL을 모아서 보낸다.
        transaction.commit(); // 트랜잭션 커밋
        ```
        
    - 지연 로딩
        - 지연로딩 : 객체가 사용될 뗴 로딩
        - 즉시 로딩 : JOIN SQL로 한번에 연관된 객체까지 미리 조회