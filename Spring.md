### 스프링 프레임워크란
* 자바 엔터프라이즈 개발을 편하게 해주는 경량급 오픈소스 애플리케이션 프레임워크
* **Lightweight Java Applicaion Framework**
    * 목표: **POJO 기반의** Enterprise Application 개발을 쉽고 편하게 할 수 있도록 한다.
    * Java Application을 개발하는데 필요한 하부구조(Infrastructure)를 포괄적으로 제공한다.
    * Spring이 하부구조를 처리하기 때문에 개발자는 Application 개발에 집중할 수 있다.
* 간단히 스프링(Spring)이라고도 불린다. 
* 동적인 웹 사이트를 개발하기 위한 여러 가지 서비스를 제공한다.
* 대한민국 공공기관의 웹 서비스 개발 시 사용을 권장하고 있는 전자 정부 표준 프레임워크의 기반 기술


> - [https://gmlwjd9405.github.io/2018/10/26/spring-framework.html](https://gmlwjd9405.github.io/2018/10/26/spring-framework.html)
### Spring Spring MVC Spring Boot의 차이
* Spring
    * 
* Spring MVC
* Spring Boot


> - [http://blog.naver.com/PostView.nhn?blogId=sthwin&logNo=221271008423&parentCategoryNo=&categoryNo=50&viewDate=&isShowPopularPosts=true&from=search](http://blog.naver.com/PostView.nhn?blogId=sthwin&logNo=221271008423&parentCategoryNo=&categoryNo=50&viewDate=&isShowPopularPosts=true&from=search)
### Bean이란
* 컨테이너 안에 들어있는 객체
* 컨테이너에 담겨있으며, 필요할 때 컨테이너에서 가져와서 사용
* @Bean 을 사용하거나 xml 설정을 통해 일반 객체를 Bean으로 등록할 수 있고, Bean으로 등록된 객체는 쉽게 주입하여 사용 가능

#### Bean 생명주기
- 객체 생성 -> 의존 설정 -> 초기화 -> 사용 -> 소멸
- 스프링 컨테이너에 의해 생명주기 관리
- 스프링 컨테이너 초기화 시 빈 객체 생성, 의존 객체 주입 및 초기화
- 스프링 컨테이너 종료 시 빈 객체 소멸

#### Bean 초기화 방법 3가지
1. 빈 초기화 메소드에 ```@PostConstruct``` 사용
  - 빈 정의 xml에 ```<context:annotation-config></context:annotation-config>``` 추가
2. ```InitializingBean``` 인터페이스의 ```afterPropertiesSet()``` 메소드 오버라이드
3. 커스텀 init() 메소드 정의
  - 빈 정의 xml에 ```init-method``` 속성으로 메소드 이름 지정
  - 또는 빈 초기화 메소드에 ```@Bean(init-method="init")``` 지정

#### Bean 소멸 방법 3가지
1. 빈 소멸 메소드에 ```@PreDestroy``` 사용
  - 빈 정의 xml에 ```<context:annotation-config></context:annotation-config>``` 추가
2. ```DisposableBean``` 인터페이스의 ```destroy()``` 메소드 오버라이드
3. 커스텀 destroy() 메소드 정의
  - 빈 정의 xml에 ```destroy-method``` 속성으로 메소드 이름 지정

##### 권장하는 방법
- 1번 방법 (권장)
  - 사용 방법이 간결하며 코드에서 초기화 메소드가 존재함을 쉽게 파악 가능하여 xml 설정 방법보다 직관적
- 2번 방법 (지양)
  - 빈 코드에 스프링 인터페이스가 노출되어 권장하지 않으며 간결하지 않은 방법
- 3번 방법
  - 빈 코드에 스프링 인터페이스는 노출되지 않지만, 코드만으로 초기화 메소드 호출 여부를 알 수 없는 단점

#### Bean Scope
* **singleton (default)**
  * 애플리케이션에서 Bean 등록 시 singleton scope로 등록
  * Spring IoC 컨테이너 당 한 개의 인스턴스만 생성
  * 컨테이너가 Bean 가져다 주입할 때 항상 같은 객체 사용
  * 메모리나 성능 최적화에 유리
* **prototype**
  * 컨테이너에서 Bean 가져다 쓸 때 항상 다른 인스턴스 사용
  * 모든 요청에서 새로운 객체 생성
  * gc에 의해 Bean 제거
* request
  * Bean 등록 시 하나의 HTTP request 생명주기 안에 단 하나의 Bean만 존재
  * 각각의 HTTP 요청은 고유 Bean 객체 보유
  * Spring MVC Web Application에서 사용
* session
  * 하나의 HTTP Session 생명주기 안에 단 하나의 Bean만 존재
  * Spring MVC Web Application에서 사용
* global session
  * 하나의 global HTTP Session 생명주기 안에 한 개의 Bean 지정
  * Spring MVC Web Application에서 사용
* application
  * ServletContext 생명주기 안에 한 개의 Bean 지정
  * Spring MVC Web Application에서 사용


> - [[Spring] IOC(Inversion Of Control): 제어 역전](https://velog.io/@max9106/Spring-IOC%EB%AF%B8%EC%99%84)
> - [[Spring] Spring Bean의 개념과 Bean Scope 종류](https://gmlwjd9405.github.io/2018/11/10/spring-beans.html)
> - [Spring 빈/컨테이너 생명주기 (Lifecycle)](https://flowarc.tistory.com/entry/Spring-%EB%B9%88%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0-Lifecycle)
> - [SpringMVC :: 스프링 컨테이너의 생명주기, 빈의 생명주기 (Life cycle), InitialzingBean, DisposableBean, @PreDestroy, @PostConstruct](https://hongku.tistory.com/106)
> - [[Spring] 빈(bean)생명주기 메소드](https://cornswrold.tistory.com/100)
