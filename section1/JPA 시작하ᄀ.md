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