N+1 쿼리 문제의 원인 ?
Spring Data JPA에서 제공하는 Repository의 ‘findAll()’, ‘findById()’ 등과 같은 메소드를 사용하면 바로 DB에 SQL 쿼리를 날리는 것이 아닙니다.

참조 : findAll() vs findById() 내부 동작 차이

JPQL이라는 객체지향 쿼리 언어를 생성, 실행시킨 후 JPA는 이것을 분석해서 SQL을 생성, 실행하는 동작에서 N+1 쿼리 문제가 발생합니다.

JPQL 입장에서는 LAZY 로딩, EAGER 로딩과 같은 글로벌 패치 전략을 신경쓰지 않고 JPQL만 사용해서 SQL을 생성합니다.

JPQL이란 ?
JPQL이란 플랫폼에 독립적인 객체지향 쿼리 언어입니다.
자바 코드에서 데이터베이스를 조회할 때 특정 SQL이나 저장 엔진에 종속되지 않게 도와줍니다.

N+1 쿼리 문제는 언제 발생할까 ?
발생하는 경우는 다음과 같은 2가지 경우가 있습니다.

두 개의 엔티티가 1:N 관계를 가지며 JPQL로 객체를 조회할 때

EAGER 전략으로 데이터를 가져오는 경우
LAZY 전략으로 데이터를 가져온 이후에 가져온 데이터에서 하위 엔티티를 다시 조회하는 경우






https://wwlee94.github.io/category/blog/spring-jpa-n+1-query/
