JPA란 무엇인가?
JPA (Java Persisitence API)는 자바 진영의 ORM 기술 표준.

어플리케이션과 JDBC 사이에서 동작



ORM (Object Relational Mapping)
객체와 테이블을 매핑해서 패러다임의 불일치를 개발자 대신 해결해준다.

객체를 저장하는 코드
jpa.persist(member);    // 저장


객체를 조회하는 코드
Member member = jpa.find(memberId); // 조회


1.3.1 JPA 소개
EJB 3.0에서 하이버네이트를 기반으로 새로운 자바 ORM 기술 표준이 만들어졌는데 이것이 바로 JPA



버전별 특징
버전	연도	설명
JPA 1.0(JSR 220)	2006년	초기 버전, 복합 키와 연관관계 기능 부족
JPA 2.0(JSR 317)	2009년	대부분의 ORM 기능 포함, JPA Criteria가 추가
JPA 2.1(JSR 338)	2013년	스토어드 프로시저 접근, 컨버터, 엔티티 그래프 기능 추가
1.3.2 왜 JPA를 사용해야 하는가?
생산성
JPA를 자바 컬렉션에 객체를 저장하듯 JPA에게 저장할 객체를 전달.
INSERT SQL을 작성하고 JDBC API 사용하는 지루하고 반복적인 일을 JPA가 대신 처리해준다.
CREATE TABLE같은 DDL문 자동 생성
데이터베이스 설계 중심의 패러다임을 객체 설계 중심으로 역전
jpa.persist(member);    // 저장
Member member = jpa.find(memberId);     // 조회
유지보수
엔티티에 필드 추가시 등록, 수정, 조회 관련 코드 모두 변경
JPA를 사용하면 이런 과정을 JPA가 대신 처리
개발자가 작성해야 할 SQL과 JDBC API 코드를 JPA가 대신 처리해줌으로 유지보수해야 하는 코드 수가 줄어든다.
패러다임 불일치 해결
상속, 연관관계, 객체 그래프 탐색, 비교하기 같은 패러다임 불일치 해결
성능
다양한 성능 최적화 기회 제공
어플리케이션과 데이터베이스 사이에 존재함으로 여러 최적화 시도 가능
데이터 접근 추상화와 벤더 독립성
데이터베이스 기술에 종속되지 않도록 한다.
데이타베이스를 변경하면 JPA에게 다른 데이터베이스를 사용한다고 알려주면 됨.


표준
자바 진영의 ORM 기술 표준


Reference
https://ultrakain.gitbooks.io/jpa/content/chapter1/chapter1.3.html
