# JPA  시작하기

생성일: 2021년 12월 18일 오후 8:19

JPA 설정 하기 - persistence.xml

- JPA 설정파일
- /META-INF/persistence.xml 위치
- persistence-unit name으로 이름 저장
- javax.persistence로 시작 : JPA 표준 속성
- hibernate로 시작  : 하이버네이트 전용 속성

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<persistence version="2.2" 
 xmlns="http://xmlns.jcp.org/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd"> 
 <persistence-unit name="hello"> 
 <properties> 
 <!-- 필수 속성 --> 
 <property name="javax.persistence.jdbc.driver" value="org.h2.Driver"/> 
 <property name="javax.persistence.jdbc.user" value="sa"/> 
 <property name="javax.persistence.jdbc.password" value=""/> 
 <property name="javax.persistence.jdbc.url" value="jdbc:h2:tcp://localhost/~/test"/> 
 <property name="hibernate.dialect" value="org.hibernate.dialect.H2Dialect"/> 
<!--<property name="hibernate.dialect" value="org.hibernate.dialect.MySQL.5Dialect"/>  -->
<!--<property name="hibernate.dialect" value="org.hibernate.dialect.Oracle10gDialect"/>  -->
 
 <!-- 옵션 --> 
 <property name="hibernate.show_sql" value="true"/> 
 <property name="hibernate.format_sql" value="true"/> 
 <property name="hibernate.use_sql_comments" value="true"/> 
 <!--<property name="hibernate.hbm2ddl.auto" value="create" />--> 
 </properties> 
 </persistence-unit> 
</persistence>
```

데이터 베이스 방언

- JPA는 특정 데이터베이스에 종속 되지 않는다.
- 각각의 데이터베이스가 제공하는 SQL 문법과 함수는 조금씩다르다
    - 가변 문자 : MySQL은 VARCHAR, Oracle은 VARCHAR2
    - 문자열을 자르는 함수 : SQL 표준은 SUBSTRING(), Oracle은 SUBSTR()
    - 페이징  : MySQL은 LIMIT, Oracle은 ROWNUM
- 방언 : SQL 표준을 지키지 않는 특정 데이터 베이스만의 고유한 기능

## JPA 간단한 실습 진행

- EntityManagerFactory는 하나만 생성해서 어플리케이션 전체에서 공유해서 사용한다.
- EntityManger 쓰레드간에 공유가 되지 않는다. 사용하고 버려야한다.
- EntityTransaction 모든 데이터는 trasaction안에서 실행되어야 한다.

DB 생성

```sql
create table Member(
	id bigint not null,
	name varchar(255),
	primary key(id)
);
```

Entity

```java
@Entity
public class Member {
    @Id
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

JPA Main 실행

```java
public class JpaMain {
    public static void main(String[] args){
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
        EntityManager em = emf.createEntityManager();

        EntityTransaction tx = em.getTransaction();
        tx.begin();
        try{

           // ..code insert / udpate/ delete

            tx.commit();
        }catch (Exception e){
            tx.rollback();
        }finally{
            em.close();
        }

        em.close();
        emf.close();
    }
}
```

JPA를 활용해서 insert / update / delete

```sql
// member insert
   Member member = new Member();
   member.setId(2L);
   member.setName("HelloB");
   em.persist(member);

// member update
// JPA를 통해서 Entity를 가져오면 JPA가 관리한다. transactio commit 하는 시점에 값이 변경되면 update 쿼리를 만들어서 날린다.
   Member findMember = em.find(Member.class, 1L);
   findMember.setName("HelloJPA");

// member delete
   Member findMember = em.find(Member.class, 1L);
   em.remove(findMember);
```

JPQL 간단한 소개 및 사용

- 테이블이 아닌 객체를 대상으로 검색하는 객체 지향 쿼리
- SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않는다(데이터 베이스 방언)
- 객체지향 SQL / 엔티티 객체를 대상으로 쿼리

```java
 // JPQL
            List<Member> result = em.createQuery("select m from Member as m", Member.class)
                    .setFirstResult(1)
                    .setMaxResults(8)
                    .getResultList();

            for(Member member : result ){
                System.out.println("member.name= " + member.getName());
            }
```