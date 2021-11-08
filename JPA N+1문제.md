# N+1 쿼리 문제의 원인 ?
Spring Data JPA에서 제공하는 Repository의 ‘findAll()’, ‘findById()’ 등과 같은 메소드를 사용하면 바로 DB에 SQL 쿼리를 날리는 것이 아닙니다.

참조 : findAll() vs findById() 내부 동작 차이

JPQL이라는 객체지향 쿼리 언어를 생성, 실행시킨 후 JPA는 이것을 분석해서 SQL을 생성, 실행하는 동작에서 N+1 쿼리 문제가 발생합니다.

JPQL 입장에서는 LAZY 로딩, EAGER 로딩과 같은 글로벌 패치 전략을 신경쓰지 않고 JPQL만 사용해서 SQL을 생성합니다.

## JPQL이란 ?
JPQL이란 플랫폼에 독립적인 객체지향 쿼리 언어입니다.
자바 코드에서 데이터베이스를 조회할 때 특정 SQL이나 저장 엔진에 종속되지 않게 도와줍니다.

# N+1 쿼리 문제는 언제 발생할까 ?
발생하는 경우는 다음과 같은 2가지 경우가 있습니다.

- 두 개의 엔티티가 1:N 관계를 가지며 JPQL로 객체를 조회할 때

- EAGER 전략으로 데이터를 가져오는 경우
LAZY 전략으로 데이터를 가져온 이후에 가져온 데이터에서 하위 엔티티를 다시 조회하는 경우


## 엔티티 모델링

1:N 관계를 만들기 위해 하나의 앨범(Album)이 많은 노래(Song)를 가질 수 있도록 엔티티를 생성하고 관계를 연결시키겠습니다.

Album Model

Java

@Entity public class Album { @Id @GeneratedValue(strategy = GenerationType.IDENTITY) private long id; @Column(nullable = false) private String albumTitle; @Column(nullable = false) private String locales; // @OneToMany(mappedBy = "album", cascade = CascadeType.ALL, fetch = FetchType.EAGER) // 2번 상황 @OneToMany(mappedBy = "album", cascade = CascadeType.ALL, fetch = FetchType.LAZY) // 1번 상황 private List<Song> songs = new ArrayList<>(); }

Song Model

Java

@Entity public class Song { @Id @GeneratedValue(strategy = GenerationType.IDENTITY) private long id; @Column(nullable = false) private String title; @Column(nullable = false) private int track; @Column(nullable = false) private int length; @ManyToOne(fetch = FetchType.LAZY) @JoinColumn(name = "album_id") private Album album; }

Database Diagram

1. 하위 엔티티를 조회하지 않는 경우

Album의 Song에 접근하지 않은 경우

Java

@Test public void N1_쿼리테스트_1() throws Exception{ List<Album> albums = albumRepository.findAll(); }

LAZY 방식 결과

하위 엔티티에 접근하지 않았기 때문에 Album만 가져오는 것을 볼 수 있습니다.

EAGER 방식 결과

N+1 발생 !

JPQL에서 동작한 쿼리를 통해서 Album 데이터를 조회합니다.
그 이후 JPA에서는 글로벌 패치 전략(EAGER 로딩)을 보고 Album의 Song 대해서 추가적인 로딩 작업을 진행해 N+1 문제를 발생시킵니다.

2. 하위 엔티티를 조회하는 경우

LAZY 로딩을 하기 위해서는 해당 Entity가 영속 상태여야 합니다.
보통 Repository에서 리스트로 가져오면 영속이 끊긴 상태로 가져오기 때문에 LAZY 전략 테스팅 시 @Transactional를 꼭 사용해야합니다 !

@Transactional을 사용하지 않으면 다음과 같은 에러가 발생합니다.

Album의 Song에 접근하는 경우

Java

@Test @Transactional // 테스팅에서 LAZY 전략시 필수 public void N1_쿼리테스트_2() throws Exception{ List<Album> albums = albumRepository.findAll(); for (Album album : albums) { System.out.println(album.getSongs().size()); // Song에 접근 ! } }

LAZY 방식 결과

N+1 발생 !

처음엔 Album 리스트만 조회했지만 Album 엔티티에서 하위 엔티티인 Song 엔티티로 접근했기 때문에 LAZY 로딩이 일어나면서 N+1 문제 발생

EAGER 방식 결과

N+1 발생 !

‘하위 엔티티를 조회하지 않는 경우’의 EAGER 방식 결과와 동일하게 N+1 문제가 발생하는 것을 볼 수 있습니다.

N+1 문제 해결 방법


Reference
https://wwlee94.github.io/category/blog/spring-jpa-n+1-query/
